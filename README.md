```Галиев Булат 11-907```

пояснение, как запустить систему в своем облаке:
* Создать бакет ```itis-2022-2023-vvot44-photos``` в Yandex Object Storage
* Создать облачную функцию ```vvot44-face-detection``` в Yandex Cloud Functions из репозитория.
Добавить файл requirements.txt с ```boto3```, добавить в переменные окружения значения ```AWS_ACCESS_KEY_ID``` и ```AWS_SECRET_ACCESS_KEY``` из сервисного аккаунта с ролью admin.
* Создать триггер ```vvot44-photo-trigger``` с настройками ```vvot44-photo-trigger``` в репозитории.
---
* Создать очередь с именем ```vvot44-tasks``` в Yandex Message Queue. Добавить в переменные окружения облачной функции ```vvot44-face-detection``` значение ```QueueUrl``` из  ```vvot44-tasks```.
* Создать бакет с именем ```itis-2022-2023-vvot44-faces```
* Создать базу даннынных ```vvot44-db-photo-face``` в YDB и в ней создать таблицу с именем faces со структурой ```vvot44-db-photo-face``` в репозитории.
* Создать Api шлюз с именем ```itis-2022-2023-vvot44-api``` в API Gateway и скопировать содержимое yml файла из репозитория.
---
* Создать реестр командой в консоли:

```yc container registry create --name my-first-registry``` 

* Сконфигурировать докер командой:

```yc container registry configure-docker```

* Скачать файлы dockerfile и index.py из репозитория и поместить в одну папку.
Сгенерировать файл сервисного аккаунта командой: 

```yc iam key create --folder-id {folder-id} --service-account-name vvot44-admin --output /vvot44-admin.json```

* Создать образ коммандой 

```docker build . -t cr.yandex/crpeus0hurv22o1jdcbu/vvot44```

И загрузить

```docker push cr.yandex/crp1nqnppgsj1noretbl/vvot44:latest```

---
* Создать контейнер ```vvot44-face-cut``` по загруженному образу и с переменными окружения 
```AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, SA_KEY_FILE=/vvot44-admin.json```

* Создать тригер с именем ```vvot44-task-trigger``` с настройками ```vvot44-task-trigger``` в репозитории.
* Создать облачную функцию с именем ```vvot44-boot``` в Yandex Cloud Functions и скопировать содержимое файла из репозитория.
Добавить файл ```requirements.txt``` с ```boto3,ydb,packaging```, добавить в переменные окружения значения
  ```AWS_ACCESS_KEY_ID``` и ```AWS_SECRET_ACCESS_KEY``` взятые из сервисного аккаунта с ролью admin, ```YDB_DATABASE``` и ```YDB_ENDPOINT``` из параметров базы данных ```vvot44-db-photo-face```,
  ```API_GATEWAY``` служебный домен  из ```itis-2022-2023-vvot44-api```
* В Telegram получить token. Далее создать Webhook вставив значения в ссылку и перейти по ссылке в браузере.
ссылка: ```https://api.telegram.org/bot{токенБота}/setWebhook?url={СсылкаДляВызоваФункции"vvot44-boot"}```
