## Значение `auto-fill`
```
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, 100px);
} 
```

свойство `auto-fill` стремится заполнить колонками всё доступное пространство. Когда элементы заканчиваются, `auto-fill` создаёт пустые колонки. Их можно увидеть через инспектор в браузере, но особенно они заметны на большом мониторе:

![[Pasted image 20221201193343.png]]

## Значение `auto-fit`

Значение `auto-fit` тоже заполняет всё доступное пространство колонками, но в отличие от `auto-fill` схлопывает пустые и отдаёт больше места под заполненные:

```
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, 100px);
} 
```

![[Pasted image 20221201193401.png]]

_Со свойством repeat(auto-fit, 100px) неявные треки схлопнулись_

При этом пустые колонки всё ещё существуют, просто с нулевым размером.

Для создания такой сетки установите функцией `minmax()` минимальную и максимальную ширину колонок:

```
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
} 
```

Значения `auto-fill` и `auto-fit` имеют схожее поведение. Разницу можно увидеть через функцию `minmax()` при изменении ширины окна браузера.

#css-grid