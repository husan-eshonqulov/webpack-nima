# Webpack nima?

## Webpack nima uchun kerak

Tassavvur qiling ishxonamiz tomonidan bizga yangi bir loyiha qilish topshirildi. Bu loyihada biz yil boshidan bugunga qadar necha kun o'tganini hisoblashimiz kerak.

Keling kompyuterimizda yangi loyiha yaratib olamiz. Buning uchun terminalda quyidagi kodlarni teramiz.

```console
foo@bar:~/Desktop$ mkdir webpack-loyiha
foo@bar:~/Desktop$ cd webpack-loyiha
foo@bar:~/Desktop/webpack-loyiha$ npm init -y
```

Shundan so'ng bizda `webpack-loyiha` degan papka va uning ichida `package.json` degan fayl hosil bo'ladi.

```
webpack-loyiha
|- package.json
```

Keling endi loyihamiz source kodlarini saqlash uchun `src` degan papka yaratib olamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ mkdir src
```

Endi shu src papkasi ichida `index.html` va `index.js` degan fayllar yaratib olamiz. Va index.js faylini index.html fayliga ulab qo'yamiz.

```console
foo@bar:~/Desktop/webpack-loyiha/src$ touch index.html
foo@bar:~/Desktop/webpack-loyiha/src$ touch index.js
```

Shunda papkamiz quyidagi ko'rishishda bo'ladi.

```
webpack-loyiha
|- /src
  |- index.html
  |- index.js
|- package.json
```

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
  <head>
    <title>Webpack Loyiha</title>
  </head>
  <body>
    <script src="index.js"></script>
  </body>
</html>
```

Endi `index.html` va `index.js` fayllariga _Hello from ..._ yozuvlarini qo'shib qo'yamiz va browser'da ochamiz.

```html
<body>
  <h1>Hello from HTML!</h1>
  <script src="index.js"></script>
</body>
```

```js
// index.js

console.log('Hello from JS!');
```

Endi loyihamizga vaqt va sanalar bilan osonroq ishlash uchun [**momentjs**](https://momentjs.com/) kutubxonasini qo'shamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm install moment
```

Shunda loyihamiz ko'rinishi quyidagicha bo'ladi.

```
webpack-loyiha
|- /node_modules
  |- /moment
|- /src
  |- index.html
  |- index.js
|- package-lock.json
|- package.json
```

Ko'rinib turibdiki loyihamizga `node_modules` papkasi va uning ichida biz o'rnatgan `moment` kutubxonasi qo'shildi.

Endi `index.js` fayliga quyidagi kodlarni yozamiz.

```js
// index.js

const begin = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
console.log(days);
```

Bu yerda biz `momentjs` kutubxonasidan foydalanib `begin` o'zgaruvchisiga yilning 1 - sanasini, `end` o'zgaruvchisiga bugungi sanani va `days` o'zgaruvchisiga esa yil boshidan bugunga qadar necha kun o'tganini o'zlashtirib olyapmiz.

Endi browserga o'tib console'ni tekshiramiz.

Shunda console'da `Uncaught ReferenceError: moment is not defined` degan error chiqqanini ko'ramiz. Buning sababi biz loyihamizga momentjs kutubxonasini qo'shdik-u, lekin `index.js` faylimizga uni hali qo'shmadik.

Loyihamizdagi barga javascript fayllarga momentjs'ni qo'shish uchun `index.html` faylimizga quyidagi tegni qo'shib qo'yamiz.

```html
<body>
  <h1>Hello from HTML!</h1>
  <script src="../node_modules/moment/min/moment.min.js"></script>
  <script src="index.js"></script>
</body>
```

Endi browserga o'tib console'ni tekshirsak hammasi kutilgandek ishlayotganini ko'ramiz.

Browser'da ham shu `days` o'zgaruvchisi qiymatini chiqarish uchun `index.js` ga quyidagi kodni qo'shamiz.

```js
// index.js

const start = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
console.log(days);
const content = document.querySelector('h1');
content.textContent = `Yil boshidan bugungacha ${days} kun o'tdi.`;
```

Shu bilan loyihamizni 1 - versiyasi tugaydi.

Tassavvur qiling siz kattaroq loyiha ustida ishlayapsiz va bu loyihada o'nlab kutubxonadan foydalanmoqchisiz. Buning uchun siz shu kutubxonalarning asosiy javascript fayllarini `node_modules` papkasi ichidan qidirib topib, ularni `index.html` fayliga ulab qo'yishingiz kerak bo'ladi.

Bundan tashqari ba'zi kutubxonalar boshqa yordamchi kutubxonalardan foydalangan bo'lishi mumkin. Shunda siz `index.html` faylga o'sha yordamchi kutubxonalarni ham qo'shib qo'yishingiz kerak bo'ladi.

Qolaversa har bir `index.html` ga ulangan javascript fayl to'g'ri tartibda joylashgan bo'lishi lomiz. Aks holda ko'plab `ReferenceError` lar vujudga keladi.

Buni tekshirish uchun `index.html` dagi `moment` va `index.js` script teglarini o'rinlarini almashtirib qo'yish kifoya.

```html
<body>
  <h1>Hello from HTML!</h1>
  <script src="index.js"></script>
  <script src="../node_modules/moment/min/moment.min.js"></script>
</body>
```

Mana shunday muammolarni hal qilish uchun bizga [**webpack**](https://webpack.js.org/) kerak bo'ladi. Va u bizga yuqorida sanab o'tilgan qadamlarni automatik tarzda qilib beradi.

## Webpack bilan tanishuv

Loyihamizni 2 - versiyasini webpack yordamida qayta refactor(o'zgartirish) qilib chiqamiz.

Webpack'ni o'rnatish uchun terminalda quyidagi buyruqlarni yozamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm install webpack webpack-cli --save-dev
```

Bu yerda `webpack` biz ishlatadigan kutubxona, `webpack-cli` webpack'dan terminal orqali foydalanish uchun kerak bo'ladigan tool(asbob), `--save-dev` esa ular faqat development(dasturchilar) uchun kerakligini bildiruvchi flag(bayroq).

Webpackni loyihaga qo'shib bo'lgandan so'ng, `index.html` faylimizdan `momentjs` ulangan script tegini olib tashlaymiz va uni `index.js` fayliga import qilib qo'yamiz.

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
  <head>
    <title>Webpack Loyiha</title>
  </head>
  <body>
    <h1>Hello from HTML!</h1>
    <script src="index.js"></script>
  </body>
</html>
```

```js
// index.js

import moment from 'moment';

const start = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
console.log(days);
const content = document.querySelector('h1');
content.textContent = `Yil boshidan bugungacha ${days} kun o'tdi.`;
```

Loyihamizni browser'da ochib console'ni tekshirsak bizda `Uncaught SyntaxError: Cannot use import statement outside a module` error chiqqanini ko'ramiz. Buning sababi javascript dasturlash tili xavfsizlik jihatidan import - export qobiliyati bilan design qilinmaganida.

Javascript'da shu qobilyatdan foydalana olish uchun webpack'ni ishlatamiz.

Loyihamizda webpack'ni ishlatish uchun quyidagi buyruqni teramiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npx webpack
```

Shundan so'ng loyihamizga `dist` papkasi qo'shiladi va bu papka ichida `main.js` fayli bo'ladi.

```
webpack-loyiha
|- /dist
  |- main.js
|- /node_modules
|- /src
  |- index.html
  |- index.js
|- package-lock.json
|- package.json
```

Shu `main.js` fayli loyihamizdagi barcha javascript kodlarni o'z ichiga olgan yagona fayl bo'ladi.

Endi biz uni `index.html` fayliga qo'shib qo'yamiz.

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
  <head>
    <title>Webpack Loyiha</title>
  </head>
  <body>
    <h1>Hello from HTML!</h1>
    <script src="../dist/main.js"></script>
  </body>
</html>
```

```js
// index.js

import moment from 'moment';

const start = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
console.log(days);
const content = document.querySelector('h1');
content.textContent = `Yil boshidan bugungacha ${days} kun o'tdi.`;
```

Endi browser'ga o'tib console'ni tekshirsak hammasi xuddi 1 - versiyadagidek ishlayotganini ko'ramiz.

## Webpackni konfiguratsiya qilish

Loyihamizda webpack konfiguratsiya uchun `webpack.config.js` faylini yaratib olamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ touch webpack.config.js
```

Webpack uchun sodda konfiguratsiya quyidagicha bo'ladi.

```js
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
};
```

```
webpack-loyiha
|- /dist
  |- main.js
|- /node_modules
|- /src
  |- index.html
  |- index.js
|- package-lock.json
|- package.json
|- webpack.config.js
```

Bu yerda `./src/index.js` qaralayotgan javascript fayl, `main.js` webpack hosil qilgan umumiy yagona fayl, `dist` esa `main.js` fayli joylashishi kerak bo'lgan papka.

Yuqorida sanab o'tilgan fayl va papka nomlarini ixtiyorimizga ko'ra o'zgartirsak ham bo'ladi.

Endi webpack'ni shu config fayl orqali ishlatmoqchi bo'lsak quyidagi buyruqni yozamiz.

```
foo@bar:~/Desktop/webpack-loyiha$ npx webpack --config webpack.config.js
```

Browserga o'tib loyihani tekshirib ko'rganimizda config fayl kutilayotgandek ishlashini ko'ramiz.

Har safar webpack'ni ishlatishda shunday uzun buyruqni yozmaslik uchun `package.json` faylida biror script yaratib qo'ysak bo'ladi.

```json
{
  "name": "webpack-loyiha",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack --config webpack.config.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "moment": "^2.29.4",
    "webpack": "^5.74.0",
    "webpack-cli": "^4.10.0"
  }
}
```

Hozir biz webpack'ni ishlatish uchun loyihamizga `start` script'ini qo'shdik.

Uni test qilish uchun quyidagi buyruqni yozish kerak.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm start
```

### Webpack loaders

Keling loyihamizni chiroyliroq qilish uchun unga `style.css` faylini qo'shamiz.

```console
foo@bar:~/Desktop/webpack-loyiha/src$ touch style.css
```

```
webpack-loyiha
|- /dist
  |- main.js
|- /node_modules
|- /src
  |- index.html
  |- index.js
  |- style.css
|- package-lock.json
|- package.json
|- webpack.config.js
```

```html
<!-- index.html -->

<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="style.css" />
    <title>Webpack Loyiha</title>
  </head>
  <body>
    <h1>Hello from html!</h1>
    <script src="../dist/main.js"></script>
  </body>
</html>
```

```css
/* style.css */

h1 {
  margin-top: 40px;
  text-align: center;
  font-family: sans-serif;
  color: #ca6dfc;
}
```

Ko'rinib turganidek `index.html` ga `style.css` fayli ulab qo'yildi.

Loyihamizni `npm start` buyrug'i yoramida qayta ishlatamiz va browserda biz yozgan stillar ishlayotganini ko'ramiz.

Endi `style.css` faylini `index.html` faylidan olib, uni `index.js` fayliga import qilib qo'yamiz.

```html
<head>
  <title>Webpack Loyiha</title>
</head>
```

```js
// index.js

import './style.css';
import moment from 'moment';

const start = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
console.log(days);
const content = document.querySelector('h1');
content.textContent = `Yil boshidan bugungacha ${days} kun o'tdi.`;
```

Loyihani `npm start` buyrug'i yordamida qayta ishlatamiz va terminalda webpack `./style.css` faylini tushuna olmayotganligi haqidagi error'ni ko'ramiz.

Webpack'ka `.css` ko'rinishidagi faylllarni tushunturish uchun [**css**](https://webpack.js.org/loaders/css-loader/) va [**style loader**](https://webpack.js.org/loaders/style-loader/) lardan foydalanamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm install css-loader style-loader --save-dev
```

Endi bu loader'larni webpack konfiguratsiya fayliga qo'shib qo'yamiz.

```js
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

`use: ['style-loader', 'css-loader']` bu yerda loader'larni to'g'ri tartibda kelishi muhimdir.

`npm start` buyrug'ini yozib, browser'ni tekshirsak hammasi ishlayotganini ko'ramiz.
