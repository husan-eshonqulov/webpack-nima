# Webpack nima?

## Webpack nima uchun kerak

Tassavvur qiling ishxonamiz tomonidan bizga yangi bir loyiha qilish topshirildi. Bu loyihada biz yil boshidan bugunga qadar necha kun o'tganini hisoblashimiz kerak.

Keling kompyuterimizda yangi loyiha yaratib olamiz. Buning uchun terminalda quyidagi buyruqlarni yozamiz.

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

Shunda loyihamiz quyidagi ko'rishishda bo'ladi.

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

Bu yerda biz `momentjs` kutubxonasidan foydalanib `begin` o'zgaruvchisiga yilning 1 - sanasini, `end` o'zgaruvchisiga bugungi sanani va `days` o'zgaruvchisiga yil boshidan bugunga qadar necha kun o'tganini o'zlashtirib olyapmiz.

Endi browserga o'tib console'ni tekshiramiz.

Shunda console'da `Uncaught ReferenceError: moment is not defined` degan error chiqqanini ko'ramiz. Buning sababi biz loyihamizga momentjs kutubxonasini qo'shdik-u, lekin `index.js` faylimizga uni hali qo'shmadik.

Loyihamizdagi barga javascript fayllarga momentjs'ni qo'shish uchun `index.html` fayliga quyidagi tegni qo'shib qo'yamiz.

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

Tassavvur qiling siz kattaroq loyiha ustida ishlayapsiz va bu loyihada o'nlab kutubxonadan foydalanmoqchisiz. Buning uchun siz shu kutubxonalarning javascript fayllarini `node_modules` papkasi ichidan qidirib topib, ularni `index.html` fayliga ulab qo'yishingiz kerak bo'ladi.

Bundan tashqari ba'zi kutubxonalar boshqa yordamchi kutubxonalardan foydalangan bo'lishi mumkin. Shunda siz `index.html` faylga o'sha yordamchi kutubxonalarni ham qo'shib qo'yishingiz kerak bo'ladi.

Qolaversa har bir `index.html` ga ulangan javascript fayl to'g'ri tartibda joylashgan bo'lishi lomiz. Aks holda ko'plab `ReferenceError` lar vujudga keladi.

Buni tekshirish uchun `index.html` dagi `momentjs` va `index.js` script teglarini o'rinlarini almashtirib qo'yish kifoya.

```html
<body>
  <h1>Hello from HTML!</h1>
  <script src="index.js"></script>
  <script src="../node_modules/moment/min/moment.min.js"></script>
</body>
```

Mana shunday muammolarni hal qilish uchun bizga [**webpack**](https://webpack.js.org/) kerak bo'ladi. Va u bizga yuqorida sanab o'tilgan qadamlarni automatik tarzda qilib beradi.

## Webpack bilan tanishuv

Keling loyihamizni 2 - versiyasini webpack yordamida qayta refactor(o'zgartirish) qilib chiqamiz.

Webpack'ni o'rnatish uchun terminalda quyidagi buyruqlarni yozamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm install webpack webpack-cli --save-dev
```

Bu yerda `webpack` biz ishlatadigan kutubxona, `webpack-cli` webpack'dan terminal orqali foydalanish uchun kerak bo'ladigan tool(asbob), `--save-dev` esa ular faqat development(dasturchilar) uchun kerakligini bildiruvchi flag(bayroq).

Webpackni loyihaga qo'shib bo'lgandan so'ng, `index.html` faylidan `momentjs` ulangan script tegini olib tashlaymiz va uni `index.js` import qilib qo'yamiz.

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

Loyihamizni browser'da ochib console'ni tekshirsak bizda `Uncaught SyntaxError: Cannot use import statement outside a module` degan error chiqqanini ko'ramiz. Buning sababi javascript dasturlash tili xavfsizlik jihati tomonidan import - export qobiliyati bilan design qilinmagan.

Javascript'da shu qobilyatdan foydalana olish uchun webpack'ni ishlatamiz.

Loyihamizda webpack'ni ishlatish uchun quyidagi buyruqni yozamiz.

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

Shu `main.js` fayli loyihamizdagi barcha javascript kodlarni o'z ichiga olgan yagona fayl hisoblanadi.

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

Bu yerda `./src/index.js` qaralayotgan javascript fayl, `main.js` webpack hosil qiladigan umumiy yagona fayl, `dist` esa `main.js` fayli joylashadigan papka.

Yuqorida sanab o'tilgan fayl va papka nomlarini ixtiyoriy tarzda o'zgartirish mukin.

Endi webpack'ni shu config fayl orqali ishlatmoqchi bo'lsak quyidagi buyruqni yozamiz.

```
foo@bar:~/Desktop/webpack-loyiha$ npx webpack --config webpack.config.js
```

Browserga o'tib loyihani tekshirib ko'rganimizda config fayl kutilayotganidek ishlashini ko'ramiz.

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

Uni test qilish uchun quyidagi buyruqni yozamiz.

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
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

> `use: ['style-loader', 'css-loader']` bu yerda loader'larni to'g'ri tartibda kelishi muhimdir.

`npm start` buyrug'ini yozib, browser'ni tekshirsak hammasi yaxshi ishlayotganini ko'ramiz.

Agar `bootstrap` ishlatmoqchi bo'lsak uni loyihaga qo'shib, `index.js` ga import qilib qo'yamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm install bootstrap
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
    <h1 class="mt-5">Hello from html!</h1>
    <script src="../dist/main.js"></script>
  </body>
</html>
```

```js
// index.js

import 'bootstrap/dist/css/bootstrap.css';
import './style.css';
import moment from 'moment';

const start = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
console.log(days);
const content = document.querySelector('h1');
content.textContent = `Yil boshidan bugungacha ${days} kun o'tdi.`;
```

Ko'rinib turganidek `h1` elementiga `mt-5` klasini qo'shdik.

Loyihani `npm start` yordamida yana qayta ishlatamiz.

Endi bootstrap'ni browser'da ishlayotganini ko'rishimiz mumkin.

### Cache Busting va Plugins

Loyihamiz tezligini oshirish maqsadida unga cache qo'shamiz. Shunda har safar foydalanuvchi serverga so'rov jo'natganda birinchi browserdagi cache qaraladi. Agar so'ralayotgan narsa cache'da mavjud bo'lsa so'rovga javob cache'dan qaytariladi. Aks holda so'rov serverga jo'natiladi.

Buning uchun webpack konfiguratsiya faylini quyidagicha o'zgartiramiz.

```js
// webpack.config.js

const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

Yuqorida `filename: 'main.[contenthash].js'` o'zgarishi qilindi.

Endi yana `npm start` orqali loyihani qayta ishlatamiz va `dist` papkasi ichiga qaraymiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm start
```

```
webpack-loyiha
|- /dist
  |- main.8afc1a3a5aca6dea3226.js
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

Shunda `dist` papkasida `main.8afc1a3a5aca6dea3226.js` shunga o'xshash fayl hosil bo'ladi.

Endi loyihani yana qayta ishlatamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm start
```

Shunda `dist` papkasi ichida hech qanday yangi fayl hosil bo'lmaganini ko'ramiz.

Endi `index.js` faylidagi `console.log(days);` ni olib tashlab loyihani qayta ishlatamiz.

```js
// index.js

import 'bootstrap/dist/css/bootstrap.css';
import './style.css';
import moment from 'moment';

const start = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
const content = document.querySelector('h1');
content.textContent = `Yil boshidan bugungacha ${days} kun o'tdi.`;
```

```console
foo@bar:~/Desktop/webpack-loyiha$ npm start
```

Shunda `dist` papkasi ichida yangi `main.6dc4374aa93648374bf3.js` shunga o'xshagan fayl qo'shilganligini ko'ramiz.

Ya'ni qachonki loyihamizdagi biror javascript fayl o'zgarsa webpack qayta ishlamoqda.

Har safar webpack har xil nomdagi javascript fayl yaratganligi uchun uni `index.html` ga hard code qilib ulab bo'lmaydi.

Bu narsani automatik qilish uchun bizga [**Html Webpack Plugin**](https://webpack.js.org/plugins/html-webpack-plugin/) yordan beradi.

Uni loyihamizga qo'shamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm install html-webpack-plugin --save-dev
```

Endi `src` papkasida yangi `template.html` degan fayl yaratib olamiz va unga `index.html` dagi asosiy content'larni ko'chiramiz.

```console
foo@bar:~/Desktop/webpack-loyiha/src$ touch template.html
```

```html
<!-- teplate.html -->

<!DOCTYPE html>
<html>
  <head>
    <title>Webpack Loyiha</title>
  </head>
  <body>
    <h1 class="mt-5">Hello from html!</h1>
  </body>
</html>
```

Endi bizga `index.html` va `dist` kerak bo'lmaydi, ularni o'chiramiz.

```
webpack-loyiha
|- /node_modules
|- /src
  |- index.js
  |- style.css
  |- template.html
|- package-lock.json
|- package.json
|- webpack.config.js
```

Keyin webpack konfiguratsiyasini quyidagicha o'zgartiramiz.

```js
// webpack.config.js

const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/template.html',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

Endi `npm start` orqali loyihani qayta ishlatganimizda `dist` papkasi ichida `main.6dc4374aa93648374bf3.js` shunga o'xshash javascript fayl va `index.html` degan html fayl hosil bo'lganini ko'ramiz.

`index.html` faylini ochib qarasak `main.6dc4374aa93648374bf3.js` fayli automatik tarzda script tegi orqali unga ulab qo'yilganligini ko'ramiz.

Endi shu `index.html` ni browserda ochib qarasak hammasi yaxshi ishlayotganligini ko'ramiz.

### Development va Production

Development va production uchun loyihamizda `webpack.common.js`, `webpack.dev.js` va `webpack.prod.js` fayllarini yaratib olamiz va `webpack.config.js` faylini o'chiramiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ touch webpack.common.js
foo@bar:~/Desktop/webpack-loyiha$ touch webpack.dev.js
foo@bar:~/Desktop/webpack-loyiha$ touch webpack.prod.js
```

```
|- /dist
|- /node_modules
|- /src
  |- index.js
  |- style.css
  |- template.html
|- package-lock.json
|- package.json
|- webpack.common.js
|- webpack.dev.js
|- webpack.prod.js
```

Endi bu fayllarga quyidagicha o'zgartirish qilamiz.

```js
// webpack.common.js

const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/template.html',
    }),
  ],
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
    ],
  },
};
```

```js
// webpack.dev.js

const path = require('path');
const common = require('./webpack.common');
const { merge } = require('webpack-merge');

module.exports = merge(common, {
  mode: 'development',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, 'dist'),
  },
});
```

```js
// webpack.prod.js

const path = require('path');
const common = require('./webpack.common');
const { merge } = require('webpack-merge');

module.exports = merge(common, {
  mode: 'production',
  output: {
    filename: 'main.[contenthash].js',
    path: path.resolve(__dirname, 'dist'),
  },
});
```

Endi `packge.json` faylimizni `scripts` bo'limini quyidagicha o'zgartiramiz.

```json
"scripts": {
    "start": "webpack --config webpack.dev.js",
    "build": "webpack --config webpack.prod.js"
  },
```

Bu yerda `start` script'iga `webpack.dev.js`, `build` script'iga esa `webpack.prod.js` webpack konfiguratsiyasi fayllari ulab qo'yilyapti.

Bundan tashqari `webpack.dev.js` va `webpack.prod.js` fayllarini `webpack.common.js` fayli bilan bog'lashda `merge` funksiyasi ishlatilyapti. Shu funksiyani loyihamizga qo'shish uchun quyidagi buyruqni yozamiz.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm install webpack-merge --save-dev
```

Endi `npm start` va `npm run buil` script'lari orqali loyihamizni ishlatsak bo'ladi.

Bunda `npm start` development, `npm run build` esa production uchun ishlatiladi.

```console
foo@bar:~/Desktop/webpack-loyiha$ npm start
```

```console
foo@bar:~/Desktop/webpack-loyiha$ npm run build
```
