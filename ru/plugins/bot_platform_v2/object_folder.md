# Объекты папки Bot Platform 2.0

  - [Процессы Viber/Telegram/Facebook Receiver](#процессы-viber/telegram/facebook-receiver)

  - [Процесс Main](#процесс-main)

  - [Event](#event)

  - [Процесс Router](#процесс-router)

  - [Процесс Send Message](#процесс-send-message)

  - [Папка Config](#папка-config)

  - [Tokens](#tokens)

  - [Command](#command)

  - [Attachments](#attachments)

  - [Пример](#пример)


## Процессы Viber/Telegram/Facebook Receiver

С этих процессов начинается работа бота. [Webhook](https://en.wikipedia.org/wiki/Webhook) каждого бота подключается к одноименному процессу **Receiver** на этапе создания **Bot Platform** (Telegram и Viber - автоматически, Facebook Messenger - ручная настройка).

  

Логика процесса **Receiver**:
-  принимает события (отправка сообщений, файлов, клики по кнопкам, переходы по ссылкам и др.) от пользователей ботов;
-  приводит полученные данные к единому формату:
	- `channel` - канал, из которого пришло сообщение. Доступные варианты:  telegram, facebook, viber.
	- `chat_id` - идентификатор чата, пользователя и т.п.
	- `event` - тип события
	- `message` - объект с деталями сообщения
-  передает данные в процесс Main.
    

>**Обратите внимание!** Если Вы хотите к уже существующей **Bot Platform** подвязать другого бота, необходимо вручную подключить webhook к процессу **Receiver**. Для этого откройте информационную панель процесса (**View details**) и перейдите во вкладку **Webhook**.

  
Нажмите кнопку **Connect to messenger**, выберите мессенджер и укажите новый токен для подключения webhook к процессу. Также необходимо обязательно актуализировать токены в [диаграмме Tokens](https://docs.google.com/document/d/1TE6x4j1UTEo1sb7hyflKHL_Zz5OqAxVt1igSguvkBBw/edit#heading=h.bflip5i11roz). REF заявки должен называться “token”.

![img](../img/bot_platform_v2/connect_to_messenger.png)

## Процесс Main

Процесс **Main** получает на вход стандартизированный набор данных из мессенджеров и выполняет первичную бизнес-логику бота перед началом маршрутизации заявок по подпроцессам.

**Input JSON Example**

    {
	    "channel": "viber",
	    "chat_id": "12312321",
	    "event": "message",
	    "message": {
	    "type": "text",
	    "text": "Lorem ipsum..."
		}
    }

#### Описание параметров процесса Main
|Parameter|Type|Required|Description|
|---|---|---|---|
| channel  | string  | +  |  Название мессенджера, из которого обратился клиент |
|  chat_id |  string | +  | Уникальный идентификатор пользователя в мессенджере  |
|  event | string  |  + | Тип события. Возможный значения: `start, context, message, command`  |
| message  |  object | +  |  Объект сообщения |
| first_name  | string  | -  | Имя пользователя, которое передается из мессенджера.  |
|last_name  | string  |  - |  Фамилия пользователя, которое передается из мессенджера. |
| gender  | string  | -  | Пол, который пользователь указал в мессенджере при регистрации  |
|  user_picture |  string | -  |Ссылка на аватар пользователя|
|  country |  string | -  | Страна, в которой находится пользователь. Определяется автоматически мессенджером. |
| language  |  string |  - |  Язык устройства пользователя |
| timezone  |  string |  - | Временная зона  |
| context  |  string | -  | Данные, которые передаются с событием “context”  |

  

### Event
| Event   | Description                                                                                                                                                                                                                                |
|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| start   | Событие инициируется при первом подключении пользователя в бота. Для Viber событие передается при каждом открытии диалога.                                                                                                                 |
| context | Событие инициируется при наличии контекста во входящем сообщении от мессенджера. Контекст - дополнительные параметры, которые могут добавляться к ссылке для перехода в бота. Например: запуск какой-то команды, реферальная ссылка и т.п. |
| message | Инициируется при отправке сообщения пользователем                                                                                                                                                                                          |
| command | Инициируется при отправке пользователем текстовое сообщение типа “/…”                                                                                                                                                                      |


  

Пример конструкции для передачи данных через “context”.  
**Viber** - `viber://pa?chatURI={{public_account_name}}&context={{params}}`  
**Facebook Messenger** - `http://m.me/{{bot_name}}?ref={{params}}`  
**Telegram** - `https://telegram.me/{{bot_name}}?start={{params}}`

## Процесс Router

Процесс, который реализует главную логику обработки событий **Bot platform**. Здесь реализована обработка всех типов event .

![img](../img/bot_platform_v2/router.png)


## Процесс Send Message

Процесс, который отвечает за отправку сообщений пользователям. Здесь происходит получение шаблонов текстов и приложений (кнопки, “карусель” и др.), локализация и динамическая подстановка значений в шаблоны.

![img](../img/bot_platform_v2/send_message.png)



#### Описание параметров процесса Send Message
| Parameter     | Type    | Required                                 | Description                                                                                                                                                                                                                                                                          |
|---------------|---------|------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| channel       | string  | +                                        | Название мессенджера, из которого обратился клиент                                                                                                                                                                                                                                   |
| chat_id       | string  | +                                        | Уникальный идентификатор пользователя в мессенджере                                                                                                                                                                                                                                  |
| text_id       | string  | +                                        | Название объекта с текстом, который будет отправлен пользователю. Тексты хранятся в State Diagram “[Localization](https://docs.google.com/document/d/1TE6x4j1UTEo1sb7hyflKHL_Zz5OqAxVt1igSguvkBBw/edit#heading=h.f7ftn7uxz4e3)”.                                                                                                                                                                    |
| attachment_id | string  | +                                        | Название объекта, который описывает приложение к текстовому сообщению (кнопки, карусели, клавиатуры). Приложения хранятся в State Diagram “[Commands](https://docs.google.com/document/d/1TE6x4j1UTEo1sb7hyflKHL_Zz5OqAxVt1igSguvkBBw/edit#heading=h.g4e3z8r7ftjq)”.                                                                                                                                |
| items         | array   | + (если используется Dynamic attachment) | Массив объектов, который используется для динамической подстановки параметров в переменные для отображения пользователю в виде карусели или клавиатуры.                                                                                                                              |
| currentPage   | integer | + (если используется Dynamic attachment) | Каждый мессенджер имеет ограничение по кол-ву отображаемых элементов в клавиатуре и карусели.Если кол-во элементов в массиве “items” больше макс. допустимого кол-ва отображаемых элементов, массив разбивается на страницы (пагинация). По умолчанию, стартовый номер страницы = 1. |

  

**Input JSON Example**

    {
    	"channel": "viber",
    	"chat_id": "12345...",
    	"text_id": "main_text",
    	"attachment_id": "main_keyboard",
    }

  

**Input JSON Example (Dynamic attachment)**

    {
    	"channel": "viber",
    	"chat_id": "12345...",
    	"text_id": "exchange_rates",
    	"attachment_id": "carousel_pattern",
    	"items": [
    	{
    	"name": "AED",
    	"value": 4.283634
    	},
    	{
    	"name": "AFN",
    	"value": 85.711652
    	},
    	{
    	"name": "ALL",
    	"value": 126.118624
    	},  
    	...
    	"currentPage": 1
    }

## Папка Config

В Bot Platform 2.0 предусмотрены диаграммы состояний отвечающие за хранение данных:

-   **Tokens** - токены ботов для мессенджеров (Viber, Telegram, FB Messenger).
-   **Commands** - соответствие (связка) названий команд и ID процессов.
-   **Localization** - тексты локализации контента.
-   **Attachments** - шаблоны приложений к сообщениям (кнопки, клавиатуры, “карусели”, списки), описанные в формате JSON.
-   **User Profile** - данные пользователей бота.  
      
    
Процессы **Bot Platform** взаимодействуют с диаграммами состояния с помощью [динамической конструкции](https://doc.corezoid.com/ru/interface/functions/getParamFromApp.html) для получения данных.

### Tokens
Эта диаграмма состояний отвечает за хранение токенов от ботов. Токены нужны для вызова API мессенджеров для отправки сообщений пользователям.

  

**JSON Example**  
  
**REF: token**

    {
    	"telegram": "...",
    	"viber": "...",
    	"facebook": "...",
    }

  

### Command

Для того, чтобы клик на кнопку в боте запускал процесс в Corezoid, необходимо создать связь между названием команды, которая инициируется при нажатии кнопки, и ID процесса, который должен быть запущен в Corezoid.

  

**Router** по названию команды [вычитывает](https://doc.corezoid.com/ru/interface/functions/getParamFromApp.html) значение параметра process_id заявки из диаграммы **Commands** и запускает процесс.

  

>**Обратите внимание!** REF заявки должен называться так же, как и ваша команда, с учетом введенного регистра. Например: /changeLanguage.

  
  

**JSON Example**

  
**REF: /changeLanguage**

    {
    	"process_id": "12345"
    }

  

### Attachments

Мессенджеры поддерживают отправку не только текстовых сообщений, но и различных приложений к сообщениям: кнопки, клавиатуры, "карусели", списки и др.

  

Все объекты, описывающие приложения к текстовым сообщениям, содержатся в диаграмме состояний **Attachments**. При добавлении приложения в диаграмму, мы рекомендуем указывать название [референса заявки](https://doc.corezoid.com/ru/interface/tasks/) в соответствии с названием целевого действия, для которого будет использоваться данное приложение. Например, “mainMenu” - для отображения главного меню, “exit” - для кнопки выхода из текущего бизнес-процесса и т.д.  
  

Это значение в дальнейшем указывается в параметре `attachment_id` при отправке сообщения пользователю.


Процесс **Send message** с помощью [динамической конструкции](https://doc.corezoid.com/ru/interface/functions/getParamFromApp.html):

    {{conv[{{attachment_proc_id}}].ref[{{attachment_id}}]}}
    
получает объект с описанием приложений для всех мессенджеров. Далее в параметр `attachment` сохраняется то приложение, которое соответствует `channel` и к нему применяются:
-   **локализация**: все значения `{{t'<key>}}` в параметрах приложения заменяются на тексты из [Localization](https://docs.google.com/document/d/1TE6x4j1UTEo1sb7hyflKHL_Zz5OqAxVt1igSguvkBBw/edit#heading=h.f7ftn7uxz4e3);
-   **динамическая подстановка значений**: замена переменных `{{param}}`, которые используются в описании приложения на значения параметров, которые поступают в процесс **Sender Message** вместе с заявкой.
    
Сформированное сообщение бот отправляет пользователю.

  
  

#### Пример

По умолчанию диаграмма состояний **Attachments** уже  содержит примеры готовых приложений.
  

**mainKeyboard - кнопки главного меню**

Facebook Messenger:

![img](../img/bot_platform_v2/main_keyboard_fb.png)


Viber:

![img](../img/bot_platform_v2/main_keyboard_viber.png)


Telegram:

![img](../img/bot_platform_v2/main_keyboard_telegram.png)


Детальнее с типами сообщений можно ознакомиться непосредственно в документации API мессенджера:

-   [Viber](https://viber.github.io/docs/api/rest-bot-api/#message-types)
-   [Facebook Messenger](https://developers.facebook.com/docs/messenger-platform/reference/send-api/)
-   [Telegram](https://core.telegram.org/bots/api#message)
    

  

**JSON Example**

**REF: exit**

    {
	    "telegram": {
		    "type": "inline_keyboard",
		    "buttons": [
			    [
				    {
				    "text": "🚪 {{t'exit}}",
				    "callback_data": "/exit"
					 }
				    ]
				   ]
			    },
			    "viber": {
				    "type": "keyboard",
				    "buttons": [
					{
					    "Columns": 6,
					    "Rows": 1,
						"BgColor": "#F3F3F3",
					    "Text": "🚪 {{t'exit}}",
					    "ActionType": "reply",
					    "ActionBody": "/exit",
					    "TextVAlign": "middle",
					    "TextHAlign": "center",
					    "TextSize": "regular",
					    "Silent": true
							    }
						    ]
					    },
		    "facebook": {
			    "type": "quick_replies",
			    "buttons": [
			    {
				    "content_type": "text",
				    "title": "🚪 {{t'exit}}",
				    "payload": "/exit"
			    }
		    ]
	    }
    }

  
  
  
#### Как работает Dynamic attachment?

  

При разработке бота часто возникает необходимость отображать данные однородной структуры. Это может быть каталог продукции, корзина с выбранными товарами, перечень действующих акций и т.д.

  

Процесс **Send message** поддерживает формирование приложений по шаблону для переменного количества элементов.

  

##### Добавление шаблона

Рассмотрим на примере существующего шаблона в **Attachment** для отображения курса валют:

  

**JSON Example (Dynamic attachment)**

**REF: carousel_pattern**

    {
    	"attachment": {
    		"facebook": {
    			"type": "carousel",
    			"items": [
    				{
    					"title": "{{value}} {{name}}",
    					"subtitle": "1 USD"
    				}
    			]
    		},
    		"viber": {
    			"type": "carousel",
    			"carouselRows": "1",
    			"carouselColumns": "6",
    			"items": [
    			{
    				"Columns": 6,
    				"Rows": 1,
    				"ActionType": "reply",
    				"ActionBody": "none",
    				"Text": "1 USD = {{value}} {{name}}",
    				"TextSize": "small",
    				"TextVAlign": "middle",
    				"TextHAlign": "middle",
    				"Silent": true,
    				"BgColor": "#FFFFFF"
    			}
    		]
    	}
    }
    }

  

>**Обратите внимание!** Для Telegram шаблон не описан, т.к. Telegram не поддерживает подобный тип сообщений.

  

##### Массив элементов items

  

Для формирования “карусели” необходимо сформировать массив элементов, из которого значения будут подставлены в шаблон:

    "items": [
    		{
    			"title": "4.183966 AED",
    			"subtitle": "1 USD"
    		},
    			{
    			"title": "85.890287 AFN",
    			"subtitle": "1 USD"
    			}
    ]

 
##### Преобразование JSON

При вызове процесса **Send message** необходимо обязательно передать параметры:

-   **attachment_id** = "carousel_pattern"
-   **items** = "items"
-   **currentPage** = 1
-   **disableExitButton** = false|true (флаг отображение кнопки “Выход”)

Все остальные действия с объектом выполняет узел с логикой **Code** "createDynamicAttachment".

![img](../img/bot_platform_v2/disableExitButton.png)

  

Процесс **Send Message** автоматически разбивает весь массив элементов на страницы и добавляет кнопки навигации (пагинация). При разбивке на страницы учитываются ограничения мессенджера согласно документации. При значении параметра **disableExitButton=false** добавляется кнопка “Выход”.


**Преобразованный JSON :**  

    "message":{
    	"quick_replies":[
    				{
    		"content_type":"text",
    		"title":"🚪 Exit",
    		"payload":"/exit"
    				}
    		],
    		"attachment":{
    		"type":"template",
    		"payload":{
    		"template_type":"generic",
    		"elements":[
    			{
    			"title":"4.183966 AED",
    			"subtitle":"1 USD"
    			},
    		{
    			"title":"85.890287 AFN",
    			"subtitle":"1 USD"
    		}
    	]
    }
    }
    }

  
  

**Пример отображения**
Facebook Messenger:  

![img](../img/bot_platform_v2/rates_usd_fb.png)

  
Viber:

![img](../img/bot_platform_v2/rates_viber.png)


### Localization

Диаграмма состояний **Localization** хранит все тексты сообщений и тексты, используемые в приложениях (кнопках, каруселях и др.), в одной заявке:

**REF: localization**

    {
    	"/exit": {
    		"en": "Exit",
    		"ru": "Выход",
    		"ua": "Вихід"
    	},
    		"mainMenu": {
    		"en": "Main Menu",
    		"ru": "Главное меню",
    		"ua": "Головне меню"
    	},
    		"no": {
    		"en": "No",
    		"ru": "Нет",
    		"ua": "Ні"
    	},
    		"yes": {
    		"en": "Yes",
    		"ru": "Да",
    		"ua": "Так"
    	},
    ...
    }

  

Такой подход позволяет:

1.  централизовать управление всеми текстами бота;
2.  отправлять сообщения, указав лишь **key** необходимого текста в объекте **localization**
3.  реализовать локализацию интерфейса бота. По умолчанию тексты указаны на языках **en**, **ru**, **ua**, но добавить можно любой язык мира.


Для отправки текстового сообщения необходимо передать в значение параметра `text_id` название ключа (key) из заявки **localization**.

![img](../img/bot_platform_v2/send_text_message.png)

  

**Пример**. Для отправки главного меню в боте необходимо в процесс **Send message** передать параметры:

    {
    "attachment_id":"mainKeyboard",
    "channel":"viber",
    "chat_id":"...",
    "text_id":"mainMenu"
    }

Для использования локализации в приложениях (attachments) параметры (текст кнопок, заголовки и др.) задаются в формате `{{t'<key>}}`, где `key` - ключ необходимого текста в объекте **localization**.  
  

**Пример**. Для локализации кнопки “Выход”:

    {  
    "content_type": "text",  
    "title": "🚪 {{t'/exit}}",  
    "payload": "/exit"  
    }

  

Процесс **Send message** отправляет сообщение на том языке, который хранится в диаграмме состояний **User Profile** для пользователя. По умолчанию - язык **en**, эта настройка изменяется на уровне узла  Set Parameter в процессе **Send message**.

### User Profile

Диаграмма состояний, в которой хранятся данные пользователей бота. Создание пользователя происходит при первом обращении к боту и при дальнейшей активности актуализируются.

Создание/редактирование заявки с данными пользователя происходит из процесса **Main** с указанием референса строго заданного формата: 
**`{{channel}}_{{chat_id}}`**, где  
  

`channel` - название мессенджера, из которого обратился клиент;
`chat_id` - уникальный идентификатор пользователя в мессенджере.  

**Пример**: viber_2yOPxC85DSpJCJHpYzjqTw=


Т.к. параметры `channel` и `chat_id` используются и являются обязательными во всех процессах **Bot platform**, это дает возможность получать и редактировать данные пользователя на любом этапе.

  