# Task_API_hotel
Task_API_hotel
Работа с Rest сервисом через Postman
>Вся документация о сервисе: https://wiki.servio.support/index.php?title=Servio_POS_API



3.1. Аутентификация
Взаимодействие с API будет происходить путём обращения данных систем протокола JSON, кодирования передачи данных UTF8, методом POST.
Перед выполнением методы проверяются наличие зарегистрированного пользователя по значению параметра AccessToken, передаваемого в header-е.
Метод Authenticate
Аутентификация в системе и получение AccessToken
Пример:
>URL https://demo.servio.support/STEND_POS_WEB_NET/POSExternal/Authenticate

Body
```
{
    "CardCode": "777",
    "TermID": "ANDROID"
}
```
В ответе получаем информацию о регистрации в системе. Необходимо использовать параметр "Token" для передачи в header-e следующих запросов.
```
curl --location --request POST 'https://demo.servio.support/STEND_POS_WEB_NET/POSExternal/Authenticate' \
--header 'Content-Type: application/json' \
--data-raw '{
    "CardCode": "777",
    "TermID": "ANDROID"
}'
```
Эти параметры доступа по адресу https будут использоваться для последующих пунктов
3.2. Получение иерархии меню
Необходимо получить иерархию меню заведения
Описание по ссылке:
>https://wiki.servio.support/index.php?title=Get_TarifItems

3.3 Получение содержимого группы
Необходимо получить содержимое папки из ИД
Описание по ссылке:
>https://wiki.servio.support/index.php?title=Get_TarifItem

3.4 Пытаемся создать счет и наполнить его несколькими позициями из п.3.3

Описание здесь:
>https://wiki.servio.support/index.php?title=Create_BillListItemPack

Необходимо создать счет с BillType =1
Обязательные параметры: BillType, для секции Items: OperType, TarifItemID, BillItemID, Quantity

3.5. Попробуем изменить количество добавленных позиций уже созданного счета с помощью Set_BillListItemPack
Параметры аналогичны методу выше. Описание здесь:

>https://wiki.servio.support/index.php?title=Set_BillListItemPack

3.6 Получаем содержимое созданного счета, используя функцию Get_Bill
Описание здесь:

>https://wiki.servio.support/index.php?title=Get_Bill

Выполнение
3.1.Аутентификация

1.Создаем запрос POST-Authenticate
2.В URL подставляем 
>https://possvc3.servio.support/29135/PosExternal/Authenticate

3.В body подставляем значение body в формате JSON
```
 {
"CardCode": "777",
"TermID": "Android"
}
```
4.Кликаем кнопку __[Send]__

3.2.Получение иерархии меню

1.Создаем запрос POST-Get_TarifItems
2.В URL подставляем 
>https://possvc3.servio.support/29135/PosExternal/Get_TarifItems

3.В Header подставляем значение AccessToken,token можно просмотреть в запросе POST-Authenticate
4.В body подставляем значение body в формате JSON
```
 {
"CardCode": "777",
"TermID": "Android"
}
```
5.Кликаем кнопку __[Send]__

3.3.Получение содержимого группы.

1.Создаем запрос POST-Get_TarifItem
2.В URL подставляем
>https://possvc3.servio.support/29135/PosExternal/Get_TarifItem

3.В Header подставляем значение AccessToken,token можно просмотреть в запросе POST-Authenticate
4.В body подставляем значение body в формате JSON
Как пример:
```
{
"GroupMenuID": 22860,
"PriceListID":3109,
"PriceListCode2":"",
"HierarchyLevel": 1,
"WithProperties": true,
"ReceptInfoInText": true,
"DateMenu": "2025.09.24 18:22:53"
}
```
5.Кликаем кнопку __[Send]__

3.4 Пытаемся создать счет и наполнить его несколькими позициями из п.3.3

1.Создаем запрос POST-Create_BillListItemPack
2.В URL подставляем
>https://possvc3.servio.support/29135/PosExternal/Create_BillListItemPack

3.В Header подставляем значение AccessToken,token можно просмотреть в запросе POST-Authenticate
4.В body подставляем значение body в формате JSON
Как пример:
```
 {
    "SystemCode": "123",
    "BillType": 1,
    //  "FirstDate": null,
    //  "LastDate": null,
    //  "UserName": null,
    //  "CompanyName": "TestCompany",
    //   "Class": "3", //string[20] доп.поле идентификации устройства
    //  "Description": "",
    //   "Locked": false,
    //  "NumType": 0,
    //   "GuestCount": 0,
    //   "CostOfDelivery": 0,
    "Items": [
    {
            "OperType": 1,
            "TarifItemID": 22863, //ИД позиции из Get_TarifItem 22863
            "BillItemID": 1,
            "ParentID": null,
            "Quantity": 3,
            "Price": null,
            "ModifierID": null
        },
        {
            "OperType": 1,
            "TarifItemID": 22864, //ИД позиции из Get_TarifItem 22864
            "BillItemID": 1,
            "ParentID": null,
            "Quantity": 3,
            "Price": null,
            "ModifierID": null
        },
        {
            "OperType": 1,
            "TarifItemID": 22865, //ИД позиции из Get_TarifItem 22865
            "BillItemID": 2,
            "ParentID": null,
            "Quantity": 1,
            "Price": null,
            "ModifierID": null
        }
    ]
 }
```
3.5. Попробуем изменить количество добавленных позиций уже созданного счета с помощью Set_BillListItemPack
 
1.Создаем запрос POST-Set_BillListItemPack
2.В URL подставляем
>https://possvc3.servio.support/29135/PosExternal/Set_BillListItemPack
3.В Header подставляем значение AccessToken,token можно просмотреть в запросе POST-Authenticate
4.В body подставляем значение body в формате JSON
Как пример:
```
{
  "BillID": 23490,// берем из response Create_BillListItemPack
    "Discounts": [
        {
            "BillID": 17391,
            "CardID": 0,
            "Discount": 30.0,
            "DiscountGroup": 0,
            "DiscountID": 0,
            "DiscountType": 4,
            "id": 1,
            "IsLoyalty": 0,
            "PersonDiscountID": 0,
            "SystemCode": "4503"
        }
    ],
    "FromService": false,
    "GuestCount": 5,
    "Items": [],
    "NumType": 1,
    "SystemCode": "4503"
 }
 ```
 5.Кликаем кнопку __[Send]__

3.6 Получаем содержимое созданного счета, используя функцию Get_Bill
1.Создаем запрос POST-Get_Bill
2.В URL подставляем 
>https://possvc3.servio.support/29135/PosExternal/Get_Bill

3.В Header подставляем значение AccessToken,token можно просмотреть в запросе POST-Authenticate
4.В body подставляем значение body в формате JSON
Как пример:
```
 {
    "BillID": "{{billid}}",
    //"CodeGUID":"805CC7A4-48F5-445C-9290-BE71964F0E6B",
    "Locked_": false
 }
 ```
 5.Кликаем кнопку __[Send]__
