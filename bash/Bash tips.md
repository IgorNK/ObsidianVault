Команды в терминале необязательно вбивать и выполнять по очереди. Команды можно указывать не по одной, а сразу списком. Для этого их разделяют двумя амперсандами `&&`:

```
mkdir third-project && cd third-project && touch index.html style.css
# создаём папку third-project,
# переходим в папку third-project
# и создаём в ней 2 файла: index.html и style.css. 
```

## Копирование `cp`

После имени команды `cp` указывают два пути:

сначала путь к файлу, который нужно скопировать;

затем путь к папке, в которую нужно сохранить копию.

```
cp что_копируем куда_копируем
cp index.html /src
# копируем index.html в папку src 
```

Можно указать сразу несколько файлов:

```
cp что_копируем что_копируем куда_копируем
cp index.html style.css script.js /src
# копируем 3 файла — index.html, style.css и script.js — в папку src 
```

## Перемещение файлов и папок `mv`

Синтаксис команды `mv` аналогичен команде `cp`. После имени команды указывают список файлов и папок, которые нужно переместить, а затем — папку, в которую нужно сложить файлы.

Создайте папку `button_disabled` внутри директории `third-project`. Для этого вспомните, как перемещаться на уровень выше:

```
pwd
/Users/students-yandex/Documents/dev/third-project/button
# Посмотрели, где мы. Перейдём на один уровень выше
cd ..
pwd
/Users/students-yandex/Documents/dev/third-project
# Отлично!

mkdir button_disabled && ls
# Создали папку и проверили, какие файлы лежат в проекте
button
button_disabled
button.css
button_disabled.css  
```

## Просмотр содержимого `cat`

Команда `cat` выводит в терминал содержимое текстового файла. После команды указывают имя файла, содержимое которого нужно просмотреть:

```
cat index.html
cat ~/Documents/file.txt 
```

У команды `cat` есть настройки, которые указывают ключами. Ключ `-n` включает нумерацию строк:

```
cat -n index.html
# выведем в окно терминала содержимое index.html
   
1 <!DOCTYPE html>
2 <html lang="en">
3 <head>
4  <meta charset="UTF-8">
5  <title>Document</title>
6 </head>
7 <body>
8        
9 </body>
10 </html> 
```

#bash