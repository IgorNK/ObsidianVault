```
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />      }
    </div>  );
}
```

Conditional prop:

```
export default function DropTarget(props) {
  const { puzzleElement, handleDrop, handleDrag, dropTargetIndex } = props;
  return (
    <li
      onDragOver={(e) => e.preventDefault()}
      {...(!puzzleElement.id && {
      onDrop: (e) => handleDrop(e, dropTargetIndex)
      })}
      className="listItem"
    >
      {puzzleElement.elementSrc && (
        <img
          src={`./${puzzleElement.elementSrc}`}
          draggable
          onDrag={(e) => handleDrag(e, puzzleElement)}
        />
      )}
    </li>
  );
}
```

#react 