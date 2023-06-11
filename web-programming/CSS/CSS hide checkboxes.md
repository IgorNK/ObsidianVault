```
.input[type="checkbox"] {
    position: absolute;
    width: 1px; /* если у элемента не будет размеров, некоторые браузеры посчитают, что его вообще нет */
    height: 1px;
    overflow: hidden; /* без этого свойства тоже сработает, но так наверняка. Мы его ещё изучим, оно скрывает элементы, выходящие за границы родителя */
    clip-path: inset(0 0 0 0);
} 
```

```
<label>
    <input type="checkbox" class="invisible-checkbox"> <!-- этот элемент мы скрыли -->
    <span class="visible-checkbox"></span> <!-- этот будем стилизовать -->
</label> 
```

```
input[type="checkbox"]:disabled {} /* выключенный */
input[type="checkbox"]:checked {} /* выбранный */
input[type="checkbox"]:focus {} /* в фокусе */ 
```

```
input[type="checkbox"] + span {} /* так для чекбокса выглядит первый сосед с именем span */ 
```

#css