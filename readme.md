## Сайт с пронумерованными анекдотами

### Запросы к Api
Для вызова __Api__ необходимо отправить __POST__-запрос на _server.php_ содержащий параметр `method` с названием вызываемого метода и параметры запроса для соответствующего метода (если они есть)

> Обработка запроса последовательная, то есть, если требуется запись в __БД__, она завершится перед тем, как будет сформирован ответ от __Api__.

### Ответы Api-сервера
Сервер присылает ответы в формате __JSON__

> Если параметр `method` не передан или не существует, будет возвращена ошибка:  
`0: Неверный запрос к Api`

#### TODO
* Обработка и возвращение ошибок в методах __Api__
* Некоторые операции на сервере можно запускать в других потоках (ускорение __Api__)

#### Дополнительные типы данных
```javascript
{natural} - числовой ({number}), целый, положительный (натуральные числа)
```

### Описание методов Api
пример
```javascript
$.ajax({                                      // параметры запроса (общие для всех вызовов Api)
  url   : 'server.php',
  method: 'post'
})
.ask({                                        // запрос Api
  method: 'tag.add',
  name  : 'Поручик Ржевский'
})
.try(callback)                                // Обработка ответа
.err(() => alert('Не удалось сохранить тег')) // Обработка ошибки (если возникнет)
```

#### Секция `tag`: Работа с тегами

##### `tag.all` - список всех тегов
параметры: [отсутствуют]

результат: массив тегов, каждый элемент - объект с полями `{id, name, hide}`

##### `tag.add` - Добавление нового тега: $response = Tag::add(_::str('name'));   break; // Добавление нового тега
параметры: `{string} name` - название тега

результат: массив тегов, каждый элемент - объект с полями `{id, name, hide}` (аналогично методу `tag.all`)

#### Секция `anekdot`: Работа с анекдотами

##### `anekdot.all` - список всех анекдотов
параметры: [отсутствуют]

результат: массив анекдотов, каждый элемент - объект с полями `{id, number, caption}`

##### `anekdot.add` - добавление анекдота
параметры:  
```javascript
{string} caption - название анекдота
{natural} number - номер анекдота (необязательно, вроде... не помню)
{string} text    - текст анекдота (первая версия)
{string} name    - название анекдота
```

результат: массив анекдотов, каждый элемент - объект с полями `{id, number, name}` (аналогично методу `anekdot.all`)

##### `anekdot.get` - информация об анекдоте (возвращает анекдот со всеми версиями и т.д.)
параметры: `{natural} id` - идентификатор анекдота
результат: объект с информацией об анекдоте
```javascript
{
  id: ИДЕНТИФИКАТОР АНЕКДОТА,
  caption: НАЗВАНИЕ АНЕКДОТА,
  number: НОМЕР АНЕКДОТА (если есть),
  version: [ // СПИСОК ВЕРСИЙ АНЕКДОТА
    {anekdot, name, text}
  ],
  tag: [
    СПИСОК ТЕГОВ АНЕКДОТА
  ]
}
```

##### `anekdot.upd` - добавление версии текста анекдота
параметры:
```javascript
{natural} anekdot - идентификатор анекдота
{string}  version - версия текста анекдота
{string}  name    - название версии анекдота
```

результат: массив версий текстов анекдота (@todo? возвращать полную информацию об анекдоте)

##### `anekdot.num` - установка номера анекдота
параметры:
```javascript
{natural} anekdot - идентификатор анекдота
{natural} number  - устанавливаемый номер
```

###### TODO: запретить добавление одинаковых номеров (зависит от: Обработка и возвращение ошибок в методах Api)

результат: информация об анекдоте (аналогично методу `anekdot.get`)

#### roadmap (список методов для реализации)
