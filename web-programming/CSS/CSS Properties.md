Element alignment:
display: inline - inline element, doesn't take a new line, takes only as much space as needed (like <a>), ignores custom width/height
display: block - block element, takes a new line, takes the entirety of parent space (like <p> or <div>), takes custom width/height
display: inline-block - keeps width/height parameters and doesn't take a new line. Aligns by the end of the last text line by default.

vertical-align: top - can align inline-block elements by their top border

```
div {
    box-sizing: border-box; /* задали box-sizing */
    width: 300px; /* ширина блока */ 
    height: 200px; /* высота блока */
    border: 2px solid red; /* граница не влияет на размер */
    padding: 20px; /* отступ не растягивает блок */
} 
```

Текст

    color — цвет текста. Можно задать ключевыми словами, HEX-кодом, в RGB- или RGBA-палитре.
    text-align — выравнивание текста внутри блока: по левому краю left, по правому right, по центру center.
    text-decoration — оформление текста. Часто используют подчеркнутый текст underline и неоформленный none.
    text-transform — перевод текста в верхний uppercase или нижний lowercase регистр.

Шрифт

    font-family — семейство шрифтов. Работает только со шрифтами, которые есть на устройстве пользователя. Можно перечислять шрифты через запятую, чтобы браузер искал среди них установленный.
    font-size — размер шрифта.
    font-weight — начертание: полужирное bold, обычное normal и тонкое lighter.

background-color — цвет фона. Можно задать словами, HEX-кодом, в RGB- или RGBA-палитре.
background-image — фоновое изображение. Ссылка на ресурс задаётся внутри параметра url()
background-size — размер фона. Часто используют значения:
    contain — фоновое изображение целиком вписывается в блок;
    cover — фон накрывает блок.
`background-position` — расположение фона по вертикали и горизонтали. Можно задать его позицию словами:
-   `left`, `center` и `right` для горизонтального выравнивания;
-   `top`, `center` и `bottom` для вертикального.

#css