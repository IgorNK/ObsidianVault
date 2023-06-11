```
.container {
  display: grid;
  grid-template-rows: 100px;
  grid-template-columns: repeat(4, 100px);
  grid-auto-rows: 100px 300px; 
}
/* высота первого трека 100px, 
а второго — 300px, 
высота третьего трека снова — 100px, 
а четвёртого — 300px и т.д. */ 
```

В прошлом уроке вы увидели, что неявные треки добавляются снизу сразу после явных. Этим поведением можно управлять свойством `grid-auto-flow`. По умолчанию его значение `row`. Чтобы вместо строк добавлялись неявные столбцы, нужно его поменять на `column`:

```
.container {
  display: grid;
  grid-template-rows: repeat(2, 100px);
  grid-template-columns: repeat(4, 100px);
  grid-auto-flow: column; /* теперь будут добавляться неявные столбцы */
} 
```

Свойство `grid-auto-columns` управляет размерами этих треков:

```
.container {
  display: grid;
  grid-template-rows: repeat(2, 100px);
  grid-template-columns: repeat(4, 100px);
  grid-auto-flow: column;
  grid-auto-columns: 100px; /* ширина неявных столбцов 100px */
} 
```

У `grid-auto-flow` есть необычное значение `dense`. Представьте, что вы укладываете вещи в поездку оптимально компактно, чтобы они уместились в чемодан с минимумом пустого места. То же произойдёт с элементами сетки, если использовать `dense`.

```
.container {
  display: grid;
  width: 500px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  grid-auto-flow: dense;
} 
```

#css-grid