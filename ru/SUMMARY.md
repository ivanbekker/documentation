# Summary

* [Введение](README.md)
* [Глоссарий](int_glossary.md)
* [Процессы и состояния](interface/process_and_state/processes.md)
   * [Диаграмма состояний](interface/process_and_state/state_diagramm.md)
   * [Процесс](interface/process_and_state/create_process.md)
   * [Отладка процесса (Debug)](interface/process_and_state/test.md)
   * [Правила остановки обработки заявок](interface/process_and_state/stop_task_rule.md)
   * [Экспорт / Импорт объектов](interface/process_and_state/eksport_import.md)
* [Узлы и логика](interface/nodes/README.md)
   * [Логика Delay](interface/nodes/delay.md)
   * [Логика Condition](interface/nodes/if.md)
   * [Логика API Call](interface/nodes/api/README.md)
       * [COREZOID](interface/nodes/api/corezoid.md)
       * [GET](interface/nodes/api/get.md)
       * [POST](interface/nodes/api/post.md)
       * [PUT / DELETE / HEAD / PATCH](interface/nodes/api/put__delete__head.md)
       * [Коды ошибок](interface/nodes/api/errors.md)
   * [Логика Waiting for Callback](interface/nodes/callback.md)
   * [Логика Code](interface/nodes/code.md)
       * [Библиотеки JavaScript](interface/nodes/function_code.md)
   * [Логика Copy task](interface/nodes/copy.md)
   * [Логика Modify Task](interface/nodes/logika_modify_task.md)
   * [Логика Sum](interface/nodes/sum.md)
   * [Логика Set Parameter](interface/nodes/set_param.md)
   * [Логика Set State](interface/nodes/setstate.md)
   * [Вызов универсального процесса](interface/nodes/rpc/README.md)
       * [Логика Call Process](interface/nodes/rpc/logic_rpc.md)
       * [Логика Reply to Process](interface/nodes/rpc/logic_rpc_reply.md)
   * [Работа с очередью заявок](interface/nodes/queue/README.md)
       * [Логика Queue](interface/nodes/queue/queue.md)
       * [Логика Get from Queue](interface/nodes/queue/get_task.md)
   * [Логика End](interface/nodes/logika_end.md)
   * [Настройки Additionally](interface/nodes/timer.md)
* [Заявки](interface/tasks/README.md)
   * [Параметры заявки](interface/tasks/task_parameters.md)
   * [Добавление заявок в процесс](interface/tasks/adding_tasks.md)
       * [Загрузка данных на Direct url процесса](interface/tasks/direct_url.md)
       * [Ручное добавление заявок в процесс](interface/tasks/new_task.md)
       * [Импорт заявок из файла](interface/tasks/import_from_csv.md)
   * [Архив заявок](interface/tasks/task_archive.md)
   * [Экспорт заявок](interface/tasks/export_to_csv.md)
   * [Функции](interface/functions/README.md)
       * [Чтение параметров узла](interface/functions/getParamFromCount.md)
       * [Значения параметров заявки](interface/functions/getParamFromApp.md)
       * [Время и дата](interface/functions/unixtime.md)
       * [Математические](interface/functions/math.md)
* [Дашборды](interface/dashboard.md)
* [Управление доступами](interface/users_groups.md)
* [Corezoid API](api/api.md)
   * [API v1](api/README.md)
      * [Описание протокола](api/spec.md)
      * [Загрузка и обновление данных](api/upload_modify.md)
      * [Диаграммы состояний/процессов](api/diagram.md)
      * [Узлы диаграммы](api/node.md)
      * [Логика процесса](api/logic.md)
      * [Получение статистики](api/statistic.md)
      * [Копирование папки в компанию](api/kopirovanie_papki_v_kompaniyu.md)
   * [API v2](api/README_v2.md)
* [Примеры](plugins/README.md)
   * [Brightpearl & Shopify](plugins/brightpearl_&_shopify/README.md)
   * [VISA API](plugins/visa/README.md)
       * [Получение доступа к API](plugins/visa/access.md)
       * [Курсы валют](plugins/visa/foreign_exchange_rates.md)
       * [P2P платежи](plugins/visa/p2p.md)
   * [Google](plugins/google/README.md)
       * [OAuth аутентификация](plugins/google/oauth.md)
       * [Gmail](plugins/google/gmail.md)
       * [AdWords](plugins/google/adwords.md)
       * [AppScripts](plugins/google/appscripts.md)
           * [Fusion Tables](plugins/google/fusion_tables.md)
           * [Sheets](plugins/google/sheets.md)
   * [eSputnik](plugins/esputnik/README.md)
       * [SMS](plugins/esputnik/sms.md)
       * [Email](plugins/esputnik/email.md)
   * [Viber](plugins/viber/viber.md)
       * [Public Account "Get Weather"](plugins/viber/public_account_weather.md)
       * [Public Account "Order Lunch"](plugins/viber/viber_order_lunch.md)
   * [Twilio](plugins/twilio/README.md)
       * [SMS](plugins/twilio/sms.md)
   * [Firebase](plugins/firebase/firebase.md)
   * [Mandrill](plugins/email/README.md)
       * [Mandrill ](plugins/email/mandrill.md)
           * [Поздравление с днем рождения](plugins/email/happy_birthday_greetings.md)  
       * [Mandrill + callback](plugins/email/mandrill_v2.md)
       * [Расчет эффективности email рассылки](plugins/email/mandrill_v3.md)
   * [Отправка SMS](plugins/sms/README.md)
       * [MessageBird](plugins/sms/messagebird.md)
   * [UniSender](plugins/email/unisender.md)
       * [Еmail](plugins/email/send_email_unisender.md)
       * [Еmail (webhook)](plugins/email/email_unisender_wh.md)
       * [SMS](plugins/email/send_sms_unisender.md)
   * [UniOne](plugins/email/unione.md)
   * [Slack](plugins/slack/weatherbot.md)
       * [weatherBot](plugins/slack/weatherbot.md) 
   * [Line](plugins/line/weatherbot.md)
       * [weatherBot](plugins/line/weatherbot.md) 
   * [Telegram](plugins/telegram/README.md)
       * [twitterBot](plugins/telegram/twitterbot.md)
       * [weatherBot](plugins/telegram/weather_bot.md)
       * [taxiBot](plugins/telegram/taxi_bot.md)
       * [bitcoinBot](plugins/telegram/bitcoinbot.md)
       * [bitcoinBot (multistage)](plugins/telegram/bitcoinbot_multistage.md)
       * [googleSpreadsheetBot](plugins/telegram/googleSpeadsheetBot.md)
       * [orderFoodBot](plugins/telegram/order_food_bot.md)
   * [Facebook & Messenger](plugins/facebook/README.md)
       * [weatherBot](plugins/facebook/weatherbot.md)
       * [Авторизация](plugins/facebook/autorization.md)
       * [Поиск публичных объектов ](plugins/facebook/poisk_publichnih_obektov.md)
       * [ticketsBot](plugins/facebook/ticketsbot.md)
   * [Skype](plugins/skype/README.md)
       * [weatherBot](plugins/skype/weatherbot.md)
       * [bitcoinBot](plugins/skype/bitcoinbot.md)
   * [Twitter](plugins/twitter/README.md)
       * [Отправка личного сообщения подписчику в twitter](plugins/twitter/sending_direct_message.md)
       * [Поиск и регулярный мониторинг твиттов](plugins/twitter/search_tweets.md)
   * [Weather](plugins/openweather/README.md)
       * [Температура по названию города](plugins/openweather/weather_city.md)
       * [Температура по координатам](plugins/openweather/weather_location.md)
   * [Salesforce](plugins/salesforce/README.md)
       * [OAuth 2.0](plugins/salesforce/oauth_20.md)
   * [Bitrix](plugins/bitrix/README.md)
       * [Авторизации приложения в запросах к REST API Bitrix](plugins/bitrix/autorizatoin.md)
       * [Создание нового лида](plugins/bitrix/new_lead.md)
   * [Оптимизация работы интернет-магазина](plugins/internet-shop/README.md)
       * [Автоматический прозвон входящих лидов](plugins/internet-shop/auto_call_lead.md)
   * [Работа с очередью заявок в обработке заказов такси](plugins/example_queue_get_task/README.md)
   * [LiqPay](plugins/liqpay/README.md)
       * [Checkout (Платежная страница)](plugins/liqpay/checkout.md)
   * [Antifraud](plugins/antifraud/README.md)
   * [Курсы валют](plugins/exchange_rates/README.md)
       * [Курсы валют ПриватБанка](plugins/exchange_rates/rates_pb.md)
       * [Архив курсов валют ПриватБанка и НБУ](plugins/exchange_rates/arhiv.md)
       * [Курсы валют и курсы драгоценных металлов НБУ и ЦБ РФ](plugins/exchange_rates/rates_nbu_tsb_rf.md)
       * [Курсы валют для юрлиц](plugins/exchange_rates/rates_pb_yur.md)
   * [АЗС "Авиас"](plugins/azs_avias/README.md)
       * [Получение курсов топлива](plugins/azs_avias/rates_avias.md)
       * [Получение адресов заправок](plugins/azs_avias/address_avias.md)
* [Bot platform](plugins/bot_platform/README.md)
* [История изменений](releases.md)
