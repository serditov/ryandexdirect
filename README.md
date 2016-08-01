# ryandexdirect - пакет для работы с API Яндекс.Директ на языке R.


## Краткое описание.

Пакет ryandexdirect помонает получить дата фрейм со списком клиентов агентств из аккаунта Яндекс.Директ, получить список и обшие параметры рекламных кампаний по каждому из проектов, а так же получить детальную статистику по кампаниям за каждый день.


## Установка пакета ryandexdirect.

Установка пакета осуществляется из репозитория GitHub, для этого сначала требуется установить и подключить пакет devtools.

`install.packages("devtools")`

`library(devtools)`

После чего можно устанавливать пакет ryandexdirect.

`install_github('selesnow/ryandexdirect')`


## Функции входящие в пакет ryandexdirect.

На данный момент в версию пакета 1.0.0 входит 4 функции:

###`yadirGetToken()`
Функция для получения токена для доступа к API Яндекс.Директ, полученый токен используется во всех остальных функциях.

###`yadirGetClientList(token = NULL)`
Данная функция возвращает дата фрейм со списком всех клиентов доступных в агентском аккаунте которому был выдан токен для доступа к API, используется только при работе с агентскими аккаунтами.

Структура возвращаемого дата фрейма.
<table>
    <tr>
        <td><center>Поле</center></td><td><center>Тип данных</center></td><td><center>Описание</center></td>
    </tr>
    <tr>
        <td>Login</td><td>chr</td><td>Логин пользователя на Яндексе</td>
    </tr>
    <tr>
        <td>FIO</td><td>chr</td><td>Фамилия Имя Отчество указанные в аккаунте клиентом</td>
    </tr>
    <tr>
        <td>StatusArch</td><td>Factor</td><td>Учетная запись пользователя помещена в архив — Yes/No</td>
    </tr>
    <tr>
        <td>DateCreate</td><td>POSIXct</td><td>Дата регистрации пользователя, YYYY-MM-DD.</td>
    </tr>
    <tr>
        <td>Role</td><td>Factor</td><td>Роль в Яндекс.Директе:
        Client — клиент рекламного агентства, или прямой рекламодатель, или представитель прямого рекламодателя;
        Agency — рекламное агентство или представитель агентства.</td>
    </tr>
    <tr>
        <td>Email</td><td>chr</td><td>Адрес электронной почты клиента</td>
    </tr>
    <tr>
        <td>Phone</td><td>chr</td><td>Контактный телефон в произвольном формате.</td>
    </tr>
</table>

###`yadirGetCampaignList(logins = NULL, token = NULL)`
Функция возвращает дата фрейм со списком рекламных кампаний и некоторых их параметров по логину.

###`yadirGetSummaryStat(campaignIDS = NULL, dateStart = Sys.Date() - 10, dateEnd = Sys.Date(), currency = "USD", token = NULL)`
Основная функция пакета, возвращает дата фрейм со статистикой в разрезе кампаний и дат.


## Пример работы с пакетом ryandexdirect.

###Подключаем пакет ryandexdirect
library(ryandexdirect)

###Получаем токен для доступа к API
myToken <- yadirGetToken()

После запуска функции автоматически будет открыт браузер, на странице с выданным вам токеном, скопируйте его и вставьте в консоль R.

<img src="http://picsee.net/upload/2016-07-29/5d6a84ad44f8.png" data-canonical-src="http://picsee.net/upload/2016-07-29/5d6a84ad44f8.png" style="max-width:100%;">


<img src="http://picsee.net/upload/2016-07-29/acfa15376aa6.png" data-canonical-src="http://picsee.net/upload/2016-07-29/acfa15376aa6.png" style="max-width:100%;">

После чего в рабочей области появится объект myToken, который вы будете использовать в остальных функциях.

Далее в случае если у вас агетский аккаунт получаем список всех клиентов:

`client <- yadirGetClientList(myToken)`

Следующий шаг получить список рекламных кампаний клиентов, для этого в функции `yadirGetCampaignList` необходимо задать вектор со списком логинов тех аккаунтов по которым вы хотите получить статистику.
