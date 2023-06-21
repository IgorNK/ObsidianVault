```
import { compose, createStore } from 'redux';
const composeEnhancers =
  typeof window === 'object' && window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__
    ? window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})
    : compose; 
```

Мы проверяем, что расширение Redux DevTools есть, а глобальный window вообще существует — это важно, например, при SSR. Если всё хорошо, мы используем расширение. В противном случае — вызовем функцию `compose`. По своей задаче обе функции примерно равны, но `window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({})` позволяет использовать расширение. Функция `compose` просто последовательно применяет расширители хранилища, если они есть. Опять же, про расширители мы поговорим в следующем уроке.

После вызова расширения нужно применить его к хранилищу. Для этого используется второй аргумент `createStore`:

```
import { compose, createStore, applyMiddleware } from 'redux';

const enhancer = composeEnhancers();

const store = createStore(rootReducer, enhancer); 
```

#react