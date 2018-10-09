## Добавление сообщения
**Адрес сервера:**
```
https://имя_хоста/zmessage.php
```
**JSON-документ:**
```json
{
	"login":"логин",
	"password":"Пароль",
	"login_user":"Логин пользователя",
	"sign":"Проверочный код",
	"messages":[
		{
			"type":"1",
			"sell":"0.55",
			"buy":"0.35",
			"originator":"mail1@mail.ru",
			"email":"mail2@mail.ru",
			"type_email":"text/plain",
			"theme_email":"Тема письма",
			"text_email":"Текст письма",
			"state":"partly_deliver",
			"time":"2018-10-03 12:00:00",
			"id_message":"1001",
			"name_delivery":"Рассылка 1"			
		},
		{
			"type":"1",
			"sell":"0.55",
			"buy":"0.35",
			"originator":"mail1@mail.ru",
			"email":"mail3@mail.ru",
			"type_email":"text/plain",
			"theme_email":"Тема письма",
			"text_email":"Текст письма",
			"state":"partly_deliver",
			"time":"2018-10-03 12:00:00",
			"id_message":"1002",
			"name_delivery":"Рассылка 2"
		}
	]
}
```
**PHP-данные:**
```php
$param = array(
    'login' => 'логин',
	'password' => 'Пароль',
	'login_user' => 'Логин пользователя',
	'sign' => 'Проверочный код',
    'messages' => array(
        array(
            'type' => '1',
            'sell' => '0.55',
            'buy' => '0.35',
			'originator' => 'mail1@mail.ru',
			'email' => 'mail2@mail.ru',
			'type_email' => 'text/plain',
			'theme_email' => 'Тема письма',
			'text_email' => 'Текст письма',
			'state' => 'partly_deliver',
			'time' => '2018-10-03 12:00:00',
			'id_message' => '1001',
			'name_delivery' => 'Рассылка 1'
        ),
		array(
            'type' => '1',
            'sell' => '0.55',
            'buy' => '0.35',
			'originator' => 'mail1@mail.ru',
			'email' => 'mail3@mail.ru',
			'type_email' => 'text/plain',
			'theme_email' => 'Тема письма',
			'text_email' => 'Текст письма',
			'state' => 'partly_deliver',
			'time' => '2018-10-03 12:00:00',
			'id_message' => '1002',
			'name_delivery' => 'Рассылка 2'
        )
    )
);
```

Где:
* **login** - ваш логин в системе;
* **password** - ваш пароль в системе;
* **login_user** - Логин пользователя которому необходимо добаить запись;
* **sign** - Проверочный код состоит из php функции md5($login."?".$login_user."?".$api_key);
	* **api_key** - Ваш уникальный код (Для уточнения вашего уникального кода свяжитесь с менеджером);
* **messages** - данные сообщения:
	* **type** - тип сообщения (1 - 5  по умолчанию 1);
	* **sell** – Стоимость сообщения, списывается с пакета;
	* **buy** – Стоимость купленных сообщений;
	* **originator** – Отправитель;
	* **email** – Email получателя;
	* **type_email** – Типа электронного письма (text/plain , text/html);
	* **theme_email** – Тема письма;
	* **text_email** – Текст письма;
	* **state** – Статус сообщения:
		* **not_deliver** - Сообщение не было доставлено;
		* **expired** - Просрочено;
		* **deliver** - Сообщение доставлено.;
		* **partly_deliver** - Сообщение было отправлено.;
	* **time** – Время отправки, если отсутствует - текущее время;
	* **id_message** – Идентификатор вашего сообщения;
	* **name_delivery** – Название рассылки;


В ответ может быть выдан один из следующих JSON-документов:
### В случае возникновения ошибки в отправляемом JSON-документе

JSON:
```json
{
	"status":"error",
	"error":"текст ошибки"
}
```
PHP (массив, полученный через php функцию json_decode):
```php
array ('status' => 'error', 'error' => 'текст ошибки')
```

### В случае получения правильного JSON-документа:

JSON:
```json
{
	"status":"success",
	"messages":[
		{"id":"100","state":"partly_deliver","id_message":"1001"},
		{"state":"error","id_message":"1002","error":"Сообщение об ошибке"}
	]
}
```
PHP (массив, полученный через php функцию json_decode):
```php
Array(
	[status] => success
	[messages] => Array(
		[0] => Array ( [id] => 100 [state] => partly_deliver [id_message] => 1001 ),
		[1] => Array ( [state] => error [id_message] => 1002 [error] => Сообщение об ошибке )
	)
)
```

Где:
* **status** - Статус запроса;
* **messages** - Сообщения:
	* **id** - Внутрений идентификатор сообщения;
	* **state** - Статус сообщения;
	* **id_message** - Идентификатор вашего сообщения;
	* **error** - Сообщение об ошибке;
