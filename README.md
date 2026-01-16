# API Документация Soliq Service

## 1. Аутентификация

(Basic Authentication).

| Параметр | Значение |
|----------|----------|
| Тип | Basic Auth |
| Username | grand_pharm |
| Password | 7C6U0WsXqIw7FO71BqS |


---

## 2. Общая схема работы с фактурами

### Вариант 1: Сохранение → Подписание

1. Создание и сохранение фактуры: `POST /api/factura-save`
2. Подписание фактуры через ЭЦП: `POST /api/factura-create`

### Вариант 2: Немедленное подписание (отправка в роуминг)

1. Формирование фактуры (JSON)
2. Подписание ЭЦП (SIGN)
3. Отправка через `POST /api/factura-create`

---

## 3. Получение идентификаторов (GUID)

### Endpoint

```http
GET /api/utils/guid
```

### Ответ

```json
{
  "data": "6949f02f526393dfb2d94a60",
  "success": true
}
```

### Использование

Полученное значение используется для:
- `FacturaId`
- `FacturaProductId`

---

## 4. Получение данных по ИНН (NP1)

### Endpoint

```http
GET /api/np1/bytin?tinOrPinfl=<ИНН>
```

### Пример ответа

```json
{
  "tin": "303303592",
  "name": "\"GRAND PHARM TRADE\" MAS'ULIYATI CHEKLANGAN JAMIYAT",
  "account": "20208000000458557001",
  "mfo": "01036",
  "oked": "46460",
  "status": {
    "vatPayer": true,
    "vatRegCode": "326090004631"
  },
  "taxpayer": {
    "code": 20,
    "nameRu": "Плательщик НДС+ (сертификат активный)"
  }
}
```

### Используемые поля

| Источник | Назначение |
|----------|-----------|
| `status.vatRegCode` | VatRegCode |
| `taxpayer.code` | VatRegStatus |
| Другие реквизиты | Банк, наименование, адрес |

---

## 5. Сохранение фактуры (factura-save)

### Endpoint

```http
POST /api/factura-save
```

### Назначение

Сохранение фактуры в системе без ЭЦП.

### Авторизация

- Basic Auth (обязательна)
- Заголовок `Authorization` обязателен

### Документация

- [Структура JSON – Счёт-фактура](https://docs.google.com/document/d/1exQg1vDhMH2WFsI_MadK9ZbtN7RyJ4XS/edit?pli=1&tab=t.0)

### Тело запроса

```json
{
  "Version": 1,
  "FacturaType": 0,
  "SingleSidedType": 0,
  "HasMarking": false,
  "FacturaId": "6949c98a526393dfb2d94541",
  "FacturaDoc": {
    "FacturaNo": "test2",
    "FacturaDate": "2025-12-22"
  },
  "OldFacturaDoc": {
    "OldFacturaId": "",
    "OldFacturaNo": "",
    "OldFacturaDate": ""
  },
  "ContractDoc": {
    "ContractNo": "test",
    "ContractDate": "2025-12-01"
  },
  "FacturaEmpowermentDoc": {
    "EmpowermentNo": "",
    "EmpowermentDateOfIssue": "",
    "AgentFacturaId": "",
    "AgentFio": "",
    "AgentPinfl": ""
  },
  "ItemReleasedDoc": {
    "ItemReleasedTin": null,
    "ItemReleasedFio": "",
    "ItemReleasedPinfl": ""
  },
  "SellerTin": "303303592",
  "BuyerTin": "200523221",
  "Seller": {
    "Name": "\"GRAND PHARM TRADE\" MCHJ",
    "Account": "20208000000458557001",
    "BankId": "01036",
    "Address": "город Ташкент Алмазарский район UZUMBOG' 10-TOR KO'CHASI, 28-A-UY",
    "Mobile": "",
    "WorkPhone": "",
    "Oked": "",
    "DistrictId": "",
    "Director": "RAXIMOV ALISHER ABDUXAKIM O'G'LI",
    "Accountant": "SHOMURZAYEV XUSNIDDIN G'ULOM O'G'LI",
    "VatRegCode": "326090004631",
    "VatRegStatus": 20,
    "BranchCode": "",
    "BranchName": ""
  },
  "Buyer": {
    "Name": "4-SON QURILISH TRESTI",
    "Account": "20210000100176992001",
    "BankId": "00425",
    "Address": "город Ташкент Мирзо-Улугбекский район Xodjayeva 2a",
    "Mobile": "",
    "WorkPhone": "",
    "Oked": "",
    "DistrictId": "",
    "Director": "RAXIMBEKOV RUSTAMBEK RAXMANBEKOVICH",
    "Accountant": "RAXIMBEKOV RUSTAMBEK RAXMANBEKOVICH",
    "VatRegCode": "326030178001",
    "VatRegStatus": 20,
    "BranchCode": "00200",
    "BranchName": "ЕКН2"
  },
  "ForeignCompany": {
    "CountryId": "",
    "Name": "",
    "Address": "",
    "Bank": "",
    "Account": ""
  },
  "ProductList": {
    "FacturaProductId": "6949c99d147b87563131a77e",
    "Tin": "303303592",
    "HasExcise": false,
    "HasVat": true,
    "HasCommittent": false,
    "HasLgota": false,
    "HasMedical": false,
    "HasCustom": false,
    "HideReportCommittent": false,
    "Products": [
      {
        "OrdNo": 1,
        "CommittentName": "",
        "CommittentTin": "",
        "CommittentVatRegCode": "",
        "CatalogCode": "00902001001031001",
        "CatalogName": "Зеленый чай развесной и упакованный всех видов (заварка) Akbar Gold китай листовой байховый 100 гр коробка",
        "BarCode": "",
        "Name": "12",
        "Serial": "",
        "MeasureId": "",
        "BaseSumma": 0,
        "ProfitRate": 0,
        "DispenseType": 0,
        "Count": 12,
        "Summa": 300,
        "TotalSum": 0,
        "DeliverySum": 3600,
        "ExciseRate": 0,
        "ExciseSum": 0,
        "VatRate": 12,
        "VatSum": 432,
        "DeliverySumWithVat": 4032,
        "WithoutVat": false,
        "PackageCode": "1392342",
        "PackageName": "шт. (коробка) 100 грамм",
        "CommittentVatRegStatus": null,
        "Origin": 2,
        "Marks": null,
        "ExchangeInfo": null,
        "LgotaId": null,
        "LgotaName": null,
        "LgotaVatSum": 0,
        "LgotaType": null,
        "WarehouseId": null,
        "WarehouseName": null
      }
    ]
  },
  "IncomeType": 0,
  "LotId": "",
  "ContractId": "",
  "WaybillLocalIds": [],
  "WaybillIds": [],
  "FacturaInvestmentObjectDoc": {
    "ObjectId": "",
    "ObjectName": ""
  },
  "Notes": "",
  "CreatedOperator": "",
  "HasRent": false,
  "FacturaRentDoc": null,
  "MySoliqContractObjectDoc": null
}
```

---

## 6. Подписание фактуры (ЭЦП)

### 6.1 Формирование подписи PKCS#7

JSON фактуры подписывается с использованием ключа ЭЦП в формате PKCS#7. JSON должен быть в минифицированном формате.

### 6.2 Проставление timestamp

#### Endpoint

```http
POST /frontend/timestamp/pkcs7
```

#### Результат

Подписанный PKCS7 с timestamp.

---

## 7. Создание и отправка фактуры (factura-create)

### Endpoint

```http
POST /api/factura-create
```

### Назначение

- Подписание фактуры
- Отправка фактуры в Soliq Service или в роуминг
- Юридическое оформление документа

### Авторизация

- Basic Auth (обязательна)

### Тело запроса

```json
{
  "sign": "<PKCS7_WITH_TIMESTAMP>"
}
```

### Параметры

| Параметр | Тип | Описание |
|----------|-----|---------|
| `sign` | string | Подписанный PKCS7 с timestamp |

---

## 8. Получение фактуры по ID



### Endpoint

```http
GET /api/factura-get/{id}
```

### Назначение

Получение полной информации о фактуре по её уникальному идентификатору.

### Авторизация

- Basic Auth (обязательна)

### Параметры URL

| Параметр | Тип | Описание |
|----------|-----|---------|
| `id` | string | Уникальный идентификатор фактуры (FacturaId) |

### Пример запроса

```http
GET /api/factura-get/6537ce86d694742c34402e2f
```

### Schema ответа

```json
{
  "status": "string",
  "description": "string",
  "data": {
    "ActTransfer": "string",
    "Buyer": {
      "Host": "string",
      "Name": "string",
      "No": "integer",
      "Time": "string",
      "Tin": "string"
    },
    "ChargeCalculation": "string",
    "CreatedAt": "string (ISO 8601)",
    "ErrMsg": "string",
    "Factura": {
      "Buyer": {
        "Account": "string",
        "Accountant": "string",
        "Address": "string",
        "BankId": "string",
        "BranchCode": "string",
        "BranchName": "string",
        "Director": "string",
        "DistrictId": "string",
        "Mobile": "string",
        "Name": "string",
        "Oked": "string",
        "VatRegCode": "string",
        "VatRegStatus": "integer",
        "WorkPhone": "string"
      },
      "BuyerTin": "string",
      "ContractDoc": {
        "ContractDate": "string (YYYY-MM-DD)",
        "ContractNo": "string"
      },
      "ContractId": "string",
      "FacturaDoc": {
        "FacturaDate": "string (YYYY-MM-DD)",
        "FacturaNo": "string"
      },
      "FacturaEmpowermentDoc": {
        "AgentFacturaId": "string",
        "AgentFio": "string",
        "AgentPinfl": "string",
        "EmpowermentDateOfIssue": "string",
        "EmpowermentNo": "string"
      },
      "FacturaId": "string",
      "FacturaInvestmentObjectDoc": {
        "ObjectId": "string",
        "ObjectName": "string"
      },
      "FacturaType": "integer",
      "ForeignCompany": {
        "Account": "string",
        "Address": "string",
        "Bank": "string",
        "CountryId": "string",
        "Name": "string"
      },
      "HasMarking": "boolean",
      "IncomeType": "integer",
      "ItemReleasedDoc": {
        "ItemReleasedFio": "string",
        "ItemReleasedPinfl": "string",
        "ItemReleasedTin": "string"
      },
      "LotId": "string",
      "OldFacturaDoc": {
        "OldFacturaDate": "string",
        "OldFacturaId": "string",
        "OldFacturaNo": "string"
      },
      "ProductList": {
        "FacturaProductId": "string",
        "HasCommittent": "boolean",
        "HasExcise": "boolean",
        "HasLgota": "boolean",
        "HasMedical": "boolean",
        "HasVat": "boolean",
        "HideReportCommittent": "boolean",
        "Products": [
          {
            "BarCode": "string",
            "BaseSumma": "number",
            "CatalogCode": "string",
            "CatalogName": "string",
            "CommittentName": "string",
            "CommittentTin": "string",
            "CommittentVatRegCode": "string",
            "CommittentVatRegStatus": "integer",
            "Count": "number",
            "DeliverySum": "number",
            "DeliverySumWithVat": "number",
            "DispenseType": "integer",
            "ExchangeInfo": {
              "PlanPositionId": "integer",
              "ProductCode": "string",
              "ProductProperties": "string"
            },
            "ExciseRate": "number",
            "ExciseSum": "number",
            "LgotaId": "integer",
            "LgotaName": "string",
            "LgotaType": "integer",
            "LgotaVatSum": "number",
            "Marks": {
              "IdentTransUpak": ["string"],
              "Kiz": ["string"],
              "NomUpak": ["string"],
              "ProductType": "integer"
            },
            "MeasureId": "string",
            "Name": "string",
            "OrdNo": "integer",
            "Origin": "integer",
            "PackageCode": "string",
            "PackageName": "string",
            "ProfitRate": "number",
            "Serial": "string",
            "Summa": "number",
            "VatRate": "number",
            "VatSum": "number",
            "WarehouseId": "integer",
            "WarehouseName": "string",
            "WithoutVat": "boolean"
          }
        ],
        "Tin": "string"
      },
      "Seller": {
        "Account": "string",
        "Accountant": "string",
        "Address": "string",
        "BankId": "string",
        "BranchCode": "string",
        "BranchName": "string",
        "Director": "string",
        "DistrictId": "string",
        "Mobile": "string",
        "Name": "string",
        "Oked": "string",
        "VatRegCode": "string",
        "VatRegStatus": "integer",
        "WorkPhone": "string"
      },
      "SellerTin": "string",
      "SingleSidedType": "integer",
      "Version": "integer",
      "WaybillId": "string",
      "WaybillIds": ["string"]
    },
    "GNK": {
      "Host": "string",
      "Name": "string",
      "No": "integer",
      "Time": "string",
      "Tin": "string"
    },
    "HasAccessBranch": "integer",
    "HasApp": "boolean",
    "Internal": "string",
    "Notes": "string",
    "OrganizationId": "string",
    "PageStatus": "string",
    "Seller": {
      "Host": "string",
      "Name": "string",
      "No": "integer",
      "Time": "string",
      "Tin": "string"
    },
    "Sign": "string",
    "Status": "string",
    "UpdatedAt": "string (ISO 8601)"
  },
  "error": "string",
  "requestId": "string (UUID)"
}
```

---
### Статусы фактуры
| Статус | Описание |
|----------|---------|
| `template` | Сохраненные |
| `receive` | Ожидает подписи партнера |
| `cancel` | Отменен |
| `accept` | Подписан |
| `reject` | Отказано |
| `fail` | Неудачно |


 
## 9. Получение списка документов (универсальный)

### Endpoint

```http
GET /api/get-all-docs
```

### Назначение

Получение списка документов с расширенными возможностями фильтрации и пагинации.

### Авторизация

- Basic Auth (обязательна)

### Query параметры

| Параметр | Тип | Описание | Пример |
|----------|-----|---------|---------|
| `docType` | string | Тип документа | `factura` |
| `docStatus` | string | Статус документа (через запятую) | `accept,reject,cancel` |
| `limit` | integer | Количество результатов на странице | `10` |
| `offset` | integer | Смещение для пагинации | `0` |
| `fromDocDate` | string | Дата начала по дате документа | `2023-03-01` |
| `toDocDate` | string | Дата окончания по дате документа | `2023-03-31` |
| `fromDate` | string | Дата начала по дате создания | `2023-04-05` |
| `toDate` | string | Дата окончания по дате создания | `2023-04-30` |
| `docNo` | string | Номер документа | `test` |
| `docId` | string | ID документа | |
| `contractDocNo` | string | Номер контракта | `23` |
| `contractDocDate` | string | Дата контракта | |
| `ownerTin` | string | ИНН продавца | `200523221` |
| `ownerName` | string | Наименование продавца | |
| `ownerPinfl` | string | ПИНФЛ продавца | |
| `ownerBranchCode` | string | Код филиала продавца | |
| `partnerTin` | string | ИНН покупателя | `203335595` |
| `partnerName` | string | Наименование покупателя | |
| `partnerPinfl` | string | ПИНФЛ покупателя | |
| `partnerBranchCode` | string | Код филиала покупателя | `00068` |
| `agentTin` | string | ИНН агента | |
| `agentName` | string | Наименование агента | |
| `agentPinfl` | string | ПИНФЛ агента | |
| `facturaType` | string | Тип фактуры | |
| `hasVat` | boolean | Содержит НДС | |
| `marked` | boolean | Маркированная | |
| `commission` | boolean | Комиссионная | |
| `unilateral` | boolean | Односторонняя | |
| `hasbenefit` | boolean | Со льготой | |
| `isSave` | string | Сохранена | |
| `totalSum` | string | Общая сумма | |
| `organization` | string | Организация | `organization` |

### Пример запроса

```http
GET /api/get-all-docs?docType=factura&limit=10&offset=0&fromDocDate=2023-03-01
```

### Schema ответа

```json
{
  "status": "string",
  "description": "string",
  "data": {
    "count": "integer",
    "documents": [
      {
        "agentName": "string",
        "agentTin": "string",
        "commission": "boolean",
        "contractDocDate": "string (YYYY-MM-DD)",
        "contractDocNo": "string",
        "createdAt": "string (YYYY-MM-DD HH:MM:SS)",
        "docDate": "string (YYYY-MM-DD)",
        "docId": "string",
        "docNo": "string",
        "docStatus": "string",
        "docType": "string",
        "facturaType": "string",
        "folderId": "string",
        "hasVat": "boolean",
        "hasbenefit": "boolean",
        "isRead": "integer (0/1)",
        "isReadAgent": "integer (0/1)",
        "isSave": "string",
        "marked": "boolean",
        "note": "string",
        "organizationId": "string",
        "ownerBranchCode": "string",
        "ownerBranchName": "string",
        "ownerName": "string",
        "ownerTin": "string",
        "partnerBranchCode": "string",
        "partnerBranchName": "string",
        "partnerName": "string",
        "partnerTin": "string",
        "sentDate": "string (YYYY-MM-DD HH:MM:SS)",
        "totalDeliverySum": "number",
        "totalDeliverySumWithVat": "number",
        "unilateral": "boolean",
        "updatedAt": "string (YYYY-MM-DD HH:MM:SS)"
      }
    ]
  },
  "error": "string",
  "requestId": "string (UUID)"
}
```

---

## 10. Примеры использования фильтров

### Получение отправленных фактур

```http
GET /api/get-all-docs?docType=factura&docStatus=accept&fromDocDate=2025-01-01
```

### Получение документов с ошибками

```http
GET /api/get-all-docs?docType=factura&docStatus=fail&fromDocDate=2025-11-01
```

### Поиск по контрагенту

```http
GET /api/get-all-docs?docType=factura&partnerTin=203335595&limit=20
```

### Расширенная фильтрация

```http
GET /api/get-all-docs?docType=factura&hasVat=true&marked=true&fromDocDate=2025-01-01&toDocDate=2025-12-31
```

---

## 11. Отмена фактуры (Cancel Factura)

### Авторизация

**Basic Auth (обязательна)**

### 11.1 Формирование подписи PKCS#7

JSON фактуры подписывается с использованием ключа ЭЦП в формате PKCS#7. JSON должен быть в минифицированном формате.

### 11.2 Проставление timestamp

#### Endpoint

```http
POST /frontend/timestamp/pkcs7
```

#### Результат

Подписанный PKCS7 с timestamp.

### 11.3 Пример для получения подписи

```json
{
  "SellerTin": "303303592",
  "FacturaId": "695d2d271a38709aa4a69767"
}
```

### 11.4 POST запрос для отмены фактуры

#### Endpoint

```http
POST https://v3.soliqservis.uz:3443/api/factura/cancel?tin=303303592
```

#### Body

```json
{
  "facturaId": "695d2d271a38709aa4a69767",
  "sign": "<Sign>"
}
```

#### Параметры

| Параметр | Описание | Тип |
|----------|---------|-----|
| `tin` | ИНН продавца (query параметр) | string |
| `facturaId` | Идентификатор фактуры для отмены | string |
| `sign` | Подписанный PKCS#7 с ЭЦП | string |

#### Пример ответа

```json
{
    "status": "OK",
    "description": "The request has succeeded",
    "data": "OK",
    "error": "",
    "requestId": "019ba2d1-8125-71bc-8407-bf76ea7a9260"
}
```

## 12. Удалить сохранённые счета-фактуры
Почему нужно удалять сохранённый ЭСФ: если при подписании произошла ошибка, ЭСФ переходит в состояние «сохранённый», и его нельзя отменить через 11. Отмена фактуры (Cancel Factura) осуществляется по эндпоинту /api/factura/cancel.

### Авторизация

**Basic Auth (обязательна)**

#### Endpoint

```http
DELETE https://v3.soliqservis.uz:3443/api/lists-delete
```

#### Body

```json
{
  "docIds": [
    "<factura id>"
  ],
  "ownerTin": "303303592"
}
```

#### Пример ответа

```json
{
    "status": "OK",
    "description": "The request has succeeded",
    "data": "OK",
    "error": "",
    "requestId": "019bb0ca-009e-750c-b97b-0deb44a4469b"
}
```

---

# Общая схема работы с ТТН

## 1. Получение идентификаторов (GUID)

### Endpoint

```http
GET /api/utils/guid
```

### Ответ

```json
{
  "data": "6949f02f526393dfb2d94a60",
  "success": true
}
```

### Использование

Полученное значение используется для:
- `WaybillLocalId`

---

## 2. Получить Регионы ТТН

### Авторизация

- Basic Auth (обязательна)

### Endpoint

```http
GET /api/waybill-region-basic
```

### Ответ

```json
{
    "status": "OK",
    "description": "The request has succeeded",
    "data": [
        {
            "active": 1,
            "code": 1730,
            "districtCode": 0,
            "name": "Fergana region",
            "nameRu": "Ферганская область",
            "nameUzCyrl": "Фарғона вилояти",
            "nameUzLatn": "Farg'ona viloyati",
            "regionId": 30
        },
        {
            "active": 1,
            "code": 1735,
            "districtCode": 0,
            "name": "Republic of Karakalpakstan",
            "nameRu": "Республика Каракалпакстан",
            "nameUzCyrl": "Қорақалпоғистон Республикаси",
            "nameUzLatn": "Qoraqalpog'iston Respublikasi",
            "regionId": 35
        },
        {
            "active": 1,
            "code": 1726,
            "districtCode": 0,
            "name": "Tashkent city",
            "nameRu": "город Ташкент",
            "nameUzCyrl": "Тошкент шаҳри",
            "nameUzLatn": "Toshkent shahri",
            "regionId": 26
        },
        {
            "active": 1,
            "code": 1727,
            "districtCode": 0,
            "name": "Tashkent region",
            "nameRu": "Ташкентская область",
            "nameUzCyrl": "Тошкент вилояти",
            "nameUzLatn": "Toshkent viloyati",
            "regionId": 27
        },
        {
            "active": 1,
            "code": 1733,
            "districtCode": 0,
            "name": "Khorezm region",
            "nameRu": "Хорезмская область",
            "nameUzCyrl": "Хоразм вилояти",
            "nameUzLatn": "Xorazm viloyati",
            "regionId": 33
        },
        {
            "active": 1,
            "code": 1724,
            "districtCode": 0,
            "name": "Syrdarya region",
            "nameRu": "Сырдарьинская область",
            "nameUzCyrl": "Сирдарё вилояти",
            "nameUzLatn": "Sirdaryo viloyati",
            "regionId": 24
        },
        {
            "active": 1,
            "code": 1703,
            "districtCode": 0,
            "name": "Andijan region",
            "nameRu": "Андижанская область",
            "nameUzCyrl": "Андижон вилояти",
            "nameUzLatn": "Andijon viloyati",
            "regionId": 3
        },
        {
            "active": 1,
            "code": 1706,
            "districtCode": 0,
            "name": "Bukhara region",
            "nameRu": "Бухарская область",
            "nameUzCyrl": "Бухоро вилояти",
            "nameUzLatn": "Buxoro viloyati",
            "regionId": 6
        },
        {
            "active": 1,
            "code": 1710,
            "districtCode": 0,
            "name": "Kashkadarya region",
            "nameRu": "Кашкадарьинская область",
            "nameUzCyrl": "Қашқадарё вилояти",
            "nameUzLatn": "Qashqadaryo viloyati",
            "regionId": 10
        },
        {
            "active": 1,
            "code": 1712,
            "districtCode": 0,
            "name": "Navoi region",
            "nameRu": "Навоийская область",
            "nameUzCyrl": "Навоий вилояти",
            "nameUzLatn": "Navoiy viloyati",
            "regionId": 12
        },
        {
            "active": 1,
            "code": 1708,
            "districtCode": 0,
            "name": "Jizzakh region",
            "nameRu": "Джизакская область",
            "nameUzCyrl": "Жиззах вилояти",
            "nameUzLatn": "Jizzax viloyati",
            "regionId": 8
        },
        {
            "active": 1,
            "code": 1718,
            "districtCode": 0,
            "name": "Samarkand region",
            "nameRu": "Самаркандская область",
            "nameUzCyrl": "Самарқанд вилояти",
            "nameUzLatn": "Samarqand viloyati",
            "regionId": 18
        },
        {
            "active": 1,
            "code": 1722,
            "districtCode": 0,
            "name": "Surkhandarya region",
            "nameRu": "Сурхандарьинская область",
            "nameUzCyrl": "Сурхондарё вилояти",
            "nameUzLatn": "Surxondaryo viloyati",
            "regionId": 22
        },
        {
            "active": 1,
            "code": 1714,
            "districtCode": 0,
            "name": "Namangan region",
            "nameRu": "Наманганская область",
            "nameUzCyrl": "Наманган вилояти",
            "nameUzLatn": "Namangan viloyati",
            "regionId": 14
        },
        {
            "active": 1,
            "code": 1750,
            "districtCode": 1,
            "name": "Xududlararo inspeksiya",
            "nameRu": "МРИ",
            "nameUzCyrl": "Ҳудудлараро инспекция",
            "nameUzLatn": "Xududlararo inspeksiya",
            "regionId": 50
        }
    ],
    "error": "",
    "requestId": "019bc697-70a9-739e-92a5-42a1b62295ba"
}
```

### Использование

Полученное значение используется для:
- `RegionId`
-  `RegionName`

---

## 3. Получить Районы ТТН

### Авторизация

- Basic Auth (обязательна)

### Endpoint

```http
GET /api/waybill-district-basic?regionId=<regionId>
```

### Ответ

```json
{
    "status": "OK",
    "description": "The request has succeeded",
    "data": [
        {
            "active": 1,
            "districtCode": 10,
            "name": "Uchtepa district",
            "nameRu": "Учтепинский район",
            "nameUzCyrl": "Учтепа тумани",
            "nameUzLatn": "Uchtepa tumani",
            "regionId": 26,
            "soato": 1726262
        },
        {
            "active": 1,
            "districtCode": 11,
            "name": "Bektemir district",
            "nameRu": "Бектемирский район",
            "nameUzCyrl": "Бектемир тумани",
            "nameUzLatn": "Bektemir tumani",
            "regionId": 26,
            "soato": 1726264
        },
        {
            "active": 1,
            "districtCode": 3,
            "name": "Yunusabad district",
            "nameRu": "Юнусабадский район",
            "nameUzCyrl": "Юнусобод тумани",
            "nameUzLatn": "Yunusobod tumani",
            "regionId": 26,
            "soato": 1726266
        },
        {
            "active": 1,
            "districtCode": 2,
            "name": "Mirzo-Ulugbek district",
            "nameRu": "Мирзо-Улугбекский район",
            "nameUzCyrl": "Мирзо Улуғбек тумани",
            "nameUzLatn": "Mirzo Ulug'bek tumani",
            "regionId": 26,
            "soato": 1726269
        },
        {
            "active": 1,
            "districtCode": 1,
            "name": "Mirabad district",
            "nameRu": "Мирабадский район",
            "nameUzCyrl": "Миробод тумани",
            "nameUzLatn": "Mirobod tumani",
            "regionId": 26,
            "soato": 1726273
        },
        {
            "active": 1,
            "districtCode": 5,
            "name": "Shaykhantakhur district",
            "nameRu": "Шайхантахурский район",
            "nameUzCyrl": "Шайхонтохур тумани",
            "nameUzLatn": "Shayxontoxur tumani",
            "regionId": 26,
            "soato": 1726277
        },
        {
            "active": 1,
            "districtCode": 9,
            "name": "Almazar district",
            "nameRu": "Алмазарский район",
            "nameUzCyrl": "Олмазор тумани",
            "nameUzLatn": "Olmazor tumani",
            "regionId": 26,
            "soato": 1726280
        },
        {
            "active": 1,
            "districtCode": 7,
            "name": "Sergeli district",
            "nameRu": "Сергелийский район",
            "nameUzCyrl": "Сирғали тумани",
            "nameUzLatn": "Sirg'ali tumani",
            "regionId": 26,
            "soato": 1726283
        },
        {
            "active": 1,
            "districtCode": 4,
            "name": "Yakkasaray district",
            "nameRu": "Яккасарайский район",
            "nameUzCyrl": "Яккасарой тумани",
            "nameUzLatn": "Yakkasaroy tumani",
            "regionId": 26,
            "soato": 1726287
        },
        {
            "active": 1,
            "districtCode": 8,
            "name": "Yashnobodsky district",
            "nameRu": "Яшнободский район",
            "nameUzCyrl": "Яшнобод тумани",
            "nameUzLatn": "Yashnobod tumani",
            "regionId": 26,
            "soato": 1726290
        },
        {
            "active": 1,
            "districtCode": 12,
            "name": "Yangihayot tumani",
            "nameRu": "Янгихаётский район",
            "nameUzCyrl": "Янгихаёт тумани",
            "nameUzLatn": "Yangihayot tumani",
            "regionId": 26,
            "soato": 1726292
        },
        {
            "active": 1,
            "districtCode": 6,
            "name": "Chilanzar region",
            "nameRu": "Чиланзарский район",
            "nameUzCyrl": "Чилонзор тумани",
            "nameUzLatn": "Chilonzor tumani",
            "regionId": 26,
            "soato": 1726294
        }
    ],
    "error": "",
    "requestId": "019bc69d-7408-7ec0-9ec2-a749175b3663"
}
```

### Использование

Полученное значение используется для:
- `DistrictCode`
- `DistrictName`

---

## 4. Запрос в ГАИ для получения списка транспортных средств, зарегистрированных на указанный ИНН

### Авторизация

- Basic Auth (обязательна)

### Endpoint

```http
GET /api/waybill-transports-basic?tinOrPinfl=<INN>
```

### Ответ

```json
{
    "status": "OK",
    "description": "The request has succeeded",
    "data": [
        {
            "model": "BYD YUAN PLUS",
            "ownershipType": 1,
            "regNo": "01 815 OKA",
            "transportType": 2
        },
        {
            "model": "GAZ 330232 750",
            "ownershipType": 1,
            "regNo": "01 529 DKA",
            "transportType": 6
        },
        ...
    ],
    "error": "",
    "requestId": "019bc6a5-90ad-7510-a8aa-c5de3d3e44b4"
}
```

### Использование

Полученное значение используется для:
- `DistrictCode`
- `DistrictName`

---

## 5. Проверка корректности PINFL и получение полного ФИО
### Авторизация

- Basic Auth (обязательна)

### Endpoint

```http
GET /api/np1/bytin?tinOrPinfl=<PINFL>
```

### Ответ

```json
{
    "account": "<accountNUM>",
    "accountant": "",
    "address": "<ADDRESS>",
    "director": "",
    "directorPinfl": 0,
    "directorTin": "",
    "isBudget": 0,
    "isItd": false,
    "mfo": "01088",
    "na1Code": 0,
    "na1Name": "",
    "name": "ODILOV UMIDJON SULTONMUROD O‘G‘LI",
    "ns10Code": 26,
    "ns11Code": 10,
    "oked": "",
    "peasantFarm": false,
    "personalNum": "<pinfl>",
    "privateNotary": false,
    "selfEmployment": false,
    "shortName": null,
    "statusCode": 0,
    "statusName": "",
    "tin": "<tin>",
    "status": {
        "active": false,
        "isSafe": true,
        "suspended": false,
        "vatPayer": false,
        "vatRegCode": ""
    },
    "taxpayer": {
        "code": 60,
        "name": "Жисмоний шахс",
        "nameRu": "Физическое лицо",
        "nameUz": "Jismoniy shaxs"
    },
    "facturaId": "",
    "facturaProductId": "",
    "product": null,
    "has_branch": 0
}
```

### Использование

Полученное значение используется для:
- `Pinfl`
- `FullName`

---

