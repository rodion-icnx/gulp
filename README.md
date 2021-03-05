# Gulp - сборка Rodion Rostovshchikov

> Используется Gulp 4

## Начало работы

Для работы с данной сборкой в новом проекте, склонируйте все содержимое репозитория <br>
`git clone <this repo>`
Затем, находясь в корне проекта, запустите команду `npm i`, которая установит все находящиеся в package.json зависимости.
После этого вы можете использовать любую из четырех предложенных команд сборки (итоговые файлы попадают в папку **app** корневой директории): <br>
`gulp` - базовая команда, которая запускает сборку для разработки, используя browser-sync

`gulp build` - команда для продакшн-сборки проекта. Все ассеты сжаты и оптимизированы для выкладки на хостинг.

`gulp cache` - команда, которую стоит запускать после `gulp build`, если вам нужно загрузить новые файлы на хостинг без кэширования.

`gulp backend` - специальная команда для создания сборки под дальнейшее бэкенд-взаимодействие. Подробнее об этом ниже.

## Структура папок и файлов

```
├── src/                          # Исходники
│   ├── js                        # Скрипты
│   │   └── main.js               # Главный скрипт
│   │   ├── components            # js-компоненты
│   │   ├── functions             # готовые, частоиспользуемые функции для сайта
│   │   ├── vendor                # папка для загрузки локальных версий библиотек
│   ├── scss                      # Стили сайта (препроцессор sass в scss-синтаксисе)
│   │   └── main.scss             # Главный файл стилей
│   │   └── global.scss           # Файл с глобальными настройками - сбросы, шрифты и т.д.
│   │   └── vendor.scss           # Файл для подключения стилей библиотек из папки vendor
│   │   └── _fonts.scss           # Файл для подключения шрифтов (можно использовать миксин)
│   │   └── _mixins.scss          # Файл для подключения миксинов из папки mixins
│   │   └── _vars.scss            # Файл для написания css- или scss-переменных
│   │   └── _settings.scss        # Файл для написания глобальных стилей
│   │   ├── components            # scss-компоненты
│   │   ├── mixins                # папка для сохранения готовых scss-компонентов
│   │   ├── vendor                # папка для хранения локальных css-стилей библиотек
│   ├── partials                  # папка для хранения html-частей страницы
│   ├── img                       # папка для хранения картинок
│   │   ├── svg                   # специальная папка для преобразования svg в спрайт
│   ├── resources                 # папка для хранения иных ассетов - php, видео-файлы, favicon и т.д.
│   │   ├── fonts                 # папка для хранения шрифтов в формате woff2
│   └── index.html                # Главный html-файл
└── gulpfile.js                   # файл с настройками Gulp
└── package.json                  # файл с настройками сборки и установленными пакетами
└── .editorconfig                 # файл с настройками форматирования кода
└── .stylelintrc                  # файл с настройками stylelint
└── README.md                     # документация сборки
```

## Оглавление

1. [Работа с html](#работа-с-html)
2. [Работа с CSS](#работа-с-css)
3. [Работа с JavaScript](#работа-с-javascript)
4. [Работа со шрифтами](#работа-со-шрифтами)
5. [Работа с изображениями](#работа-с-изображениями)
6. [Работа с иными ресурсами](#работа-с-иными-ресурсами)
7. [Backend-скрипт](#backend-скрипт)

## Работа с html

Благодаря плагину **gulp-file-include** вы можете разделять html-файл на различные шаблоны, которые должны храниться в папке **partials**. Удобно делить html-страницу на секции.

> Для вставки html-частей в главный файл используйте `@include('partials/filename.html')`

Если вы хотите создать многостраничный сайт - копируйте **index.html**, переименовывайте как вам нужно, и используйте.

При использовании команды `gulp build`, вы получите минифицированный html-код в одну строку для всех html-файлов.

## Работа с CSS

В сборке используется препроцессор **sass** в синтаксисе **scss**.

Стили, написанные в **components**, следует подключать в **main.scss**.
Стили из **\_fonts**, **\_settings**, **\_vars** и **\_mixins** подключены в **global.scss**.

Чтобы подключить сторонние css-файлы (библиотеки) - положите их в папку **vendor** и подключите в файле **vendor.scss**

Если вы хотите создать свой миксин - делайте это в папке **mixins**, а затем подключайте в файл **\_mixins.scss**.

Если вы хотите использовать scss-переменные - подключите **\_vars.scss** также в main.scss или в любое другое место, где он нужен, но обязательно удалите **:root**.

> Для подключения css-файлов используйте директиву `@import`

В итоговой папке **app/css** создаются три файла: <br> **main.css** - для стилей страницы, <br> **global.css** - для глобальных стилей, <br> **vendor.css** - для стилей всех библиотек, использующихся в проекте.

При использовании команды `gulp build`, вы получите минифицированный css-код в одну строку для всех css-файлов.

## Работа с JavaScript

Поддержка `import` и `require` не реализована! Файлы собираются автоматически из различных папок.

JS-код лучше делить на компоненты - небольшие js-файлы, которые содержат свою, изолированную друг от друга реализацию. Такие файлы помещайте в папку **components**.

В папке **functions** лежат (и будут появляться новые) файлы с готовым кодом различных часто встречающихся кейсов, будь то использование бургер-меню, реализация табов и т.д. Вам останется лишь поменять селекторы в коде на свои.

В файле **main.js** ничего не подключается, он рекомендуется для реализации общей логики сайта.

Чтобы подключить сторонние js-файлы (библиотеки) - положите их в папку **vendor**.

При использовании команды `gulp build`, вы получите минифицированный js-код в одну строку для всех js-файлов.

## Работа со шрифтами

Т.к. автор не поддерживает IE11, в сборке реализована поддержка только формата **woff2** (это значит, что в миксине подключения шрифтов используется только данный формат).

Загружайте файлы **woff2** в папку **resources/fonts**, а затем вызывайте миксин `@font-face` в файле **\_fonts.scss**.

## Работа с изображениями

Любые изображения, кроме **favicon** кладите в папку **img**.

Если вам нужно сделать svg-спрайт, кладите нужные для спрайта svg-файлы в папку **img/svg**. Иные svg-файлы просто оставляйте в папке **img**.

При использовании команды `gulp build`, вы получите минифицированные изображения в итоговой папке **img**.

## Работа с иными ресурсами

Любые ресурсы (ассеты) проекта, под которые не отведена соответствующая папка, должны храниться в папке **resources**. Это могут быть видео-файлы, php-файлы (как, например, файл отправки формы), favicon и прочие.

## Backend-скрипт

На данный момент реализована отдельная версия сборки под backend, которая похожа на `gulp build`, однако отличается отсутствием минификации итоговых файлов, что при передаче backend-специалисту просто не нужно.

Важно понимать, что если вы разрабатываете сайт под дальнейшее backend-взаимодействие, стоит сразу разбивать css-файлы по-другому. Вместо использования папки **components**, создавайте css-файлы напрямую рядом с **main.scss**, подключайте к html-файлу.

Также важно, что при таком взаимодействии нужно подключать js-файлы из папки **components** также напрямую к html-файлу.
