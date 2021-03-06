# Базовый функционал Bot Platform.

## Курс валют

Процесс **/exchangeRates (Dynamic Attachment)** в папке Sample Bots.
При нажатии в главном меню кнопки “Курс валют”, отображается карусель с текущим курсом Доллара США по отношению к другим валютам.

## Смена языка общения

Процесс **/changeLanguage** в папке Sample Bots.
Бот предлагает выбрать язык общения из 3 вариантов "en", "ru", "ua". Выбранное пользователем значение сохраняется в диаграмме состояний **User Profile** и применяется для **локализации** текстов и приложений в боте.

## Стандартный опрос

Процесс **/sampleSurvey** в папке Sample Bots/Sample Survey.
Бот предлагает оценить “Вам нравится этот бот?” с вариантами выбора "Да" и "Нет".
Результаты опроса копируются в диаграмму состояний **Results**, в которой узлы - это [метрики дашборда](https://doc.corezoid.com/ru/interface/dashboard.html#%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D1%87%D0%B0%D1%80%D1%82%D0%B0)  **Results Dashboard**. Диаграмма состояний и дашборд размещены в одной папке с процессом опроса.

## Подключение оператора

Подключение оператора и маршрутизация сообщений из мессенджеров в операторский чат Sender осуществляется с помощью Логики **Sender action - Category "Widgets" - Robot "Send Message For Bot Platform".**

Для получения ответов от операторов используется процесс **Sender Receiver** (находится в папке Messengers/Sender/). На этапе создания **Bot platform** этот процесс автоматически подвязывается к событию Sender "Получение сообщений от бот платформы".

По умолчанию в компании есть только один оператор - владелец компании. Как добавить операторов подробно описано в [документации](https://doc.sender.mobi/adm_panel_operators.html).

## Опрос NPS

Процесс **/nps**, который находится в папке (Messengers/Sender/Chat/NPS), используется для оценки диалога, который запускается автоматически после завершения чата с оператором. Бот задает пользователю вопрос “Диалог был завершен. Уточните, пожалуйста, Ваш вопрос был решен?” и выбранная оценка передается в [аналитику](https://doc.sender.mobi/adm_panel_analytics.html#%D0%B4%D0%B8%D0%B0%D0%BB%D0%BE%D0%B3%D0%B8) Sender. Так же, как и в [стандартном опросе](https://docs.google.com/document/d/1TE6x4j1UTEo1sb7hyflKHL_Zz5OqAxVt1igSguvkBBw/edit#heading=h.y1axjqi0sxgw) предусмотрена диаграмма состояний и дашборд для статистики.
