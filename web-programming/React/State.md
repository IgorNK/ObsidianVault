State can be set as a function or as a state:
```
 this.setState(function(prevState, props){
   return { showForm: !prevState.showForm }
 });

 // либо
 this.setState({ showForm: !this.state.showForm });
```
When setting as state, the function has access to the previous state and props

As a second argument setState can take a callback function:
```
 this.setState(
     { showForm: !this.state.showForm },
     () => {
         this.myCustomLogger("Состояние компонента изменилось!")
     }
 );
  
```

State changing example:
```
class CollapsableTextBlock extends React.Component {

  state = { toggled: false };

  onHeadingClick = () => {
    this.setState({toggled: !this.state.toggled})
  }

  render() {
    return (
      <div className="CollapsableTextBlock">
        <h1 onClick={this.onHeadingClick}>
          {this.props.header}
        </h1>
        {this.state.toggled && <p>{this.props.children}</p>}
      </div>
    );
  }
} 
```

Props drilling: the same code with state handled down to a child component as a prop:
```
function CollapsableTextContent(props) {
    if (!props.toggled) {
        return null;
    }
    return <p>{props.children}</p>
}

class CollapsableTextBlock extends React.Component {

  state = { toggled: false };

  onHeadingClick = () => {
    this.setState({toggled: !this.state.toggled})
  }

  render() {
    return (
      <div className="CollapsableTextBlock">
        <h1 onClick={this.onHeadingClick}>
          {this.props.header}
        </h1>
        <CollapsableTextContent toggled={this.state.toggled}> 
                    {this.props.children}
        </CollapsableTextContent>
      </div>
    );
  }
} 
```


Using spread syntax (...) to modify only some of the state parameters, useful for arrays:
```
class NewsPanel extends React.Component {
    // Состояние компонента
  state = {
        theme: "светлая",
        posts: [
            { id: 1, title: "Новость 1" },
            { id: 2, title: "Новость 2" }
        ],
        commentsEnabled: true,
        user: {
            name: "Гекльберри Финн",
            uuid: "123e4567-e89b-12d3-a456-426655440000",
            lastActive: 1614498769824
        }
    };

    // Метод, который отвечает за добавление
    addNewPost = () => {
      this.setState(prevState => {
        // Вычисляем номер добавляемой новости на основании текущего количества
        const newPostNumber = prevState.posts.length + 1;
        return {
          // Сохраняем текущие значения, над которыми не производим действий
          ...prevState,
          posts: [
            // Сохраняем текущий список новостей
            ...prevState.posts,
            // Добавляем в список новостей новую запись с вычисленным номером
            {
              id: prevState.posts.length + 1,
              title: `Новость ${newPostNumber}`
            }
          ]
        };
      });
    };

    render() {
      return <React.Fragment>
          <ul>
            {this.state.posts.map(post => <li key={post.id}>{`ID#${post.id}: ${post.title}`}</li>)}
          </ul>
          {/* При нажатии на кнопку будет вызван метод addNewPost */}
          <button onClick={this.addNewPost}>Добавить новость</button>
        </React.Fragment>;
    }
} 
```


#react