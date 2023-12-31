-   `pwd` — покажи в какой я папке;
-   `ls` — покажи файлы в папке, где я сейчас;
-   `cd first-project` — перейди в папку `first-project`;
-   `cd first-project/html` — перейди в папку html, находящуюся в папке `first-project`;
-   `cd ..` — перейди на уровень выше в родительскую папку;
-   `cd ~` — перейди в домашнюю директорию (у нас это `/Users/stas_basov`);
-   `mkdir second-project` — в текущей папке создай папку с именем `second-project`;
-   `rm about.html` — удали файл `about.html`;
-   `rmdir images` — удали папку `images`;
-   `rm -r second-project` — удали папку `second-project` и всё, что она содержит;
-   `touch index.html` — создай файл `index.html` в текущей папке;
-   `touch index.html style.css script.js` — если нужно создать несколько файлов, их имена можно вводить через пробел.

Git config:
```
git config --global user.name "Stas Basov" 
# вводите своё имя или ник латиницей и в кавычках

git config --global user.email "stasbasov@yandex.ru"
# здесь нужно ввести свой реальный e-mail 
```
```
git config --list
# вывели в окно командной строки список всех свойств конфига 
```

По умолчанию в вашем локальном репозитории имя основной ветки master, поэтому если в удаленном репозитории используется любое другое имя, например `main` как в «Гитхабе», то вам потребуется переименовать локальную ветку командой:

```
git branch -M main 
```

Чтобы привязать удалённый репозиторий к локальному, нужно воспользоваться командой `git remote add`. Этой команде нужно передать два параметра: имя удалённого репозитория и его адрес (из вкладки SSH). Вот так (только замените SSH на свой):

```
git remote add origin git@github.com:YandexPracticum/first-project.git 
```

С адресом всё понятно, но почему имя репозитория — `origin`, а не `first-project`? Тут всё немного запутано. В данном случае имя репозитория может не совпадать с его именем на «Гитхабе». Этим именем мы будем называть удалённый репозиторий локально при вводе команд в командную строку. Мы могли бы дать имя `first-project`, но имя `origin` — это стандартное имя удалённого репозитория. В будущем это позволит нам опускать его в командах, «Гит» будет по умолчанию искать удалённый репозиторий с именем `origin`.

Репозитории связаны, осталось загрузить код на «Гитхаб». Это делает команда `git push`. Вызовите её вот так:

```
git push -u origin main 
```

#git