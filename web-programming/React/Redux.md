```
import { guid } from '../utils'
import { ADD_TODO, TOGGLE_TODO } from './constants'
 
// Исходное состояние
const initialState = [
    {
        id: guid(),
        completed: false,
        expiresAt: '08.04.20201',
        text: 'Купить авокадо 4 шт.'
    }
]

// Редьюсер
const todoList = (state = initialState, action) => {
  switch (action.type) {
        // Добавление новой задачи в список дел
    case ADD_TODO:
      return [
        ...state,
        {
          id: guid(),
          text: action.text,
                    expiresAt: action.expiresAt,
          completed: false
        }
      ]
        // Изменение статуса задачи в списке дел
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.id ? { ...todo, completed: !todo.completed } : todo
      )
        // Реакция на прочие типы экшенов
    default:
      return state
  }
} 
```

### Работа со множеством редьюсеров

В больших приложениях требуется разделять код хранилища на части. Мы уже говорили, что один из недостатков Redux — его «многословность». Код хранилища будет очень быстро разрастаться. Поэтому, согласитесь, `switch-case` из 20 тысяч строк — не лучшее решение. При этом для инициализации хранилища нужен один корневой редьюсер, который собирает все части состояния воедино.

Посмотрим, как всё это организовать. Вы уже знаете, что каждый редьюсер — чистая функция, а состояние — объект. Тогда напишем корневой редьюсер для списка дел и представим, что в приложении есть и другие части состояния:

```
import { combineReducers } from 'redux';

// Редьюсер списка дел
const todoList = (state, action) => { ... }

// Редьюсер пользователя приложения
const user = (state, action) => { ... }

// Редьюсер коллективной работы над списком дел
const collaboration = (state, action) => { ... }

// Корневой редьюсер
const rootReducer = combineReducers({
    todoList,
    user,
    collaboration
}) 
```

Самое простое хранилище с одним аргументом можно инициализировать так:

```
import { createStore } from 'redux';
import { rootReducer } from './reducers'; 

const store = createStore(rootReducer); 
```

## Доступ к состоянию хранилища. Компонент `Provider`

Этот компонент нужен для доступа из приложения к актуальному состоянию хранилища. «Под капотом» `Provider` использует React.Context. Но если контекст можно применять на любом уровне иерархии, то компонент `Provider` рекомендуется использовать только на верхнем уровне приложения. Так в хранилище можно обратиться из любой точки приложения, как бы глубоко в иерархии ни находился компонент. В качестве пропсов компонент `Provider` получает объект `store`, созданный с помощью известной вам функции `createStore()`:

```
import { createStore } from 'redux';
import { Provider } from 'react-redux';

import App from './components/app/app';

// Корневой редьюсер, который обрабатывает экшены
import { rootReducer } from './services/reducers';

// Инициализируем хранилище с помощью корневого редьюсера
const store = createStore(rootReducer);

ReactDOM.render(
    // Оборачиваем приложение компонентом Provider из пакета react-redux
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
); 
```

[[connect]]
[[useSelector]]
[[useDispatch]]

#react