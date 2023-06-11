```
.container {
  display: grid;
  grid-template-columns: 100px 100px 150px;
  grid-template-rows: 150px 150px 100px;
  gap: 10px;
} 
```

```
/* вместо этого: */
grid-template-columns: 20% 20% 20% 20% 20%;
/* можно написать: */
grid-template-columns: repeat(5, 20%); 
```

```
grid-template-columns:repeat(6, 1fr); 
```

```
.block_size_big {
  grid-column-start: 1;
  grid-column-end: 3;
} 
```

```
.block_size_big {
  grid-column-start: -1;
  grid-column-end: -3;
  grid-row-start: 1;
  grid-row-end: 6;
} 
```

Чтобы каждый раз не считать линии, им можно давать имена. Цифровые названия линий останутся доступными. Имена линиям указывают в квадратных скобках на этапе создания строк и столбцов:

```
grid-template-rows: [aside-start] 300px [aside-end]; 
```

Чтобы задать элементу расположение, отсчитывая от линии с именем `aside-start`, нужно написать:

```
grid-row: aside-start / 4; 
```
Имя линии может быть любым, кроме ключевого слова `span`. Словом `span` указывают, какое количество строк или столбцов должен занимать элемент до или после какой-то линии. Например:

```
.block {
  grid-column-start: 2;
  grid-column-end: span 2;
  grid-row-start: span 2;
  grid-row-end: 3;
} 
```

```
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(4, 1fr);
  grid-template-areas:
  "header header header"
  "news news aside"
  "promo promo aside"
  ". footer footer";
} 
```

```
.header {
  grid-area: header;
} 
```

#css-grid