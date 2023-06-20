```
import { useDispatch } from 'react-redux';
import { useState } from 'react';

import { addTodo, removeToDo } from './services/actions';

function App(props) {
    // Функция dispatch теперь доступна из хука внутри компонента
    const dispatch = useDispatch();

    const [text, setText] = useState('');

    const onSubmit = () => {
        // Отправляем экшен, используя переменную из хука React.useState
        dispatch(addTodo(text))
    }

    const onDelete = (id) => {
        // Отправляем экшен
        dispatch(removeToDo(id))
    }

    const 
    

  // ...
}

export default App;
```

Хук `useDispatch()` можно использовать и так:

```

import { useDispatch } from 'react-redux';
import { useEffect } from 'react';

const App = (props) => {
    const dispatch = useDispatch();

    useEffect(() => {
        // Отправляем экшен при монтировании компонента
        dispatch({type: 'GET_DELIVERY_OPTIONS' });
    }, [])

    // ...
}

export default App; 
```



#react 