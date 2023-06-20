```
const [state, dispatch] = React.useReducer(reducer, initialArg, init);
```

\[state, dispatch\] - результат хука, state - текущее состояние, dispatch - функция отправки экшена
reducer - функция-обработчик состояния
initialArg - исходное значение
init - функция инициализации

_Интерфейс хука useReducer()._

Хук принимает до трёх аргументов:

- функцию `reducer()`, которая отвечает за дополнительную логику обработки состояния;
- начальное значение состояние `initialArg`;
- необязательный аргумент: функцию инициализации `init()`, с помощью которой можно вычислить начальное значение состояния.

И возвращает массив из:

- текущего значения состояния`state`;
- функции `dispatch()`, которая вызывает `reducer()` и передаёт ему необходимые аргументы.

## Как применять `useReducer()`

```
// начальное значение стейта
const initialState = { count: 0 };

// функция-редьюсер
// изменяет состояния в зависимости от типа переданного action
function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      throw new Error(`Wrong type of action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const handleIncrementClick = () => {
        // при вызове dispatch достаточно указать тип действия
    dispatch({ type: "increment" });
  };

  const handleDecrementClick = () => {
    dispatch({ type: "decrement" });
  };

  return (
    <>
      Count: {state.count}
      <button onClick={handleDecrementClick}>-</button>
      <button onClick={handleIncrementClick}>+</button>
    </>
  );
} 
```

#react