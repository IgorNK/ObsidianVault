```
const UserContext = React.createContext();

function App() {
    // создайте стейт с помощью хука useState()
  const userState = useState({ name: "Виссарион" });

  return (
        // положите стейт в провайдер контекста
    <UserContext.Provider value={userState}>
      <UserPage />
    </UserContext.Provider>
  );
}

function UserPage() {
    // значение состояния можно получить через деструктуризацию
    // прямо как получение состояния в хуке useState()
  const [user] = useContext(UserContext);

  return (
    <section>
      <h2>Привет, {user.name}! Это твой личный кабинет.</h2>
      <UserEditForm />
    </section>
  );
}

function UserEditForm() {
    // точно так же, через деструктуризацию, можно получить и функцию-сеттер
  const [user, setUser] = useContext(UserContext);

  function handleSubmit(e) {
    e.preventDefault();
    setUser({ name: e.target.name.value });
  }

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Имя: </label>
        <input id="name" type="text" placeholder={user.name} />
      </div>
      <div>
        <button type="submit">Сохранить</button>
      </div>
    </form>
  );
} 
```