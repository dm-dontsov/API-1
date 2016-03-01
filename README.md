# API WAPCLICK.ONLINE

**версия 1.0**

## 1. Описание

WapClick – новая прогрессивная технологий мобильных подписок. Абоненту достаточно нажать одну кнопку для получения доступа к платному контенту и совершения подписки. Абонент получает качественный контент, списания с его счёта происходит ежедневно!


### 1.1 Особенности:

1. Абонент соглашается на подписку на странице оператора связи (landing page).
2. Оператор связи списывает средства со счёта абонента. Списание средств и предоставление доступа прекращается при выполнении одного из следующих условий – абонент отписался, закончился срок подписки, нет достаточного колличества средств на счету.
3. Тарификация подписавшегося абонента происходит ежедневно.


## 2. Подключение

Для подключения партнёр предоставляет:

1. Адрес уведомлений.
2. Адрес возврата на сайт (может быть не задан, если партнёр будет задавать адрес возврата для каждого абонента при инициации подписки).

Менеджер сообщает партнёру:

1. Идентификатор заведённой подписки (service_id).
2. Секретный ключ (secret_key).

### 2.1 Стандартная схема

Партнёр размещает на страницах сайта код загрузки скрипта

```html
<script type="text/javascript" src="//wapclick.mobi/script/[идентификатор подписки].js" async></script>
```

 Пример

```html
<script type="text/javascript" src="//wapclick.mobi/script/13511.js" async></script>
```

Партнёр размещает на страницах сайта элемент для перехода к активации подписки. Это может быть кнопка или любой другой элемент

```html
<div id="subscribe_btn"></div>
```

Так же необходимо внизу страницы разместить элемент для отображения правил подписки

```html
<div id="footer_frame"></div>
```

### 2.2 Упрощённая схема

Партнёр размещает ссылку на баннере или кнопке

```
http://wapclick.mobi/init/[идентификатор подписки].html
```

Пример

```html
<a href="http://wapclick.mobi/init/13511.html">Оформите подписку!</a>
```

Упрощённая схема может быть использована только после согласования с менеджером.

## 3. Дополнительные параметры подключения

Если требуется предоставлять абоненту несколько видов контента или требуется большая безопасность взаимодействия, то возможно задавать дополнительные параметры при использовании как стандартной, так и упрощённой схем

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| back_url | varchar(1000) | Адрес возврата абонента | https://site.com/content
| landing[operator] | varchar(1000) | Идентификатор лэндинга на стороне оператора | 111

Параметр p_data будет в дальнейшем передан партнёру в уведомлении об успешной тарификации.

back_url будет использован при возврате абонента вместо адреса, прописанного по умолчанию.

Параметр landing используется для отображения абоненту кастомизированных лэндингов при оформлении подписки на сайте оператора и представляет собой идентификатор лэндинга. Данных параметров может быть несколько, если требуется указать идентификатор лэндинга для нескольких операторов.

Данные параметры не являются обязательными.

Пример

```html
<script type="text/javascript" src="https://wapclick.mobi/script/13511.js?p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&back_url=https%3A%2F%2Fsite.com%2Fcontent&landing[mf]=111&landing[mts]=222" async></script>
```

## 4. Возврат абонента на сайт партнёра

После операции подписки абонент возвращается на back_url, переданный в дополнительных параметрах кода загрузки скрипта или на адрес возврата на сайт, который прописывается при заведении подписки. При возврате к адресу добавляются следующие параметры

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| code | int | Статус подписки | 0
| error | varchar(30) | Расшифровка статуса подписки | ok

Получение данных параметров (успешного статуса подписки) не является гарантией успешного статуса подписки на стороне оператора, т.к. может быть подделано абонентом. Рекомендуется опираться на данные параметры только в простейших сервисах или для предварительного уведомления абонента об успехе или неуспехе.

Для гарантии подписки абонента на стороне оператора требуется обрабатывать уведомление о тарификации и предоставлять абоненту контент после получения успешного уведомления.

Статусы подписок

| code | error | Описание
| --- | --- | ---
| 0 | ok | Подписка оформлена
| 1 | invalid params | Неверные параметры
| 2 | invalid auth | Неверный проверочный код
| 3 | invalid provider auth (please, contact us) | Изменились настройки подключения
| 4 | operator is not supported | Оператор не поддерживается
| 5 | service not found | Сервис не определен или нет такого сервиса
| 6 | service disabled | Сервис отключен
| 7 | already | Абонент уже был подписан или отписан
| 8 | billing failed: no money / fraud | У абонента нет денег / фрод
| 9 | refuse | Абонент отказался от подписки
| 10 | disabled by operator options for abonent / subscriptions banned | У абонента отключены опции подписок на стороне оператора
| 11 | operator subscription service internal error | Ошибка на стороне оператора связи
| 12 | pending | Ожидается действие от абонента
| 13 | service internal error | Ошибка на стороне wapclick.online
| 14 | invalid phone number / cannot find | Оператор на своей стороне не может определить номер телефона абонента
| 15 | activation not finished | Активация не окончена
| 16 | incorect activation code | Неверный код подтверждения от абонента
| 17 | attempt count exceeded | Превышено количество попыток получения временной ссылки от оператора
| 18 | expired | Превышено время ожидания действий от абонента
| 19 | pending (wait confirmaton) | Абонент не завершил действие на стороне оператора
| 20 | cannot find source | Источник не зарегистрирован
| 21 | cannot find oferta | Оферта не найдена
| 22 | device block | Устройство абонента не поддерживает подписки
| 23 | service blacklisted | Чёрный список

## 5. Уведомления о тарификациях

wapclick.online уведомляет партнера об успешных списаниях денежных средств со счета абонента.

Уведомление - GET запрос с параметрами

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| action | varchar(30) | Тип уведомления - тарификация | charge_report
| status | int | Статус тарификации. 0 - успешная, -1 - неуспешная | 0
| phone | bigint | Номер абонента | 79031234567
| op | varchar(30) | Оператор | beeline
| c_amount | numeric(18,2) | Сумма тарификации в валюте абонента | 100.01
| amount | numeric(18,2) | Сумма тарификации в валюте расчётов с партнёром | 100.01
| c_pay | numeric(18,2) | Сумма выплаты партнёру в валюте абонента | 50.01
| pay | numeric(18,2) | Сумма выплаты партнёру в валюте расчётов с партнёром | 50.01
| c_curr | character(3) | Буквенный код валюты абонента (ISO 4217) | RUB
| curr | character(3) | Буквенный код валюты расчётов с партнёром (ISO 4217) | RUB
| tid | varchar(100) | Уникальный идентификатор транзакции | 08057700-690d-11e5-b610-321018f8ac82
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| service_id | integer | Идентификатор подписки | 1234

Пример уведомления

```
https://site.com/subscriptions?action=charge_report&status=0&phone=79031234567&op=beeline&c_amount=100.01&amount=100.01&c_pay=50.01&pay=50.01&c_curr=RUB&curr=RUB&tid=08057700-690d-11e5-b610-321018f8ac82&p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&service_id=1234
````

## 6. Уведомления об отписках

wapclick.online уведомляет партнера об отписке абонента.

Уведомление - GET запрос с параметрами

| Параметр | Тип | Описание | Пример
| --- | --- | --- | ---
| action | varchar(30) | Тип уведомления - отписка | unsubscribe_report
| service_id | integer | Идентификатор подписки | 1234
| phone | bigint | Номер телефона абонента | 79031234567
| tid | varchar(100) | Идентификатор транзакции | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| p_data | varchar(100) | Идентификатор подписки в системе партнёра | 077dd9d0-690d-11e5-b533-0d1018f8ac82
| sign | char(64) | Подпись запроса sha256(action+service_id+phone+tid+p_data+secret_key). Если p_data не использовался, то он не участвует в формировании подписи | 68e656b251e67e8358bef8483ab0d51c6619f3e7a1a9f0e75838d41ff368f728

Пример уведомления

```
https://site.com/subscriptions?action=unsubscribe_report&service_id=1234&phone=79031234567&tid=077dd9d0-690d-11e5-b533-0d1018f8ac82&p_data=077dd9d0-690d-11e5-b533-0d1018f8ac82&sign=68e656b251e67e8358bef8483ab0d51c6619f3e7a1a9f0e75838d41ff368f728
```

## 7. Оценка конверсии

Система поддерживает подключение UTM-меток к элементам активации подписки. Для их использования нужно добавить метки в виде атрибута data. Метки будут доступны для выборки в личном кабинете.

Поддерживаемые метки:

* utm_source
* utm_medium
* utm_campaign
* utm_term
* utm_content

Пример

````html
<div id="subscribe_btn" data-utm_source="1" data-utm_medium="2" data-utm_campaign="3" data-utm_term="4" data-utm_content="5"></div>
````

## 8. Дополнительные возможности API

Обращение к методам API происходит по адресу:

````
https://api.wapclick.mobi/{version}/{method}?params
````
Актуальная версия API – v1.

### Пример обращения к API

```
curl https://api.wapclick.mobi/v1/operators.getList?format=json
```

### Авторизация

Некоторые методы API требуют авторизации, для доступа к ним используется basic-аутентификация.

Доступ к такиим методам можно получить с помощью логинов и паролей, в рамках механизма Basic-аутентификации протокола HTTP.

Для Basic-аутентификации каждый запрос приложения к API должен содержать заголовок Authorization следующего вида:

```
Authorization: Basic <токен доступа>
```

Токен доступа здесь — это строка вида <логин>:<пароль> в кодировке base64.

В качестве **логина** используется ID заведённой подписки (service\_id), в качестве **пароля** – cекретный ключ подписки (secret\_key).



### Получение списка операторов

**Описание:** получение списка операторов, поддерживаемых в системе.

```
GET /v1/operators.getList
```

**Параметры:**

Параметр | Тип    | Описание
---------|------- |----------
format   | string | Формат ответа, допустимые варианты: json (по умолчанию), text, xml.<br />*Необязательный параметр*.

### Получение списка IP-адресов операторов

**Описание:** получение списка IP-адресов абонентов операторов, поддерживаемых в системе.

```
GET /v1/operators.getIps
```

**Параметры:**

| Параметр | Тип    | Описание
| --- | --- | ---
| operators | string  | Один или несколько кодов операторов, перечисленных через запятую.<br />Пример: mts,beeline
| countries | string  | Один или несколько кодов стран, перечисленных через запятую.<br />Пример: ru,kz
| type | string | Формат результирующего списка.<br />Допустимые параметры:<br />* **cidr** – свёрнутый в подсети список IP-адресов (по умолчанию)<br />* **list** – развёрнутый список адресов<br />*Необязательный параметр*.
| format | string | Формат ответа, допустимые варианты: json (по умолчанию), text, xml.<br />*Необязательный параметр*.


### Получение статистики

**Описание:** получение списка транзакций за период.

```
GET /v1/subscriptions.getStats
```

**Метод требует авторизации**


**Параметры:**

Параметр | Тип    | Описание
---------|------- |----------
date_from	| string  | Начальная дата поиска транзакций.<br />Формат: YYYY-MM-DD (пример: 2016-01-01)
date_to	| string  | Конечная дата поиска транзакций.<br />Формат: YYYY-MM-DD (пример: 2017-12-31)
limit		| integer	| Количество возвращаемых записей. По умолчанию 100.<br />*Необязательный параметр*.
offset		| integer | сдвиг, необходимый для получения конкретной выборки результатов. По умолчанию 0.<br />*Необязательный параметр*.
format   	| string 	| Формат ответа, допустимые варианты: json (по умолчанию), text, xml.<br />*Необязательный параметр*.

## 9. Контакты

С вопросами обращайтесь по почте [support@wapclick.online](mailto:support@wapclick.online)

