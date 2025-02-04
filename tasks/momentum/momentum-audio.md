## Советы по выполнению задания Momentum
- [ссылка на задание](momentum.md)

## 6. Аудиоплеер
#### Добавление звука на веб-страницу.
С появлением HTML5 добавление аудиоплеера на страницу превратилось в рядовую задачу. Для этого достаточно в html-файл добавить код плеера, и в атрибуте `src` указать ссылку на аудио-файл.
```html
<audio src="" controls></audio>
```
[Пример 1 на codepen](https://codepen.io/irinainina/pen/poerqar)

Внешний вид встроенного в браузер аудиоплеера в разных браузерах выглядит немного по-разному и очень плохо стилизуется. Если нужно создать плеер с определённым дизайном, в приведённом выше коде необходимо убрать атрибут `controls`, который отвечает за отображение плеера и написать js-код для его проигрывания:
```js
const audio = document.querySelector('audio');
function playAudio() {
  audio.currentTime = 0;
  audio.play();
}
function pauseAudio() {
  audio.pause();
}
```
[Пример 2 на codepen](https://codepen.io/irinainina/pen/RwoKVBW)

Функции `playAudio()` и `pauseAudio()` достаточно простые. Первая запускает проигрывание плеера используя метод `play()`, вторая - останавливает проигрывание, используя метод `pause()`. Строка `audio.currentTime = 0` указывает, что аудиотрек при каждом запуске функции `playAudio()` будет проигрываться с начала.

Есть возможность создать плеер средствами JavaScript:
```js
const audio = new Audio();

function playAudio() {
  audio.src = // ссылка на аудио-файл;
  audio.currentTime = 0;
  audio.play();
}
```
[Пример 3 на codepen](https://codepen.io/irinainina/pen/dyvzrNV)

#### Объединение проигрывания и остановки звука в одной функции. Флаги
У большинства плееров используется одна кнопка для проигрывания и остановки воспроизведения. Если звука нет, клик по ней запускает проигрывание звука, если звук есть - останавливает. Это юзер-френдли - пользователю так удобнее. 

Чтобы реализовать такую возможность, нам необходимо объединить функции `playAudio()` и `pauseAudio()` в одну. Для создания такой функции нам прежде всего необходимо выяснить, играет ли в данный момент музыка. Для этого в глобальной области видимости, там, где находим элементы DOM и разместили переменную `randomNum`, создадим переменную `isPlay`.

Такие переменные, названия которых начинаются с `is`, называются флагами. Флаг может принимать только два значения: `true` или `false`. С помощью флагов проверяем наличие или отсутствие чего-либо. В данном случае проверяем проигрывание в данный момент звука.

Когда мы только открываем страницу, звука нет. Поэтому переменную `isPlay` создаём с значением `false`:
```js
let isPlay = false;
```
При клике по кнопке `Play audio` необходимо не только запустить проигрывание звука, но и изменить значение переменной, указав, что теперь `isPlay = true`.  
При клике по кнопке `Stop audio` останавливаем проигрывание звука и указываем, что `isPlay = false`.

Зная, когда у нас проигрывается звук, а когда нет, можем создать видоизменённую функцию `playAudio()` которая будет по-разному работать в зависимости от того, проигрывается ли в данный момент звук. Если звука нет `if(!isPlay)`, запускаем проигрывание, иначе - останавливаем. Такую функцию вам предстоит написать самостоятельно.

#### Объединение кнопок проигрывания и остановки звука в одну
Теперь, когда у нас есть одна функция, которая умеет и проигрывать, и останавливать звук, в плеере достаточно оставить только одну кнопку для запуска данной функции. Наша задача научиться менять внешний вид кнопки в зависимости от того, проигрывается в данный момент звук или нет.

Раньше, работая над слайдером, мы научились менять стиль элемента используя свойство `style`. Возможность изменения стиля элемента средствами JavaScript применяем только если нет других вариантов изменить стиль, например, если его значение получаем динамически в результате выполнения js-кода. Для всех остальных случаев используем JavaScript для добавления или удаления у элемента класса, для которого в css-коде прописан нужный стиль.

В css-коде создан класс `pause` для которого фоновым изображением указана иконка остановки звука. Для того, чтобы на кнопке проигрывания и остановки звука иконка проигрывания менялась на иконку остановки, нам необходимо добавлять кнопке класс `pause`, когда звук воспроизводится, и убирать, когда звука нет. Для этого используем метод `classList`.

#### Метод `classList`
Метод `classList` предназначен для работы с классами и поддерживается всеми современными браузерами:
- `element.classList.add('class');` - добавляет элементу класс;
- `element.classList.remove('class');` - удаляет класс;
- `element.classList.toggle('class');` - переключает класс: добавляет, если класса нет, и удаляет, если он есть.
- `element.classList.contains('class');` - проверяет, есть ли данный класс у элемента (возвращает `true` или `false`)

Обратите внимание. Так как метод `classList` работает только с классами, то в кавычках внутри может находиться только название класса. И вот перед этим названием класса **точка не ставится никогда**. Эту ошибку часто совершают начинающие разработчики.

Для изменения стиля кнопки при клике удобно использовать метод `classList.toggle();`
```js
function toggleBtn() {
  button.classList.toggle('pause');
}
button.addEventListener('click', toggleBtn);
```
[Пример на codepen](https://codepen.io/irinainina/pen/NWpwdMe)

Простая логика переключения класса при клике усложняется тем, что кроме кнопки `play` есть ещё кнопки `play-next` и `play-prev`, при клике по которым тоже воспроизводится музыка. Функцию для переключения класса `pause` пишем по той же логике, по которой написали видоизменённую функцию `playAudio()` - проверяем состояние флага `isPlay` и в зависимости от этого добавляем или удаляем у кнопки класс `pause` при помощи методов `classList.add()`, и `classList.remove()`

#### Треки можно пролистывать кликами по кнопкам
Функция пролистывания треков очень похожа на функцию пролистывания изображений, которую написали в процессе создания слайдера. Нам точно так же нужно создать глобальную переменную `playNum`. Присвоим ей значение `0`, так как вначале проигрывания воспроизводится первый трек. Создадим две функции `playNext()` и `playPrev()`, которые изменяют значение переменной `playNum`, увеличивая или уменьшая его. Внутри функций `playNext()` и `playPrev()` вызываем видоизменённую функцию `playAudio()`, которая будет проигрывать соответствующий трек. В следующем пункте рассмотрим как получить список треков для воспроизведения.

#### Создаём список воспроизведения. Модули в JavaScript
Было бы хорошо предусмотреть для пользователя возможность редактировать плейлист - добавлять в него новые треки, удалять старые. Но редактирование пользователем основного кода приложения не очень хорошая идея. Список треков нужно вынести в отдельный файл. Назовём его playList.js. Подключить такой файл можно непосредственно в index.html, и это будет работать. Но в какой-то момент у нас может оказаться несколько десятков файлов и нам всё равно придётся использовать модули. Попробуем это сделать уже сейчас. Нам необходимо подключить playList.js в index.js. Посмотрим, как это можно сделать.

Создадим файл playList.js
```js
const playList = [
  {      
    title: 'Aqua Caelestis',
    src: '../assets/sounds/Aqua Caelestis.mp3',
    duration: '00:58'
  },  
  {      
    title: 'River Flows In You',
    src: '../assets/sounds/River Flows In You.mp3',
    duration: '03:50'
  }
]
export default playList;
```
Последняя строчка в коде - экспорт по умолчанию. Такой экспорт в файле может быть только один.

Подключаем данный файл в `index.js`, в консоли можно увидеть результат подключения
```js
import playList from './playList.js';
console.log(playList);
```

Теперь, чтобы в функции `playAudio()` указать какой трек воспроизводить, вместо ссылки на трек достаточно указать его номер в плейлисте: 
```js
audio.src = playList[playNum].src;
```

Обратите внимание:
- при подключении в `index.html` js-файла с импортом, необходимо указать `type="module"`  
  `<script type="module" src="js/index.js"></script>`
- файл с импортом будет работать только на сервере. Live Server подходит.

#### Плейлист создаётся средстави JavaScript
Перед разработчиком часто возникает задача сгенерировать html-элементы средствами JavaScript. Как, например, в этом случае: так как пользователь может редактировать плейлист, мы не знаем заранее ни количество треков, ни их название. Список воспроизведения должен создаваться на основе данных, которые содержит экспортированный из файла `playList.js` массив `playList`.

Нам необходимо:
1. создать на странице элемент `li` - один из пунктов списка воспроизведения
2. добавить этому элементу класс 'play-item'
3. добавить этому элементу текстовое содержимое - название трека
4. добавить созданный элемент `li` в уже существующий на странице элемент `ul` с классом 'play-list'
5. Повторить указанную последовательность действий для всех элементов массива `playList`

Решение
1. создать элемент средствами js позволяет метод `createElement()`
```js
const li = document.createElement('li');
```
2. для добавления класса используем уже известный нам метод `classList.add()`
3. для добавления названия трека используем свойство `textContent`
4. для добавления элемента в другой элемент, который уже есть на странице, используем метод `append()`
```js
playListContainer.append(li)
```
4. Чтобы повторить указанную последовательность действий для каждого элемента массива `playList`, нужно перебрать все элементы массива. Самый простой и понятный начинающим способ - перебор элементов массива в цикле
```js
  for(let i = 0; i < playList.length; i++) {
    // здесь ваш код
  }
```
Более современный и удобный вариант - пройтись по массиву свойством `forEach`
```js
  playList.forEach(el => {
    // здесь ваш код
  })
```

#### Ключевые навыки, которые вы приобрели:
- добавление звука на страницу
- использование флагов
- метод `classList`
- модули в JavaScript
- создание элемента средствами JavaScript
- создали аудиоплеер