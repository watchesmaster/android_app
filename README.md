App-Server API created for one of the biggest catalogues of Swiss watches http://www.watches-master.com
You may download full application using link below:
https://play.google.com/store/apps/details?id=com.infute.watchesmaster

API based on requests sending by application and server answers on appropriate request action.
Each request sends parameters through POST request to pre-defined URL.

Access to database
When application starts first time it sends device id and special identifier to the server to obtain unique identifier (token). It is obligatory parameter for further actions and requests.
When server receives device id it checks whether this device was assign with token. If it already exists in database then it is send to the device, if not – a new one is creating.
Application decodes answer in JSON format with token and amount of items to update. Afterwards this answer is saving to the SharedPreferences for further manipulations.

Database update
On each run, application synchronizes with server to receive up-to-date database. 
To avoid problems that may happen on devices with low performance method of inserting data optimized for multiple inserts. In compare with standard methods that use ContentValues for insert, multiple insert with SQLStatements increased performance and reduced launching time by several times. 
Considering device may lose internet connection while updating database, position of last successfully inserted item is stored after each input. On next run or request, application this position.
Application decodes answer in JSON format and inserts to database. 
When database was received in full amount, time of last successful update is stored and 


Пункт 2
Получение актуальной версии базы данных
Приложение отправляет запрос на сервер с токеном, датой последнего обновления, в формате Unix timestamp (по умолчанию 0 – для получения полной базы). Так как, база данных состоит из большого количества записей и при передаче всего объема, некоторые модели телефонов могут не справится с подобной задачей. Для этих целей было введено ограничение на кол-во получаемых записей (500).
Ответом от сервера, после проверки токена, является база данных в JSON формате.
По окончанию парсинга и записи в локальную базу данных при помощи SharedPreferences сохраняется дата последнего обновления, где БД была получена в полном объеме.

Пункт 3
Добавление товара в избранное
В базе данных ведется история добавления товаров в избранное.
При добавлении пользователем товара в Избранное, приложение направляет запрос серверу на внесение данного товара в базу данных в таблицу с Избранным.
В базе данных осуществляется проверка наличия id устройства и id выбранной модели часов. В случае отсутствия проводится запись. Сервер направляет устройству ответ.
Если от сервера не приходит положительный ответ (подтверждение записи, наличие существующей), то запрос повторно дублируется при следующем запуске приложения.

Пункт 4
Отправка заявки
Для отправки заявки была сформирована форма, после заполнения которой отправляется запрос серверу на внесение данной заявки в базу данных. Так же, вместе с заявкой сохраняются контактные данные покупателя, id устройства и id часов.
При успешном внесении заявки в базу данных, сервер направляет положительный ответ приложению. В ином случае, при следующем запуске приложения происходит проверка на наличие неотправленных заявок.
