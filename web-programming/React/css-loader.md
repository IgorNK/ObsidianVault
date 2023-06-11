You can load css files for components as modules if you name them "...module.css":
```
import React from 'react';

import styles from './app.module.css';

// Импортируем компоненты
import Header from '../header/header';
import DogImage from '../dog-image/dog-image';
```
After building the project CSS selectors would all have unique names hashed by webpack.

Regular css files are imported like this:
```
import './awesome-css-file.css'; 
```
This way webpack would take them in their unmodified form.

Module styles can be used this way:
```
import React from 'react';

import styles from './app.module.css';
// Подключили модуль app;

import Header from '../header/header';
import DogImage from '../dog-image/dog-image';

class App extends React.Component {
  render() {
    return (
      <div className={ styles.app }> 
                    {/* Указали в значении путь до селектора; */}
          <Header />
          <DogImage />
      </div>
    )
  }
}

export default App; 
```

or:
```
import React from "react";
import dogImage from "../dog-image/dog-image.module.css";

class DogImage extends React.Component {
  render() {
    return (
      <div className={dogImage.card}>
        <img
          className={dogImage.image}
          src={this.props.image}
          alt="Грустная собачка"
        />
        <h2 className={dogImage.title}>{this.props.name}</h2>
        <span className={dogImage.description}>{this.props.description}</span>
      </div>
    );
  }
}

export default DogImage; 
```

***Composes keyword*** can be used in modules to re-utilize already defined styles:
```
.card {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.image {
  max-width: 400px;
}

.title {
  font-size: 25px;
  color: #393a34;
  font-family: 'Helvetica', Arial, sans-serif;
  margin: 5px 0;
}

.description {
  composes: title;
  /* В значении composes указывается название селектора. */
  font-size: 15px;
  font-style: italic;
} 
```
Если обратиться к переменной `dog-image`, в которой находятся хешированные селекторы, можно увидеть следующее:

```
{
    card: "dog-image_card__11D0F"
    description: "dog-image_description__1IUew dog-image_title__3mTse"
    image: "dog-image_image__2fGQ8"
    title: "dog-image_title__3mTse"
} 
```

Получается, теперь к HTML-элементу добавляется сразу два класса: `dog-image_description__1IUew` и `dog-image_title__3mTse`. Другими словами, `composes` работает на основе каскадности CSS-правил. А значит, зависимое CSS-правило `.description` должно быть объявлено после `.title`.

This also works with outside modules:
```
.selector {
    composes: anotherSelectorName from "./yet-awesome-file.module.css"
} 
```

#react