**componentDidMount** - good to use for event subscribtion and server API calls:

```
class VirtualList extends React.Component {
    state = { offsetY: 0 }
    componentDidMount() {
        document.addEventListener(
            'scroll', 
            ()=>this.setState({ offsetY: window.pageYOffset })
        )
    }
    // ...
} 
```

```
class Stories extends React.Component {
    state = { 
        stories: [],
        loading: true,
        hasError: false
    }

    componentDidMount() {
        fetch('/stories/all')
            .then(res => res.json())
            .then(data => this.setState({ stories: data.stories, loading: false }))
            .catch(e => this.setState({ ...this.state, loading: false, hasError: true }))
    }
    // ...
} 
```

**componentDidUpdate** - goot to use with server API calls if comparisons with previous state or props are needed:
```
class Product extends React.Component {
    state = { 
        productData: null,
        loading: true,
        hasError: false
    }
    
    getProductData = () => {
        fetch(`/api/v1/products/${this.props.productId}`)
            .then(res => res.json())
            .then(data => this.setState({ productData: data.productData, loading: false }))
            .catch(e => this.setState({ ...this.state, loading: false, hasError: true }))
    }    

    componentDidMount() {
        this.getProductData()
    }

    componentDidUpdate(prevProps, prevState) {
      // Сравниваем предыдущие пропсы с обновившимися.
        // Если они отличаются, то делаем запрос:
      if (this.props.productId !== prevProps.productId) {
        this.getProductData();
      }
    }
    // ...
} 
```
setState() can be used in componentDidUpdate but a condition should be used, otherwise there's a risk of an infinite loop:
```
// ...
componentDidUpdate(prevProps, prevState) {
  // Сравниваем предыдущее состояние с новыми
    // Если нужные нам ключи отличаются, то делаем запрос:
  if (this.props.productId !== prevProps.productId) {
        this.setState({ ...this.state, loading: true });
    this.getProductData();
  }
}
// ... 
```

**shouldComponentUpdate** - used for optimization to override the default rules for component re-rendering upon props or state change:
```
class TodoItem extends React.Component {
  constructor(props) {
    super(props);
    this.state = { done: false };
  }

  shouldComponentUpdate(nextProps, nextState) {
    if (this.props.tagColor !== nextProps.tagColor) {
      // Если изменилось значение props.tagColor, то будет вызван повторный рендер
      return true;
    }
    if (this.state.done !== nextState.done) {
      // Если изменилось значение state.done, то будет вызван повторный рендер
      return true;
    }

    // Во всех остальных случаях повторного рендеринга не будет
    return false;
  }

  toggleTodo = () => {
    this.setState({ done: !this.state.done });
  };

  render() {
    const btnText = this.state.done ? "Вернуть в работу" : "Выполнено";
    return (
      <>
        <TodoTag tagColor={this.props.tagColor} />
        <button onClick={this.toggleTodo}>{btnText}</button>
      </>
    );
  }
}
```

**componentWillUnmount** - helpful for event handler cleanup to prevent memory leaks:
```
class VirtualList extends React.Component {
  state = { offsetY: 0 };
  // Создадим отдельную функцию для того чтобы слушатель событий можно было удалить
  setOffset = () => {
    this.setState({ offsetY: window.pageYOffset });
  };

  componentDidMount() {
    document.addEventListener("scroll", this.setOffset);
  }

  componentWillUnmount() {
    // Не забывайте отписываться от событий, чтобы не допустить утечек памяти
    document.removeEventListener("scroll", this.setOffset);
  }
  // ...
} 
```


#react