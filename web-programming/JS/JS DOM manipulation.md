`insertAdjacentHTML`
```
zoo.insertAdjacentHTML('beforeend', '<div class="tiger"></div>'); 
```
-   `'beforebegin'` — вставка до открывающего тега;
-   `'afterbegin'` — вставка после открывающего тега;
-   `'afterend'` — вставка после закрывающего тега.

Относительно разметки блока это выглядит так:

```
<!-- beforebegin -->
<div>
    <!-- afterbegin -->
    
    <!-- существующая разметка-->
    
    <!-- beforeend -->
</div>
<!-- afterend --> 
```

Свойство `insertAdjacentText` работает аналогичным образом, только вставляет текст, как и свойство `textContent`.


Существует 5 методов добавления элементов на страницу. Их добавляют относительно других DOM-элементов: перед или за ними, вместо или внутрь.

-   `node.append(...nodes or strings)` — добавляет узлы или строки в конец `node`;
-   `node.prepend(...nodes or strings)` — в начало `node`;
-   `node.before(...nodes or strings)` — до `node`;
-   `node.after(...nodes or strings)` — после `node`;
-   `node.replaceWith(...nodes or strings)` — заменяет `node` заданными узлами или строками.
Метод `closest`. Он возвращает ближайший родительский элемент с переданным селектором.

Когда мы вызываем его на элементе кнопки удаления, то получаем искомый элемент списка, просто передав его класс:

```
// выберем кнопку удаления
const deleteButton = document.querySelector('.todo__item-button');

// добавим обработчик
deleteButton.addEventListener('click', function () {
  const listItem = deleteButton.closest('.todo__item');
  listItem.remove();
}); 
```

## Перемещение элементов

Добавить в DOM можно и элемент, который там уже есть. Тогда элемент удалится с прошлого места и встанет на новое:

```
const list = document.querySelector('.todo-list');

// в свойстве children хранится
// псевдомассив дочерних элементов
const listItems = list.children;

// переместили первый элемент todo-листа в конец
list.append(listItems[0]); 
```

Это справедливо для всех пяти методов добавления: `append`, `prepend`, `before`, `after` и `replaceWith`.

```
// клонировать элемент вместе со всем его содержимым
const deepCopy = elem.cloneNode(true);

// клонирование без дочерних элементов
const shallowCopy = elem.cloneNode(false); 
```

## Тег `template`

Тег `template` — заготовка вёрстки. Её используют для создания элементов. Если добавить `template` в HTML, содержимое тега не отобразится на сайте:

```
<template id="user">
  <div class="user">
    <img class="user__avatar" alt="avatar">
    <p class="user__name"></p>
  </div>
</template> 
```

Зато в JavaScript мы можем получить этот элемент методом `querySelector`:

```
const userTemplate = document.querySelector('#user'); 
```

Чтобы получить содержимое `template`, нужно обратиться к его свойству `content`:

```
const userTemplate = document.querySelector('#user').content; 
```

Теперь этот элемент можно клонировать, наполнить содержимым и вставить в DOM.

```
const userTemplate = document.querySelector('#user').content;
const usersOnline = document.querySelector('.users-online');

// клонируем содержимое тега template
const userElement = userTemplate.querySelector('.user').cloneNode(true);

// наполняем содержимым
userElement.querySelector('.user__avatar').src = 'tinyurl.com/v4pfzwy';
userElement.querySelector('.user__name').textContent = 'Дюк Корморант';

// отображаем на странице
usersOnline.append(userElement); 
```

```
function addSong(artistValue, titleValue) {
  const songTemplate = document.querySelector('#song-template').content;
  const songElement = songTemplate.querySelector('.song').cloneNode(true);

  songElement.querySelector('.song__artist').textContent = artistValue;
  songElement.querySelector('.song__title').textContent = titleValue;
  
  songsContainer.append(songElement);
}
```


## Ссылка на родителя — `parentElement`

Свойство `parentElement` содержит ссылку на родительский элемент:
```
const element = document.querySelector('p');

console.log(element.parentElement); // body, так как это родитель p 
```

## Псевдомассив детей — `children`

Свойство `children` содержит псевдомассив всех дочерних элементов указанного элемента.

Дочерние элементы `body`:

```
/* script.js */

const body = document.querySelector('body');

console.log(body.children); // HTMLCollection(3) [p, p, p] 
```

## Первый и последний ребёнок — `firstElementChild` и `lastElementChild`

Свойства `firstElementChild` и `lastElementChild` позволяют получить первый и последний дочерние элементы:

Первый и последний ребёнок:

```
/* script.js */

const body = document.querySelector('body');

console.log(body.firstElementChild); // <p>Ребёнок раз</p>
console.log(body.lastElementChild); // <p>Ребёнок три</p> 
```

Если у элемента нет дочерних элементов, `firstElementChild` и `lastElementChild` вернут `null`.

## Предыдущий и следующий сосед — `previousElementSibling` и `nextElementSibling`

Свойства `previousElementSibling` и `nextElementSibling` содержат ссылки на предыдущий и следующий соседний элементы:

Предыдущий и следующий сосед:

```
/* script.js */

const element = document.querySelectorAll('p')[1];

console.log(element.previousElementSibling); // <p>Ребёнок раз</p>
console.log(element.nextElementSibling); // <p>Ребёнок три</p>

// если соседа нет — вернётся null
console.log(element.nextElementSibling.nextElementSibling); // null 
```

Все эти свойства доступны только для чтения. Перезаписать их не получится:

```
/* script.js */

const body = document.querySelector('body');

console.log(body.children); // HTMLCollection(3) [p, p, p]
body.children = [];
console.log(body.children); // HTMLCollection(3) [p, p, p] 
```

#javascript