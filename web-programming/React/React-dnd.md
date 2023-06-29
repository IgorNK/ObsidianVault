[[React-dnd + Redux]]

```
npm i react-dnd react-dnd-html5-backend
```

```
import { DndProvider } from "react-dnd";
import { HTML5Backend } from "react-dnd-html5-backend";

const DragAndDropContainer = () => {

    return (
        <section className={styles.app}>
            <DndProvider backend={HTML5Backend}>
                <article className={styles.element}>
                    <div className={styles.animals}>
                        {
                            elements.map( animal => <DraggableAnimal key={animal.id} data={animal}/>)
                        }
                    </div>
                        <DropTarget onDropHandler={handleDrop} >
                            {
                                draggedElements.map(item => (
                                    <div key={item.id} className={styles.item}>
                                        <span className={styles.animalItem}>
                                            {item.content}
                                        </span>
                                        <p>
                                            {item.description}
                                        </p>
                                    </div>
                                ))
                            }
                        </DropTarget>
                </article>
            </DndProvider>
        </section>
    )
}

export default DragAndDropContainer 
```

Работа с компонентами в `react-dnd` происходит через два кастомных хука:

- `useDrag` — для перетаскиваемого элемента,
- `useDrop` — для целевого элемента.

### Перетаскивание элемента. Хук useDrag

`useDrag` позволяет добавлять элементам функциональность перетаскивания. Хук возвращает массив из трёх значений:

- `CollectedProps` — объект, который предоставляет другим частям компонента доступ к вычислениям функции `collect` внутри хука `useDrag`;
- `dragRef` — реф, который добавляется перетаскиваемому элементу или компоненту;
- `dragPreviewRef` — реф, который указывается для элемента, используемого в качестве превью, или компонента во время перетаскивания. `dragPreviewRef` использовать необязательно.

Сам хук принимает два параметра:

- объект или функцию;
- массив `deps`, который используется для мемоизации вычислений.

первый параметр содержит объект с уникальными для `react-dnd` полями:

- Обязательное свойство `type`. Это строка, благодаря которой целевой элемент понимает, какие элементы в него можно перетащить. Это поле мы укажем и в хуке `useDrag`, и в `useDrop`.
- Обязательное свойство `item`. Это данные о перетаскиваемом элементе. Они используются в обоих хуках — `useDrag` и `useDrop`. Поскольку `react-dnd` использует `HTML5 dnd api` в качестве провайдера — важно помещать в `item` только ключевую информацию о перетаскиваемом объекте. Почти всегда можно использовать только id конкретного элемента.
- Необязательный метод `collect`. Об этой функции мы говорили выше — это набор вычислений для работы с пропсами. Метод принимает два параметра: `monitor` и пропсы.
```
import React from "react";
import styles from "./draggable-animal.module.css";
import { useDrag } from "react-dnd";

const DraggableAnimal = ({data}) => {
    const {id, content} = data;
    const [, dragRef] = useDrag({
        type: "animal",
        item: {id}
    });

    return (
            <div
                ref={dragRef}
                className={styles.animalElement}
            >
                {content}
            </div>
    )
    ;
};

export default DraggableAnimal 
```

Для подсвечивания перетаскиваемого объекта можно использовать метод `collect` с параметром `monitor`. Параметр `monitor` — собственная реализация хранилища в React dnd. В объекте `monitor` 11 методов, но мы разберём только основные из них:

- `canDrag` — возвращает булевое значение `true` в случае, если в этот момент никакой элемент не перетаскивается.
- `isDragging` — возвращает `true`, если происходит перетаскивание.
- `didDrop` — возвращает `true`, если на целевом элементе срабатывает событие `drop`.
- `getItemType` — возвращает `type` перетаскиваемого элемента.
- `getItem` — возвращает объект `item`. Это те данные, которые мы указали в ключе `item` хука `useDrag`.

Например, мы можем полностью скрыть перетаскиваемый элемент из основного контейнера:

```
import React from "react";
import styles from "./draggable-animal.module.css";
import { useDrag } from "react-dnd";

const DraggableAnimal = ({data}) => {
    const {id, content} = data;
    const [{isDrag}, dragRef] = useDrag({
        type: "animal",
        item: {id},
        collect: monitor => ({
            isDrag: monitor.isDragging()
        })
    });

    return (
            !isDrag && 
            <div
                ref={dragRef}
                className={styles.animalElement}
            >
                {content}
            </div>
    )
    ;
};

export default DraggableAnimal 
```

### Работа с целевым элементом. Хук useDrop

Этот хук похож на `useDrag` структурой и методами объекта `monitor`, но сам объект внутри хука несколько отличается. Разберём основные параметры и возвращаемые значения этого хука.

Хук возвращает массив из двух значений:

- `CollectedProps` — объект, который предоставляет другим частям компонента доступ к вычислениям функции `collect` внутри хука `useDrop`;
- `DropTargetRef` — реф, который указывает на целевой элемент.

Внутри объекта хука `useDrop` наиболее важны:

- Обязательное свойство `accept`. Его значение — строка, которая должна быть аналогична свойству `type` перетаскиваемого компонента.
- Необязательные методы `hover` и `drop`. Оба принимают в качестве параметра `item` перетаскиваемого компонента и `monitor`. Первый метод срабатывает, когда перетаскиваемый элемент попадает в зону целевого. Второй метод — при «броске» перетаскиваемого элемента в целевой.
- Необязательный метод `collect`. Аналогичен методу `collect` хука `useDrag`. Единственная разница — наличие метода `isOver` и отсутствие метода `isDragging`.

По аналогии с компонентом `DraggableAnimal` напишем новый код в компоненте `DragTarget`:

```
import React from 'react';
import styles from './drop-target.module.css'
import { useDrop } from "react-dnd";

const DropTarget = ({children, onDropHandler}) => {
    const [, dropTarget] = useDrop({
        accept: "animal",
        drop(itemId) {
            onDropHandler(itemId);
        },
    });

    return (
            <div
                ref={dropTarget}
                className={styles.target}
            >
                {children}
            </div>
    );
};

export default DropTarget; 
```

В этом компоненте впервые появились наши вычисления. React-dnd берёт на себя всё, что касается событий dnd, но работу с вычислениями и изменением состояния должен описывать сам разработчик. Опишем изменения состояния в компоненте `DragAndDropContainer` и передадим их компоненту `DropTarget` в качестве пропса:

```
import { DndProvider } from "react-dnd";
import { HTML5Backend } from "react-dnd-html5-backend";

const DragAndDropContainer = () => {
    const [elements, setElements] = React.useState([]);
    const [draggedElements, setDraggedElements] = React.useState([]);

    const handleDrop = (itemId) => {
        setElements([
            ...elements.filter(element => element.id !== itemId.id)
        ]);

        setDraggedElements([
            ...draggedElements,
            ...elements.filter(element => element.id === itemId.id)
        ]);
    };

    return (
        // Другая возвращаемая разметка и компоненты
        <DropTarget onDropHandler={handleDrop} />
    )
}

export default DragAndDropContainer 
```

Стоит подсказать пользователю, что элемент интерфейса можно перетащить в целевой элемент. Например, окрашивая рамку целевого элемента. Для этого можно пойти двумя путями: использовать метод `hover` хука `useDrop` или написать собственный метод внутри `collect`. Воспользуемся вторым подходом: мы хотим получать значения из `useDrop` в других местах. Обратимся к методу `isOver` объекта `monitor`, он ведёт себя схожим образом с `hover`:

```
import React from 'react';
import styles from './drop-target.module.css'
import { useDrop } from "react-dnd";

const DropTarget = ({children, onDropHandler}) => {
    const [{isHover}, dropTarget] = useDrop({
        accept: "animal",
        drop(itemId) {
            onDropHandler(itemId);
        },
        collect: monitor => ({
            isHover: monitor.isOver(),
        })
    });

    const borderColor = isHover ? 'lightgreen' : 'transparent';

    return (
            <div
                ref={dropTarget}
                className={styles.target}
                style={{borderColor}}
            >
                {children}
            </div>
    );
};

export default DropTarget; 
```

EXAMPLE:

```
const [{opacity}, dragRef] = useDrag({
    type: 'items',
    item: {id},
    collect: monitor => ({
      opacity: monitor.isDragging() ? 0.5 : 1
    })
  });

  return (
    <div style={{opacity}} className={`${styles.product}`}>
      <div ref={dragRef} className={styles.productBox}>
        <img className={styles.img} src={src} alt="фото товара." />
        <p className={styles.text}>{text}</p>
      </div>
      <div className={styles.amountbox}>
        <AmountButton onClick={decrease}>-</AmountButton>
        <p className={styles.amount}>{qty}</p>
        <AmountButton onClick={increase}>+</AmountButton>
      </div>
      <div className={styles.price}>
        <p className={`${styles.price} ${discount && styles.exPrice}`}>
          {priceFormat(price * qty)}
        </p>
        {discount && <p className={styles.price}>{priceFormat(discountedPrice)}</p>}
      </div>
      <DeleteButton onDelete={onDelete} />
    </div>
  );
```

```
const [{isHover}, dropTarget] = useDrop({
    accept: (tabName === 'items') ? 'items' : 'postponed',
    collect: monitor => ({
      isHover: monitor.isOver()
    })
  })

  const className = `${styles.tab} ${currentTab === tabName ? styles.tab_type_current : ''} ${isHover && styles.onHover}`;
  return (
    <div ref={dropTarget} className={className} onClick={switchTab}>
      {text}
    </div>
  );
};
```

[[React-dnd + Redux]]

#react 