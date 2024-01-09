# **Шпаргалка по Git**

# **Git - что это?**

Git - это быстрая, масштабируемая, распределенная система управления версиями с необычно богатым набором команд, который обеспечивает как высокоуровневые операции, так и полный доступ к внутренним компонентам.

Git - это проект с открытым исходным кодом. Первоначально он был написан Линусом Торвальдсом с помощью группы хакеров по всей сети. 

Само название "git" создатель объяснил тем, что описал инструмент как "глупый трекер контента", а название как (в зависимости от вашего настроения):

* случайная комбинация из трех букв, которая произносится и на самом деле не используется какой-либо общей командой UNIX. Тот факт, что это неправильное произношение "get" может быть или не быть актуальным.
* stupid. contemptible and despicable. simple. Выберайте на свой вкус.
"глобальный информационный трекер" (global information tracker): вы в хорошем настроении, и это действительно работает для вас. Ангелы поют, и свет внезапно заполняет комнату.
* "чертов идиотский грузовик дерьма" (goddamn idiotic truckload of sh*t): когда он ломается

# **Список команд:**

## **1. Работа  с репозиторием**

```
$ cd ~/dev/first-project # перешли в нужную папку
$ git init # создали репозиторий
$ cd <папка с репозиторием> # перешли в папку
$ rm -rf .git # удалили подпапку .git
$ touch todo.rtf
$ touch readme.rtf
# создали файлы todo.rtf и readme.rtf
$ git status # проверили статус
$ git add --all # подготовили к сохранению все файлы в репозитории
$ git status # проверили статус
```

Файлы, которые отмечены зелёным, теперь отслеживаются и готовы к сохранению. Но сохранения пока не произошло, потому что команда `git` add только запоминает текущее содержимое (контент) файла.

Если сейчас отредактировать любой из «зелёных» файлов в папке `first-project`, он перейдёт в состояние `modified` (англ. «изменённый») и будет и в «зелёном», и в «красном» списках. 

Например, откройте файл `todo.rtf` в любом редакторе (подойдёт даже блокнот) и напишите в нём: `1. Пройти пару уроков по Git.`.

Сохраните изменения, а затем снова вызовите команду `git status` в консоли.

Файл `todo.rtf` теперь есть и в «зелёном», и в «красном» списках:

*зелёным отмечена пустая версия файла — в таком виде он был во время последнего запуска команды git add;
*красным отмечена версия с текстом `1. Пройти пару уроков по Git.`.

Чтобы запомнить новое состояние файла, нужно снова ввести команду `git add` и передать в качестве параметра имя изменённого файла или ключ `--all`.

```
$ git add todo.rtf
# или
$ git add --all
```
Теперь файл `todo.rtf` снова готов к сохранению! Будет сохранена последняя добавленная версия с текстом `1. Пройти пару уроков по Git.`.

## **2. Работа с коммитом**

**Коммит** — это одна из основных сущностей в Git (и в других системах контроля версий). Коммит гарантирует, что изменения будут сохранены в истории и при необходимости к ним можно будет «откатиться». Это как если бы вы могли выполнить операцию `Ctrl+Z` для целой папки (репозитория).

```
$ git commit -m 'Мой первый коммит!'
```
где ключ `-m` позволяет присвоить коммиту сообщение. Помните, что такие сообщения должны быть информативными: чётко описывать изменения.

Для просмотра истории коммитов — `git log`. По умолчанию `git log` выводит коммиты в обратном хронологическом порядке — последние коммиты оказываются первыми сверху. В этом можно убедиться, если посмотреть на дату и время их создания.

# **Git и платформы для удалённой работы**

_Нужно помнить, что Git и GitHub — это два разных проекта, которые развиваются независимо друг от друга_. 

Git:
- консольный инструмент для работы с локальными и удалёнными репозиториями;
- проект с открытым исходным кодом.

GitHub:
- платформа для размещения удалённых репозиториев;
- принадлежит компании Microsoft.

## **Привязать удалённый репозиторий к локальному** 

Перейдите на страницу удалённого репозитория, выберите тип `SSH` и скопируйте `URL`. Кнопка справа позволит сделать это мгновенно.

Откройте консоль, перейдите в каталог локального репозитория и введите команду `git remote add` (от англ. _remote_ — «удалённый» и _add_ — «добавить»). 

```
$ cd ~/dev/first-project
$ git remote add origin git@github.com:%ИМЯ_АККАУНТА%/first-project.git
```

Команде необходимо передать два параметра: имя удалённого репозитория и его URL. В качестве имени используйте слово `origin`. А URL вы скопировали со страницы удалённого репозитория.

`origin` (англ. «источник») — стандартный псевдоним, с помощью которого можно обращаться к главному удалённому репозиторию (обычно такой репозиторий один). Это значительно упрощает работу.

## **Убедиться, что репозитории связаны**

```
$ git remote -v
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (fetch)
origin    git@github.com:%ИМЯ_АККАУНТА%/%ИМЯ-ПРОЕКТА%.git (push)
```
где флаг `-v` — короткая форма флага `--verbose` (англ. «подробный»). Он позволяет показать больше информации в выводе.

## **Синхронизация репозиториев**

```
$ git push -u origin main # загрузить содержимое локального репозитория на GitHub.
```
При этом во время взаимодействия с удалёнными репозиториями Git выводит в консоль отладочную информацию: количество объектов (файлов), которые отправляются на сервер, информацию о прогрессе сжатия и записи и так далее.

## **Файл README.md**

Чтобы другие пользователи, а также потенциальные клиенты или работодатели могли понять, что представляет собой проект, его нужно описать. Такое описание принято указывать в файле `README.md` (от англ. read — «прочитай» и me — «меня»).

Как правило, в `README.md` проекта можно найти следующую информацию:

* Название проекта и его краткое описание: кем создан, для чего, какие решает задачи и какие закрывает проблемы.
* Технологии, которые применяются в проекте. В чём его отличие от аналогичных.
* Документация проекта — подробная инструкция о том, что представляет собой проект.
* Планы проекта, если они есть.

_Кстати, это **текстовый файл**, который можно создать командой `touch`, а затем редактировать так же, как и любой другой текстовый документ. Например, в Visual Studio Code._

Полезные ссылки по базовому синтаксису маркдауна:

[https://www.markdownguide.org/cheat-sheet/]: На английском
[https://gist.github.com/fomvasss/8dd8cd7f88c67a4e3727f9d39224a84c]:  На русском

Загружаем получившийся репозиторий на GitHub. Не забываем проверить, что локальная и удалённая версии идентичны.



# **Навигация по коммитам. Статусы файлов**

## **Хеш - идентификатор коммита**


**Хеширование** (от англ. hash, «рубить», «крошить», «мешанина») — это способ преобразовать набор данных и получить их «отпечаток» (англ. fingerprint).

Информация о коммите — это набор данных: когда был сделан коммит, содержимое файлов в репозитории на момент коммита и ссылка на предыдущий, или **родительский** (англ. parent), коммит.

Git хеширует (преобразует) информацию о коммите с помощью алгоритма SHA-1 (от англ. Secure Hash Algorithm — «безопасный алгоритм хеширования») и получает для каждого коммита свой уникальный хеш — результат хеширования.

Обычно хеш — это короткая (40 символов в случае SHA-1) строка, которая состоит из цифр `0—9` и латинских букв `A—F` (неважно, заглавных или строчных). Она обладает следующими важными свойствами:
* если хеш получить дважды для одного и того же набора входных данных, то результат будет гарантированно одинаковый;
* если хоть что-то в исходных данных поменяется (хотя бы один символ), то хеш тоже изменится (причём сильно).

Git хранит таблицу соответствий `хеш → информация о коммите`. Если вы знаете хеш, вы можете узнать всё остальное: автора и дату коммита и содержимое закоммиченных файлов. Можно сказать, что хеш — основной идентификатор коммита.

При работе с Git хеши будут встречаться вам регулярно. Их можно будет передавать в качестве параметра разным Git-командам, чтобы указать, с каким коммитом нужно произвести то или иное действие.

Все хеши и таблицу `хеш → информация о коммите` Git сохраняет в служебные файлы. Они находятся в скрытой папке ``.git`` в репозитории проекта.
