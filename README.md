# Требования к приложениям от аутсорса:
1. Приложение должно иметь возможность быть развернутым в docker через Dockerfile
2. Dockerfile должен поставляться в коде
3. Должны быть указаны зависимости на внешние системы (база данных, хранилище, телеграмм и тп), инструкция по настройке этих систем (если требуется)
4. Должны быть инструкции по настройке самого приложения (переменные окружения, строки подключения, конфиг файлы и тп)
5. Описать бэкграундные задачи, задачи по расписанию (если таковые имеются)/
6. В приложении не должно быть хардкода на окружение, все должно быть настраевываемым
7. Приложение должно уметь работать в нескольких экземплярах (для фейловера)
8. Перечислить функционал, который не может работать в параллель (если такой имеется)
9. Приложение должно уметь самостоятельно осуществлять миграции базы данных (желательно при старте)
10. Должен быть реализован метод проверки /healthcheck через API, возвращает код 200 на через метод GET (который говорит, что приложение ОК)
    ```bash
    curl http://localhost/healthcheck
    > GET /healthcheck HTTP/1.1
    < HTTP/1.1 200 OK
    ```
11. Логи должны быть в формате JSON для возможности импорта Elasticsearch. Пример приведен ниже
Пример лога в формате JSON. Внутри поля **fields** все поля должны быть либо строкой либо числом для каждой записи.
    ```json
    {
        "@timestamp": "2021-09-14T07:26:45.3535355+00:00",
        "level": "Information",
        "messageTemplate": "Выполнен метод {Action}",
        "message": "Выполнен метод \"Vesta.Transactions.Domain.Controllers.RegisterOfPaymentOrdersController.GetState (Vesta.Transactions.Domain)\"",
        "fields": {
            "Action": "Vesta.Transactions.Domain.Controllers.RegisterOfPaymentOrdersController.GetState (Vesta.Transactions.Domain)",
            "CorrelationId_Guid": "f4d1794f-ad1f-472a-a31d-4dd302fffc40",
            "UserId_Guid": "281aa8cb-2dc7-430a-9364-6aa4318fba84",
            "RequestId": "1516f9c1355e737e72befbc8cd9d9a47",
            "StatusCode_Int32": 200,
            "Result": "{\"Value\":\"GetRegisterOfPaymentOrdersStateResponse { Balances: [AccountBalance { AccountNumber: \\\"40702810600001021267\\\", AbsAmount: 76906116.29, Amount: 43058776.00 }], RegisterStatus: ReadyToUploadNewRegister, RegisterStatusDescription: \\\"Все платежи обработаны. Теперь можно заново загрузить ошибочные\\\", Message: \\\"\\\", NotProcessedCount: 0, SuccessCount: 5759, ErrorsCount: 7, Errors: [PaymentOrderError { Number: \\\"1291426\\\", Message: \\\"Нет доступного счета (40820810200001570540) в АБС\\\" }, PaymentOrderError { Number: \\\"1291556\\\", Message: \\\"Нет доступного счета (40820810600001568767) в АБС\\\" }, PaymentOrderError { Number: \\\"1292014\\\", Message: \\\"Нет доступного счета (40817810000001924626) в АБС\\\" }, PaymentOrderError { Number: \\\"1295229\\\", Message: \\\"Нет доступного счета (40820810400001566406) в АБС\\\" }, PaymentOrderError { Number: \\\"1295436\\\", Message: \\\"Нет доступного счета (40820810100001459318) в АБС\\\" }, PaymentOrderError { Number: \\\"1296055\\\", Message: \\\"Нет доступного счета (40817810800001946082) в АБС\\\" }, PaymentOrderError { Number: \\\"1299362\\\", Message: \\\"Нет доступного счета (40817810400001959741) в АБС\\\" }] }\",\"IsSuccess\":\"True\",\"Error\":\"null\",\"HttpStatusCode\":\"200\"}",
            "Path": "/RegisterOfPaymentOrders/state",
            "Query": "",
            "Elapsed_TimeSpan": "00:00:00.0260514",
            "BaseService Version": "1.154\n",
            "ActionId": "39717906-e85d-4112-8df8-423067d90ce3",
            "ActionName": "Vesta.Transactions.Domain.Controllers.RegisterOfPaymentOrdersController.GetState (Vesta.Transactions.Domain)",
            "RequestPath": "/RegisterOfPaymentOrders/state",
            "SpanId": "|e9bd50fd-4543183c4a66d970.",
            "TraceId": "e9bd50fd-4543183c4a66d970",
            "ParentId": "",
            "ConnectionId": "0HMBN9FDP9GLU",
            "Plugin Version": "1.2487-release\n"
        }
    }
    ```
12. Логи должны быть доступны через команду docker logs
    ```bash
    docker logs app-container
    ```
13. По возможности представить ориентировочные значения по потребляемым ресурсам, активность использования
    ```yaml
      cpu: 500m
      memory: 512Mi
    ```
