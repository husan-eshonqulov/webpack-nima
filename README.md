# Webpack nima?

## 1. Webpack nima uchun kerak

Tassavvur qiling ishxonamiz tomonidan bizga yangi bir loyiha qilish topshirildi. Bu loyihada biz yil boshidan bugunga qadar necha kun o'tganini hisoblab berishimiz kerak.

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

Endi `index.html` va `index.js` fayllariga _Hello from..._ yozuvlarini qo'shib qo'yamiz va browser'da ochamiz.

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

const start = moment([new Date().getFullYear(), 0, 1]);
const end = moment(new Date());
const days = end.diff(start, 'days');
console.log(days);
```

Bu yerda biz `momentjs` kutubxonasidan foydalanib `start` o'zgaruvchisiga yilning 1 - sanasini, `end` o'zgaruvchisiga bugungi sanani va `days` o'zgaruvchisiga esa yil boshidan bugunga qadar necha kun o'tganini o'zlashtirib olyapmiz.

Endi browserga o'tib console'ni tekshiramiz.

Shunda console'da `Uncaught ReferenceError: moment is not defined` degan error chiqqanini ko'ramiz. Buning sababi biz loyihamizga momentjs kutubxonasini qo'shdik-u, `index.js` faylimizga uni hali qo'shmadik.

Loyihamizdagi barga javascript fayllarga momentjs'ni qo'shish uchun `index.html` faylimizga quyidagi tegni qo'shib qo'yamiz.

```html
<body>
  <h1>Hello from HTML!</h1>
  <script src="../node_modules/moment/min/moment.min.js"></script>
  <script src="index.js"></script>
</body>
```

Endi browserga o'tib console'ni tekshirsak hammasi kutilgandek ishlayotganini ko'ramiz.

Browser'da ham shu `days` o'zgaruvchisi qiymatini chiqarish uchun quyidagi kodni yozamiz.

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

Tassavvur qiling siz kattaroq loyiha ustida ishlayapsiz va bu loyihada o'nlab kutubxonadan foydalanmoqchisiz. Buning uchun siz shu kutubxonalarni asosiy javascript faylini `node_modules` papkasi ichidan qidirib topishingiz va ularni `index.html` fayliga ulab qo'yishingiz kerak bo'ladi.

Bundan tashqari ba'zi kutubxonalar boshqa yordamchi kutubxonalardan foydalangan bo'lishi mumkin. Shunda siz `index.html` faylga o'sha yordamchi kutubxonalarni asosiy javascript fayllarini ham qo'shib qo'yishingiz kerak bo'ladi.

Qolaversa har bir `index.html` ga ulangan javascript fayl to'g'ri tartibda joylashgan bo'lishi kerak. Aks holda ko'plab `ReferenceError` lar vujudga kelishi mumkin.

Buni tekshirish uchun `index.html` dagi `moment` va `index.js` script teglarini o'rinlarini alishtirib qo'yish kifoya.

```html
<body>
  <h1>Hello from HTML!</h1>
  <script src="index.js"></script>
  <script src="../node_modules/moment/min/moment.min.js"></script>
</body>
```

Mana shunday muammolarni hal qilish uchun bizga webpack kerak bo'ladi. Va u bizga yuqorida sanab o'tilgan qadamlarni o'zi automatik qilib beradi.
