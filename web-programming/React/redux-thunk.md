Напишем усилитель для логирования экшенов и применим его с помощью `applyMiddleware()`:

```
import { createStore, applyMiddleware } from 'redux';
import { rootReducer } from './services/reducers';

// Наш усилитель
const actionLogger = store => next => action => {
    // Выводим в консоль время события и его содержание
  console.log(`${new Date().getTime()} | Action: ${JSON.stringify(action)}` );
    // Передаём событие «по конвейеру» дальше
  return next(action);
};

// Расширитель хранилища принимает в качестве аргумента усилитель
const enhancer = applyMiddleware(actionLogger);

// Инициализируем хранилище, использовав расширитель
const store = createStore(rootReducer, enhancer); 
```

Усилителей может быть несколько, функция `applyMiddleware()` принимает любое количество аргументов и применяет их последовательно:

```
import { createStore, applyMiddleware } from 'redux';
import { rootReducer } from './services/reducers';

// Усилитель 1
const actionLogger = store => next => action => {
  console.log(`${new Date().getTime()} | Action: ${JSON.stringify(action)}` );
  return next(action);
};

// Усилитель 2
const errorLogger = store => next => action => {
  if (action.type === 'SOMETHING_FAILED') {
        console.error(`Произошла ошибка: ${JSON.stringify(action)}`)
    }
  return next(action);
};

// Расширитель хранилища принимает несколько усилителей одновременно
const enhancer = applyMiddleware(actionLogger, errorLogger);

// Инициализируем хранилище, использовав расширитель
const store = createStore(rootReducer, enhancer); 
```

## Усилитель Redux: библиотека `redux-thunk`

Как вы знаете, экшены в Redux — просто объекты вида:

```
const someAction = {
    type: 'DO_SOME_WORK',
    // ...дополнительные данные
} 
```

Но в реальном мире простого объекта бывает недостаточно, ведь если говорить про взаимодействие с API, то экшен — функция, которая возвращает `Promise`. У такой функции должна быть возможность отправлять экшены на каждом этапе жизни промиса. Тут-то и приходит на помощь одна из самых популярных библиотек — `redux-thunk`. Благодаря возможности добавлять усилители в Redux, `redux-thunk` позволяет использовать функции (и асинхронные тоже) в качестве экшенов.

Подключим усилитель к хранилищу:

```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './services/reducers';

const store = createStore(rootReducer, applyMiddleware(thunk)); 
```

Напишем асинхронный экшен и вызовем его. Будем запрашивать данные с сервера и отправлять экшены по мере выполнения запроса:

```
import {
    GET_FEED,
    GET_FEED_FAILED,
    GET_FEED_SUCCESS
} from './constants';

// Наш первый thunk
export function getFeed() {
    // Воспользуемся первым аргументом из усилителя redux-thunk — dispatch
  return function(dispatch) {
        // Проставим флаг в хранилище о том, что мы начали выполнять запрос
        // Это нужно, чтоб отрисовать в интерфейсе лоадер или заблокировать 
        // ввод на время выполнения запроса
    dispatch({
      type: GET_FEED
    })
        // Запрашиваем данные у сервера
    fetch('/feed').then( res  => {
      if (res && res.success) {
                // В случае успешного получения данных вызываем экшен
                // для записи полученных данных в хранилище
        dispatch({
          type: GET_FEED_SUCCESS,
          feed: res.data
        })
      } else {
                // Если произошла ошибка, отправляем соответствующий экшен
        dispatch({
          type: GET_FEED_FAILED
        })
      }
    }).catch( err => {
            // Если сервер не вернул данных, также отправляем экшен об ошибке
            dispatch({
                type: GET_FEED_FAILED
            })
        })
  }
} 
```

OR:

```
export function getItems() {
  return function(dispatch) {
    dispatch({
      type: GET_ITEMS_REQUEST
    })
    
    getItemsRequest().then(res => {
      if (res && res.success) {
        dispatch({
          type: GET_ITEMS_SUCCESS,
          items: res.data
        });
      } else {
        dispatch({
          type: GET_ITEMS_FAILED
        });
      }
    });
  }
}
```

С применением `redux-thunk` мы можем передать функцию `getFeed()` в `store.dispatch()`. Но прежде чем мы напишем компонент, который вызовет такой экшен, посмотрим на редьюсер:

```
import {
    GET_FEED,
    GET_FEED_FAILED,
    GET_FEED_SUCCESS
} from './constants';

const initialState = {
    feedRequest: false,
    feedFailed: false,
    feed: []
}

export const feedReducer = (state = initialState, action) => {
  switch (action.type) {
    case GET_FEED: {
      return {
        ...state,
                // Запрос начал выполняться
        feedRequest: true,
                // Сбрасываем статус наличия ошибок от предыдущего запроса 
                // на случай, если он был и завершился с ошибкой
                feedFailed: false,
      };
    }
    case GET_FEED_SUCCESS: {
      return { 
                ...state, 
                // Запрос выполнился успешно, помещаем полученные данные в хранилище
                feed: action.feed, 
                // Запрос закончил своё выполнение
                feedRequest: false 
            };
    }
    case GET_FEED_FAILED: {
      return { 
                ...state, 
                // Запрос выполнился с ошибкой, 
                // выставляем соответствующие значения в хранилище
                feedFailed: true, 
                // Запрос закончил своё выполнение
                feedRequest: false 
            };
    }
        default: {
            return state
        }
    }
} 
```

Чтобы пользовательский интерфейс корректно реагировал на асинхронную операцию, нам потребуется минимум три экшена:

1. Запрос начал выполняться: если значение равно `true`, показываем в интерфейсе лоадер, блокируем кнопки и так далее.
2. Запрос выполнился успешно: помещаем данные в хранилище для дальнейшей отрисовки их в компонентах, убираем статус выполнения запроса, прячем лоадеры и так далее.
3. Запрос выполнился с ошибкой: показываем в интерфейсе уведомление об ошибке и, к примеру, красим поля ввода в красный цвет. На этом этапе также убираем статус выполнения запроса.

Последнее, что нам осталось сделать, — написать компонент, в котором будет вызвана экшен-функция:

```
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
// Наш thunk для запроса данных с сервера
import { getFeed } from '../sevices/actions';

const NewsFeed = () => {
    // Вытаскиваем селектором нужные данные из хранилища
    const { feed, feedRequest, feedFailed } = useSelector(state => state.news);

    // Получаем метод dispatch
    const dispatch = useDispatch();
    
    useEffect(()=> {
        // Отправляем экшен-функцию
        dispatch(getFeed())
    }, [])
    
    // Используем условный рендеринг для разных состояний хранилища
    if (feedFailed) {
        return <p>Произошла ошибка при получении данных</p>
    } else if (feedRequest) {
        return <p>Загрузка...</p>
    } else {
        return <>{feed.map(post => <NewsPost key={post.id} {...post} />)}</>;
    }
} 
```

#react 
