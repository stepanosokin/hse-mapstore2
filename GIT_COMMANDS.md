Git workflow для отдельного репозитория на базе чужого проекта (BSD)

Сценарий:

есть оригинальный репозиторий (upstream)

ты делаешь отдельный репозиторий

хочешь регулярно подтягивать обновления

upstream использует ветку master

у тебя:

main — максимально близок к upstream

my-changes — твои изменения

Целевая схема
upstream/master  → оригинальный проект
origin/main      → твой main
origin/my-changes → твои доработки

1. Клонирование оригинального проекта
git clone https://github.com/ORIGINAL_OWNER/ORIGINAL_REPO.git
cd ORIGINAL_REPO

2. (Опционально) Переименование папки
cd ..
mv ORIGINAL_REPO my-project
cd my-project

3. Создай пустой репозиторий на GitHub

Например:

https://github.com/you/my-project


Важно:
репозиторий должен быть пустым (без README и LICENSE).

4. Переименовываем origin в upstream

Проверка:

git remote -v


Меняем:

git remote rename origin upstream

5. Добавляем свой репозиторий как origin
git remote add origin https://github.com/you/my-project.git


Проверка:

git remote -v


Должно быть:

origin    → https://github.com/you/my-project.git
upstream  → https://github.com/ORIGINAL_OWNER/ORIGINAL_REPO.git

6. Публикуем код в свой репозиторий

Узнай имя ветки:

git branch


Если это master:

git push -u origin master

7. Создаём рабочую ветку для своих изменений
git checkout -b my-changes

8. Как подтягивать обновления из upstream (ветка master)
Обновляем свой master
git checkout master
git fetch upstream
git merge upstream/master

9. Как синхронизировать my-changes с новым master

Рекомендуемый способ — rebase.

git checkout my-changes
git rebase master


Смысл:

твои коммиты аккуратно перекладываются поверх обновлённого master.

Если во время rebase появились конфликты

После исправления файлов:

git add .
git rebase --continue


Если нужно отменить:

git rebase --abort

10. Как пушить my-changes после rebase
git push --force-with-lease origin my-changes


Почему так:

rebase переписывает историю

обычный git push не сработает

--force-with-lease безопаснее, чем --force

11. Как пушить master

master ты не ребейзишь, только мержишь.

git checkout master
git push origin master

12. Полный рабочий цикл

Если upstream использует ветку master:

git checkout master
git fetch upstream
git merge upstream/master

git checkout my-changes
git rebase master
git push --force-with-lease origin my-changes

13. Важные правила
Никогда не делай rebase для master
rebase — только для:
- my-changes
- feature-веток

Не мержи my-changes в master
git checkout master
git merge my-changes   ❌


master должен оставаться максимально близким к upstream.

14. Полезные проверки

Посмотреть локальные ветки и трекинг:

git branch -vv


Посмотреть удалённые ветки:

git branch -r


Ты должен видеть:

upstream/master
origin/main
origin/my-changes

15. Типовая структура работы
main
 └─ регулярно догоняет upstream/master

my-changes
 └─ всегда ребейзится на новый master


Это оптимальная схема для долгоживущего производного проекта.