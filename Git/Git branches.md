Создадим тестовый проект, назовём его `learn_branches`. Внутри него создадим файлы `index.html` и `style.css` и сделаем коммит:

```
# создадим новый проект и инициализируем в нём Git
mkdir learn_branches && cd learn_branches && git init && touch index.html style.css

git add . # добавляет все файлы, вместо этого можно вызвать git add -A
git commit -m "Added initial commit" 
```

После коммита появилась ветка main. Чтобы это проверить, введите:

```
git branch # команда для просмотра веток
* main # наша основная ветка, звёздочкой отмечено в какой вы ветке сейчас 
```

Сделайте ещё один коммит:

```
git add . && git commit -m "Added simple markup" 
```

Мы будем делать шапку сайта. Поскольку это новая функциональность, назовём ветку `feature/header`:

```
pwd # посмотрели, где находимся
/Users/students-yandex/dev/learn_branches

git branch feature/header # создали новую ветку
git branch # проверили, в какой ветке находимся
feature/header # появилась новая ветка
* main # но мы пока находимся в ветке main 
```

Ветку создали, но пока мы находимся в ветке `main`. Чтобы переключиться в ветку `feature/header`, введите команду `git checkout feature/header`:

```
git checkout feature/header # переключились в ветку feature/header
git branch # проверили
* feature/header
main 
```

Можно создать ветку и сразу переключиться на неё командой:

```
git checkout -b feature/header # создали и сразу переключились в ветку feature/header 
```

## Объединение веток

Шапка сайта готова, её ветку пора объединить с веткой `main`. Чтобы объединить, или «смёрджить» ветки, нужно переключиться в ветку, куда должны попасть изменения. В нашем случае это ветка `main`:

```
git checkout main # переключились в main 
```

Все коммиты нужно переместить в ветку `main`. Это делается командой `git merge feature/header`:

```
git merge feature/header
Updating ffd42e2..fe581f6
Fast-forward
 index.html | 4 ++--
 style.css  | 8 ++++++++
 2 files changed, 10 insertions(+), 2 deletions(-) 
```

## Удаление ветки

После того как ветка «влита» в `main`, её можно удалить. Для этого переключитесь на неё командой `git checkout main` и введите команду `git branch` с ключом `-D` и названием ветки:

```
# проверили, где мы
git branch
feature/header
git checkout main

# удалили ветку feature/header
git branch -D feature/header
Deleted branch feature/header (was fe581f6). 
```

#git