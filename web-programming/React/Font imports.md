Since webpack is used, it's preferable to use relative paths:
```
/* index.css */
/* Другие стили в index.css */

@font-face {
  font-family: 'OpenSansCondensed';
  font-style: normal;
  font-weight: 700;
  src: url("./fonts/OpenSansCondensed-Bold.woff") format("woff");
}

@font-face {
  font-family: 'OpenSansCondensed';
  font-style: normal;
  font-weight: 300;
  src: url("./fonts/OpenSansCondensed-Light.woff") format("woff");
}

@font-face {
  font-family: 'OpenSansCondensed';
  font-style: italic;
  font-weight: 300;
  src: url("./fonts/OpenSansCondensed-LightItalic.woff") format("woff");
} 
```