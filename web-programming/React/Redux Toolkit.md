```
import { configureStore } from '@reduxjs/toolkit'
import logger from 'redux-logger'
import todosReducer from './todos/todosReducer'
import { customEnhancer } from './enhancers'

const preloadedState = {
  todos: [],
}
const store = configureStore({
  reducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
  devTools: process.env.NODE_ENV !== 'production',
  preloadedState,
  enhancers: [customEnhancer],
}) 
```

```
import { createSlice } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: 0,
  reducers: {
    incrementByValue: (state, action) => state + action.payload,
  },
}) 
```

```
import { useDispatch } from 'react-redux';
import { useEffect } from 'react';
import { counterSlice } from './slices'

const App = (props) => {
    const dispatch = useDispatch();
    const { actions } = counterSlice();
    useEffect(() => {
        // Отправляем экшен при монтировании компонента
        dispatch(actions.incrementByValue(1));
    }, [])

    // ...
}

export default App; 
```

в Redux Toolkit с применением `createAction()` создание экшена и его типа выглядит так:

```
import { createAction } from '@reduxjs/toolkit'

const increment = createAction('ACTION_NAME') 
```

В случае если экшен содержит данные (payload), это можно записать так:

```
import { createAction } from '@reduxjs/toolkit'

const addTodo = createAction('todos/add', function prepare(text) {
  return {
    payload: {
      text,
      id: 1,
      createdAt: '08.04.20201',
    },
  }
})
```

В итоге `addToDo` будет выглядеть таким образом:

```
import { useDispatch } from 'react-redux';
import { useEffect } from 'react';
import { addTodo } from './actions'

const App = (props) => {
    const dispatch = useDispatch();
    const { actions } = counterSlice();
    useEffect(() => {
        // Отправляем экшен при монтировании компонента
        dispatch(addTodo);
    }, [])

    // ...
}

export default App; 
```


`createReducer`:

```
import { createAction, createReducer } from '@reduxjs/toolkit'

const increment = createAction('counter/increment')
const decrement = createAction('counter/decrement')
const incrementByAmount = createAction('counter/incrementByAmount')

const initialState = { value: 0 }

const counterReducer = createReducer(initialState, (builder) => {
  builder
    .addCase(increment, (state, action) => {
      state.value++
    })
    .addCase(decrement, (state, action) => {
      state.value--
    })
    .addCase(incrementByAmount, (state, action) => {
      state.value += action.payload
    })
}) 
```

После этого `counterReducer`, как и другие редьюсеры, подключается к корневому редьюсеру:

```
...
import { counterReducer } from './reducers'
...

// Редьюсер списка дел
const todoList = (state, action) => { ... }

// Редьюсер пользователя приложения
const user = (state, action) => { ... }

// Редьюсер коллективной работы над списком дел
const collabaration = (state, action) => { ... }

// Корневой редьюсер
const rootReducer = (state, action) => {
    todoList: todoList(state.todoList, action),
    user: user(state.user, action),
    collabaration: collabaration(state.collabaration, action),
    counterReducer
} 
```

#react 