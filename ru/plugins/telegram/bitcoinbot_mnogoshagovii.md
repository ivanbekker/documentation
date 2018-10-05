# BitcoinBot (multistage)

Клонируйте [папку "BitcoinBot (multistage)"](https://admin.corezoid.com/folder/conv/59748)

![](../img/multibot_clone.png)


Подключите Main (главный) процесс к Telegram, указав ключ Вашего Бота

![](../img/multibot_key.png)

Для получения ключа Бота нужно отправить команду `/newbot` в чат с BotFather. Далее указать имя и имя пользователя Бота. Вы получите:

![](../img/botweather_keybot.png)


##Как работает многошаговый BitcoinBot

![](../img/multi_schema.png)


[**Main процесс**](https://admin.corezoid.com/editor/59750/97490)

Получает все сообщения, поступающие в чат Бота и первым шагом пытается обновить заявку в процессе BitcoinBot.

Обновить заявку в процессе BitcoinBot = продолжить чат с пользователем в рамках уникального id чата.

![](../img/multibot_modify.png)

Это становится возможным багодаря созданию новой заявки в процессе BitcoinBot, когда пользователь отправляет команду `/curs`. Данная заявка создается с референсом равным id чата (`{{message.chat.id}}`).

Если при обновлении заявки по id чата таковая не была найдена, значит у нас еще нет открытого диалога в данном чате - нет заявки в процессе BitcoinBot. Тогда проверяем какая команда получена от пользователя.

В случае получния команды `/start`, отправляем сообщение с информацией о Боте. В случае, если получена `/curs` - создаем заявку в процессе BitcoinBot (как было описано выше).

![](../img/multibot_comand.png)

Если же получено что-то отличное от `/start` или `/curs`, определяем команду и оповещаем об этом пользователя соответствующим сообщением.

![](../img/multibot_undef.png)


[**Процесс "BitcoinBot"**](https://admin.corezoid.com/editor/59750/97491)

Сюда поступают заявки из Main процесса, если пользователь отправил команду `/curs` и первым шагом отправляется сообщение с предложением выбрать валюту.

>Напомним, референсом заявок в этом процессе является уникальный id чата (`message.chat.id`).

Это позволяет получать обновления (новые команды или сообщения от пользователя в чат Бота) из Main процесса, когда заявка находится в узле с [Логикой CALLBACK](https://doc.corezoid.com/ru/interface/nodes/callback.html).

Итак, после отправки сообщения с предложением выбрать валюту заявка переходит в ожидание этого выбора.

![](../img/multibot_1.png)

Если чрез 2 минуты выбор не будет сделан (заявка не обновится из Main процесса), то отправим сообщение об истечении времени ожидания.

Если же выбор валюты сделан, предлагаем пользователю выбрать какой курс показать - покупки или продажи и опять ждем в узле с CALLBACK.

![](../img/multibot_2.png)

Точно так же, если нет соответвующего выбора, отправим сообщение об истечении времени ожидания.

Если выбор сделан, получим курсы и отправим в сообщении.

![](../img/multibot_3.png)

[**Процесс "Send message"**](https://admin.corezoid.com/editor/59750/97489)

Отправляет сообщения в Telegram.

Оба процесса (Main и BitcoinBot) обращаются к нему через [Логику RPC](https://doc.corezoid.com/ru/interface/nodes/rpc/logic_rpc.html).


##Тестирование и запуск

Просто добавьте своего Бота в Telegram и начните чат.

Перейдите в режим `View` или `Debug`,

![](../img/botweather_view.png)

чтобы увидеть поток заявок, их прохождение и распределение по узлам процесса.

![](../img/multibot_view.png)