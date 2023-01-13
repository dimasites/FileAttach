# FileAttach: прикрепление файлов к ресурсам для MODx Revolution

Компонент может быть установлен из официального [репозитария MODx](https://modx.com/extras/package/fileattach)

**Модуль для загрузки файлов к ресурсам в менеджере сайта и во frontend**
Возможности:
* Административное управление всеми загруженными файлами на сайте
* Транслитерирование имен загружаемых файлов
* Для обзора загруженных файлов есть медиа источник (modx filesource), он доступен при глобальной регистрации пакета во время установки
* Список файлов хранится в отдельной таблице в БД
* Поддерживается докачка файлов
* Постраничный вывод списка файлов на фронтенде
* Поддерживается ограничение доступа к загрузке и скачиванию политиками modx

К каждому файлу можно указывать описание, режим приватности (доступность по прямой ссылке), количество скачиваний, контрольную сумму SHA1.

Файлы доступны для скачивания по прямой ссылке. Для "закрытых" файлов генерируется длинное название, не соответствующее изначальному имени файла. У "открытых" файлов имя сохраняется.

Произвольный порядок файлов в списке можно задать перетягиванием записей в редакторе на нужное место.

Поддерживает работу в СУБД MySQL (MODX Revo v2.x и 3.x) и SQLSrv DB (не поддерживается в MODX3).

Минимальная версия PHP v5.3.

Разработка компонента ведется на странице: [https://gitlab.com/13hakta/FileAttach](https://gitlab.com/13hakta/FileAttach)

Обратите внимание: дополнение несовместимо с плагинами, которые изменяют название файлов при загрузке, такими как fileTranslit.

### Снимки экрана (TBD)

* Редактор в административном режиме
* Список файлов в менеджере
* Дерево медиа источника
* Редактирование файла
* Редактирование файла в административном режиме
* Список файлов в фронтенде сайта
* Окно загрузки
* Как выглядит настройка доступа для скачивания

### Чанк FileAttachTpl

Позволяет задать произвольное оформление для вывода записей файлов.

|  Название |  Описание |
| --- | --- |
| **description** |  Описание |
| **docid** |  Идентификатор ресурса, для которого загружен файл |
| **download** |  Количество скачиваний |
| **fid** |  Уникальный строковой идентификатор файла |
| **hash** |  Контрольная сумма SHA1 |
| **ext** |  Расширение файла в нижнем регистре |
| **id** |  Идентификатор файла |
| **internal_name** |  Внутреннее имя. Содержит имя файла в файловой системе |
| **name** |  Имя файла. Совпадает с internal_name когда private=нет |
| **path** |  Путь внутри медиа источника |
| **private** |  Признак закрытости файла |
| **rank** |  Порядок в списке. Можно использовать для сортировки |
| **size** |  Размер файла в байтах |
| **tag** |  Метка |
| **timestamp** |  Метка времени в unix timestamp |

Изначальное содержание чанка:

``[[+description:notempty=`<strong>[[+description]]</strong><br/>`]]``<br>
`<a href="[[+url]]">[[+name]]</a> <span class="badge">[[+download]]</span>`<br>
``[[+size:notempty=`<br/><small>Size: [[+size]] bytes</small>`]]``<br>
``[[+ext:notempty=`<br/><small>Type: <img src="/img/[[+ext]].png" /></small>`]]``

### Сниппет FileAttach

Выводит список файлов.

|  Название |  Значение по умолчанию |  Описание |
| --- | --- | --- |
| **&ext** |  |  Фильтр по расширению файла. Работает при включении showExt |
| **&groups** |  |  Ограничение доступа для просмотра списка файлов указанным группам пользователей. Перечисление через запятую |
| **&limit** | 0 |  Ограничение вывода файлов на странице. Если не указано, то вывод всех прикрепленных файлов |
| **&totalVar** | total |  Название плейсхолдера для вывода количества записей. Работает при непустом &limit |
| **&offset** | 0 |  Количество пропущенных записей в выводе |
| **&makeURL** | false |  Создавать ссылку для скачивания файла |
| **&outputSeparator** |  |  Разделитель вывода записей |
| **&privateUrl** | false |  Форсировать использование обработчик скачиваний, что позволяет считать скачивания даже для открытых файлов |
| **&resource** | 0 |  Показать файлы для документа с номером id, если не указано, то вывод только для текущего документа |
| **&showHASH** | false |  Получить хэш файла |
| **&showExt** | false |  Извлекать расширение файла |
| **&showSize** | false |  Получать размер файла |
| **&showTime** | false |  Получать дату изменения файла |
| **&sortBy** | name |  Сортировать по полю |
| **&sortDir** | ASC |  Направление сортировки |
| **&toPlaceholder** | false |  Сохранять результат в плейсхолдер, вместо прямого вывода на странице |
| **&tag** |  |  Фильтр по метке |
| **&tpl** | FileAttachTpl |  Чанк оформления каждого ряда файлов |
| **&inline** | false |  Создавать ссылки для получения потока содержимого файла, вместо скачивания |

### Сниппет FileLink

Выводит адрес вложенного файла, который позволяет получить содержимое файла (но не скачать). Выбор файлов только из приложенных к текущему документу.

Можно использовать для встраивания изображений из вложенных файлов.

|  Название |  Значение по умолчанию |  Описание |
| --- | --- | --- |
| **&fid** |  |  Идентификатор вложенного файла |
| **&groups** |  |  Ограничение доступа для просмотра списка файлов указанным группам пользователей. Перечисление через запятую |

### Класс FileItem

#### Методы

|  Название |  Описание |  Параметры |
| --- | --- | --- |
| **generateName** |  Сгенерировать новое имя файла | length (int) = 32 |
| **getFullPath** |  Получить полный путь к файлу |  |
| **getPath** |  Получить путь к файлу относительно корня медиа источника |  |
| **getSize** |  Получить размер файла |  |
| **getUrl** |  Получить ссылку на файл |  |
| **rename** |  Переименовать файл | name (str) |
| **sanitizeName** |  Отфильтровать недопустимые комбинации символов в имени файла | name (str) |
| **setPrivate** |  Установить режим приватности | private (bool) |

### Системные настройки

|  Название |  Значение по умолчанию |  Описание |
| --- | --- | --- |
| **calchash** | false |  Вычислять контрольную сумму SHA1 при загрузке файла |
| **download** | true |  Считать количество скачиваний |
| **files_path** |  Путь |  Путь файла относительно корня медиа источника |
| **mediasource** | 1 |  Идентификатор медиа источника |
| **private** | false |  Делать файл закрытым при загрузке |
| **put_docid** | false |  Размещать файл в подкаталоге ресурса |
| **templates** |  |  Список шаблонов документов, в которых будет активирован модуль. Перечисление через запятую |
| **user_folders** | false |  Размещать файл в подкаталоге пользователя |
| **translit** | false |  Транслитерировать имена загружаемых файлов. Необходима настройка дополнения через системные настройки в разделе "friendly urls" |

### Системные события

|  Название |  Аргументы |
| --- | --- |
| **faOnRemove** | id - номер объекта в таблице,object - объект FileItem |
| **faOnUpload** | id - идентификатор ресурса |
| **faOnUploadItem** | id - идентификатор ресурса,object - объект FileItem |

### Коннектор для скачивания файлов

Закрытые файлы скачиваются через коннектор, что позволяет скрыть прямую ссылку на файл и произвести подсчет количества скачиваний. Можно скачивать открытые файлы через коннектор, указав в вызове сниппета **&privateUrl**=`1`, при этом коннектор сделает перенаправление на прямую ссылку.

Ссылка на коннектор имеет вид: `MODX_ASSETS_URL/components/fileattach/connector.php?action=web/download&ctx=web&fid=file_id`, где file_id - строковой идентификатор файла в таблице БД.

В сниппете создается ссылка с указанием контекста для текущего ресурса.

### Политики доступа

Список разрешений

|  Название |  Описание |
| --- | --- |
| **fileattach.doclist** |  Управление файлами в документе |
| **fileattach.download** |  Возможность скачивать файлы |
| **fileattach.totallist** |  Управление всеми файлами |
| **fileattach.remove** |  Удаление файлов во frontend |
| **fileattach.list** |  Получение списка файлов во frontend |

Для загрузки файлов пользователю надо включить доступ для:

* Разрешение "create" в медиа-источнике
* Разрешение "file_upload" из шаблона AdminTemplate

Для удаления файлов во frontend разрешено удалять свои файлы, прикрепленные к соответствующему ресурсу. Для возможности удаления не только своих файлов надо включить разрешение **fileattach.totallist**.

Для скачивания надо разрешить **fileattach.download**
Есть две политики, которыми можно разрешить скачивание, первая это <<File Attach Download>> предназначенная только для скачивания, и <<File Attach Frontend>> для разрешения работы во frontend.<br>
Назначьте одну из них для группы пользователя в контексте web.

При установке модуля создается набор политик:

|  Название |  Описание |
| --- | --- |
| **File Attach** |  Полный доступ |
| **File Attach Download** |  Только скачивание |
| **File Attach Frontend** |  Разрешает использовать процессоры для управления файлами во frontend |

Обратите внимание на то, что в модуле нет возможности ограничивать доступ к файлам отдельных ресурсов, отдельным файлам. В модуле реализован способ сокрытия прямого пути к файлу, ограничение на скачивание через политику задается для всех файлов.

### Пример использования

Доставьте скобки для вызова кода.

В простом случае можно просто вызвать сниппет:

`[[FileAttach]]`

Чтобы для всех файлов считалось количество скачиваний надо чтобы они открывались через приватную ссылку:

``[FileAttach? &privateUrl=`1`]]``

Сортировка по порядку, заданному вручную в менеджере:

``[[FileAttach? &sortBy=`rank`]]``

Ограничение на доступ к списку только пользователям из группы Editors:

``[[FileAttach? &groups=`Editors`]]``

Фильтр по расширению:

``[[FileAttach? &showExt=`1` &ext=`zip`]]``

Набор вложенных изображений в слайдере:

``[[FileAttach? &inline=`1` &ext=`jpg`]]``


### Управление файлами во frontend

Для загрузки используется менеджер очереди загрузки, загрузка через AJAX с помощью [FormData](https://developer.mozilla.org/ru/docs/Web/API/FormData).

---

## FileAttach

FileAttach is a tool to create file collections.
Package provides UI for MODx Manager to upload files to Resources,
manage file list, view download statistics, snippet for front-end listing.
Allows to count downloads, keep files privately (without direct url),
calculate SHA1 hash for uploaded files. Works with MediaSource.
Provides MediaSource to view tree with Resources and files attached to them.

## How to Export

First, clone this repository somewhere on your development machine:

`git clone http://github.com/13hakta/FileAttach.git ./`

Then, create the target directory where you want to create the file.

Then, navigate to the directory FileAttach is now in, and do this:

`git archive HEAD | (cd /path/where/I/want/my/new/repo/ && tar -xvf -)`

(Windows users can just do git archive HEAD and extract the tar file to wherever
they want.)

Then you can git init or whatever in that directory, and your files will be located
there!

## Configuration

Now, you'll want to change references to FileAttach in the files in your
new copied-from-FileAttach repo to whatever name of your new Extra will be. Once
you've done that, you can create some System Settings:

- 'mynamespace.core_path' - Point to /path/to/my/extra/core/components/extra/
- 'mynamespace.assets_url' - /path/to/my/extra/assets/components/extra/

Then clear the cache. This will tell the Extra to look for the files located
in these directories, allowing you to develop outside of the MODx webroot!

## Information

Note that if you git archive from this repository, you may not need all of its
functionality. This Extra contains files and the setup to do the following:

- Integrates a custom table of "FileItem"
- A snippet listing Items sorted by name and templated with a chunk
- A snippet showing link to get inline content
- A custom manager page to manage FileItem on

If you do not require all of this functionality, simply remove it and change the
appropriate code.

Also, you'll want to change all the references of 'FileAttach' to whatever the
name of your component is.

## Copyright Information

FileAttach is distributed as GPL (as MODx Revolution is), but the copyright owner
(Vitaly Chekryzhev) grants all users of FileAttach the ability to modify, distribute
and use FileAttach in MODx development as they see fit, as long as attribution
is given somewhere in the distributed source of all derivative works.
