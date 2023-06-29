файлы редьюсера и экшена для каждого компонента: `DropTarget` и `DraggableAnimal`. Редьюсер `DropTarget` будет содержать только информацию о названии каждой из досок:

```
// /services/reducers/drop-target.js

const initialState = {
    boards: ["default", "fish", "mammals", "insects"]
};

export const dropTargetReducer = (state = initialState, action) => {
    return state;
}; 
```

Редьюсер `DraggleAnimal` будет содержать исходное состояние с массивом животных `animals`. У каждого животного будет атрибут `board` с начальным значением `default`. В сам редьюсер `draggableAnimalReducer` будет записана обработка только одного экшена — `UPDATE_TYPE`:

```
// /services/reducers/draggable-animal.js
import { UPDATE_TYPE } from "../actions/draggable-animal";

const initialState = {
    animals: [
        {
            id: 12,
            content: "🦔",
            description: "Ёжики очень классные. И являются настоящими звёздами в Instagram.",
            board: "default"
        },
        // Другие животные
    ]
};

export const draggableAnimalReducer = (state = initialState, action) => {
    switch (action.type) {
        case UPDATE_TYPE: {
            return {
                ...state,
                animals: state.animals.map(animal =>
                    animal.id === action.id ? {...animal, board: action.board} : animal
                )
            };
        }
        default:
            return state;
    }
}

// /services/reducers/index.js
import { combineReducers } from "redux";
import { draggableAnimalReducer } from "./draggable-animal";
import { dropTargetReducer } from "./drop-target";

export const rootReducer = combineReducers({
    animalList: draggableAnimalReducer,
    boardList: dropTargetReducer
}); 
```

Подключим стор к проекту и переделаем компонент `DragAndDropContainer`. Теперь у нас есть несколько целевых элементов для перетаскивания, поэтому заберём из хранилища массив `boards` и отрисуем по компоненту `DropTarget`:

```
const DragAndDropContainer = () => {
    // Получим все доски из хранилища
    const boards = useSelector(state => state.boardList.boards)

    return (
        <section className={styles.app}>
            <DndProvider backend={HTML5Backend}>
                <article className={styles.element}>
                    {
                        // Отрисуем каждую доску и передадим её название в качестве пропса
                        boards.map((item, i) => (
                            <DropTarget key={i} board={item} />
                        ))
                    }
                </article>
            </DndProvider>
        </section>
    )
};

export default DragAndDropContainer; 
```

Обновим `DropTarget` и хук `useDrop` в нём:

```
const DropTarget = ({ board }) => {
    const dispatch = useDispatch();
    const animals = useSelector(state => state.animalList.animals)

    const [{ isHover } , drop] = useDrop({
        accept: "animal",
        collect: monitor => ({
            isHover: monitor.isOver(),
        }),
        drop(itemId) {
            dispatch({
                type: UPDATE_TYPE,
                ...itemId,
                board
            });
        },
    });

    const boardClass = board === 'default' ? styles.animalsPull : styles.animals;
    const borderColor = isHover ? 'lightgreen' : 'transparent';
    return (
        <div ref={drop} className={boardClass} style={{borderColor}}>
            {animals
                // Получим массив животных, соответствующих целевому элементу
                .filter(animal => animal.board === board)
                // Отрисуем массив
                .map(animal => <DraggableAnimal key={animal.id} data={animal} />)
            }
        </div>
    )
};

export default DropTarget 
```

Последнее, что требуется сделать для достижения такого результата, — модифицировать разметку `DraggableAnimal` в зависимости от того, в каком целевом элементе располагается перетаскиваемый.

```
const DraggableAnimal = ({ data }) => {
    const { id, board } = data;
    const [{ isDrag }, drag] = useDrag({
        type: "animal",
        item: { id },
        collect: monitor => ({
            isDrag: monitor.isDragging()
        })
    });

    // Отображение DraggableAnimal в целевом элементе "default"
    const draggableAnimalPreview = (
        <div ref={drag} className={styles.animalElement}>
            {data.content}
        </div>
    );
    
    // Отображение DraggableAnimal в других целевых элементах
    const draggableAnimalCard = (
        <div ref={drag} className={styles.item}>
            <span className={styles.animalItem}>
                {data.content}
            </span>
            <p>
                {data.description}
            </p>
        </div>
    );

    // Проверяем, где сейчас находится карточка
    const draggableAnimalContent = (
        board === "default" ? draggableAnimalPreview : draggableAnimalCard
    );

    return (
            !isDrag && draggableAnimalContent
    );
};

export default DraggableAnimal 
```

#react 