Функции `mapStateToProps` и `mapDispatchToProps` — основные аргументы `connect()`. Но у функции `connect()` есть ещё два аргумента, работа которых связана с основными. Третий и четвёртый аргументы используются редко, поэтому мы не будем подробнее на них останавливаться. Вы можете самостоятельно ознакомиться с ними [в официальной документации](https://react-redux.js.org/api/connect).

А пока — закрепим использование функции `connect()` на примере:

```
import { connect } from 'react-redux';

function App(props) {
  // ...
   
}

const mapDispatchToProps = (dispatch, ownProps) => {
  return {
          toggleTodo: () => dispatch(toggleTodo(ownProps.todoId)),
            addTodo: (text) => dispatch(addToDo(text)),
            changeLayout: () => dispatch({ type: 'CHANGE_LAYOUT' })
    };
};

const mapStateToProps = (store, ownProps) => {
  return { 
        todos: store.todos,
        theme: ownProps.theme ? ownProps.theme : store.appearance.theme,
        user: store.account.userData
    };
};

// Собираем всё вместе, передав два аргумента в connect
export default connect(mapStateToProps, mapDispatchToProps)(App);
```


В пакете `redux` есть вспомогательная функция — `bindActionCreators(actionCreators, dispatch)`. Представим, что у нас есть такие экшены:

```

 // ./services/actions.js
 export function addTodo(text) {
   return {
     type: 'ADD_TODO',
     text
   }
 }
 
 export function removeTodo(id) {
   return {
     type: 'REMOVE_TODO',
     id
   }
 }

  
```

Вот так выглядит функция `mapDispatchToProps` без `bindActionCreators`:

```

 import { addTodo, removeToDo } from './services/actions';
 
 const mapDispatchToProps = dispatch => {
   return {
             addTodo: (text) => dispatch(addToDo(text)),
             removeToDo: (id) => dispatch(removeTodo(id))
     };
 };

  
```

А так — с использованием `bindActionCreators`:

```

 import * as TodoActionCreators from './services/actions'
 
 const mapDispatchToProps = dispatch => 
     bindActionCreators(TodoActionCreators, dispatch);

  
```

По сути, `bindActionCreators` — синтаксический сахар, который позволяет писать чуть меньше кода.

Но существует ещё более ёмкий способ передать функции отправки экшенов в пропсы компонента:

```

 import { addTodo, removeToDo } from './services/actions';
 
 const mapDispatchToProps = {
     addTodo,
     removeToDo
 }
 
  
```

Необязательно передавать оба аргумента в функцию `connect()`, но если оставить её совсем без аргументов, изменение данных в хранилище не будет вызывать повторный рендеринг, а в пропсах появится дополнительная функция — `dispatch()`. Эта функция доступна и в случае, если передан только первый аргумент — `mapStateToProps`. Вы сможете использовать `dispatch()` из пропсов напрямую в компоненте:

```

class App extends React.Component {
    constructor(props) {
        super(props);

        this.state = { step: 'delivery' }    
    }

    componentDidMount() {
        this.props.dispatch({type: 'GET_DELIVERY_OPTIONS' });
    }

    render() {
        // ...
    }
}

export default connect()(App);
 
```

#react 