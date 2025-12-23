# API Документация Soliq Service

## 1. Аутентификация

Все запросы к API требуют базовую аутентификацию (Basic Authentication).

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

## 11. Статусы документов

| Статус | Описание |
|--------|----------|
| `accept` | Принят |
| `reject` | Отклонен |
| `cancel` | Отменен |
| `receive` | Получен |
| `revoked_by_gnk` | Аннулирован ГНК |
| `agent_receive` | Получен агентом |
| `agent_reject` | Отклонен агентом |
| `agent_accept` | Принят агентом |
| `pending` | Ожидает обработки |
| `template` | Шаблон |
| `sample` | Образец |
| `sent` | Отправлен |
| `not_real` | Не реальный |
| `not_found` | Не найден |
| `fail` | Ошибка |

---

## 12. Типичные ошибки и решения

| Ошибка | Решение |
|--------|----------|
| Контрагент не подтвердил документ | Убедитесь, что контрагент принял и подтвердил исходный документ |
| Некорректный ID лота | Проверьте правильность указанного ID лота |
| Статус юр.лица: ликвидировано | Контрагент больше не существует в реестре |
| ID прежнего счета-фактуры некорректный | Укажите правильный ID исходного документа для исправления |

```json
{
  "status": "string",
  "description": "string",
  "data": {
    "documents": [
      {
        "docId": "string",
        "docDate": "string (YYYY-MM-DD)",
        "docNo": "string",
        "docType": "string",
        "docStatus": "string",
        "contractDocNo": "string",
        "contractDocDate": "string (YYYY-MM-DD)",
        "totalDeliverySum": "number",
        "totalVatSum": "number",
        "totalDeliverySumWithVat": "number",
        "ownerTin": "string",
        "ownerName": "string",
        "ownerBranchCode": "string",
        "partnerTin": "string",
        "partnerName": "string",
        "partnerBranchCode": "string",
        "agentTin": "string",
        "agentName": "string",
        "hasVat": "boolean",
        "createdAt": "string (YYYY-MM-DD HH:MM:SS)",
        "updatedAt": "string (YYYY-MM-DD HH:MM:SS)",
        "commission": "boolean",
        "unilateral": "boolean",
        "marked": "boolean",
        "hasbenefit": "boolean",
        "hasLot": "boolean",
        "isRead": "integer (0/1)",
        "isReadAgent": "integer (0/1)",
        "ownerBranchName": "string",
        "partnerBranchName": "string",
        "note": "string",
        "organizationId": "string",
        "folderId": "integer",
        "facturaType": "string",
        "reqId": "string",
        "deletedAt": "integer"
      }
    ],
    "count": "integer",
    "totalDeliverySum": "number",
    "totalVatSum": "number",
    "totalDeliverySumWithVat": "number",
    "hasMarking": "boolean",
    "hasNote": "boolean"
  },
  "error": "string",
  "requestId": "string (UUID)"
}
```

---

## 11. Получение списка документов с ошибками

### Endpoint

```http
GET /api/v3/lists
```

### Назначение

Получение списка документов, которые не прошли проверку или отправку (со статусом "fail").

### Авторизация

- Basic Auth (обязательна)

### Query параметры

| Параметр | Тип | Описание | Пример |
|----------|-----|---------|---------|
| `path` | string | Тип папки (ошибки/неудачные документы) | `error` |
| `docType` | string | Тип документа | `factura` |
| `limit` | integer | Количество результатов на странице | `20` |
| `offset` | integer | Смещение для пагинации (начиная с 0) | `0` |
| `fromDocDate` | string | Дата начала фильтра (YYYY-MM-DD) | `2025-11-01` |
| `folderId` | integer | ID папки (0 для всех папок) | `0` |

### Пример запроса

```http
GET /api/v3/lists?path=error&offset=0&fromDocDate=2025-11-01&folderId=0&limit=20&docType=factura
```

### Schema ответа

```json
{
  "status": "string",
  "description": "string",
  "data": {
    "documents": [
      {
        "docId": "string",
        "docDate": "string (YYYY-MM-DD)",
        "docNo": "string",
        "docType": "string",
        "docStatus": "string (fail)",
        "contractDocNo": "string",
        "contractDocDate": "string (YYYY-MM-DD)",
        "totalDeliverySum": "number",
        "totalVatSum": "number",
        "totalDeliverySumWithVat": "number",
        "ownerTin": "string",
        "ownerName": "string",
        "ownerBranchCode": "string",
        "partnerTin": "string",
        "partnerName": "string",
        "partnerBranchCode": "string",
        "agentTin": "string",
        "agentName": "string",
        "hasVat": "boolean",
        "createdAt": "string (YYYY-MM-DD HH:MM:SS)",
        "updatedAt": "string (YYYY-MM-DD HH:MM:SS)",
        "commission": "boolean",
        "unilateral": "boolean",
        "marked": "boolean",
        "hasbenefit": "boolean",
        "hasLot": "boolean",
        "isRead": "integer (0/1)",
        "isReadAgent": "integer (0/1)",
        "ownerBranchName": "string",
        "partnerBranchName": "string",
        "note": "string (описание ошибки)",
        "organizationId": "string",
        "folderId": "integer",
        "facturaType": "string",
        "reqId": "string",
        "deletedAt": "integer"
      }
    ],
    "count": "integer",
    "totalDeliverySum": "number",
    "totalVatSum": "number",
    "totalDeliverySumWithVat": "number",
    "hasMarking": "boolean",
    "hasNote": "boolean"
  },
  "error": "string",
  "requestId": "string (UUID)"
}
```

### Ожидаемый ответ

```json
{
  "status": "OK",
  "description": "The request has succeeded",
  "data": {
    "documents": [
      {
        "docId": "69490c31147b8756313155bf",
        "docDate": "2025-12-22",
        "docNo": "5600504446",
        "docType": "factura",
        "docStatus": "fail",
        "contractDocNo": "46/B",
        "contractDocDate": "2022-01-05",
        "totalDeliverySum": -4170539,
        "totalVatSum": -500464.68,
        "totalDeliverySumWithVat": -4671003.68,
        "ownerTin": "303303592",
        "ownerName": "\"GRAND  PHARM  TRADE\" MAS'ULIYATI CHEKLANGAN JAMIYAT",
        "partnerTin": "308456691",
        "partnerName": "AVITSENNA FARM SINTES MCHJ",
        "hasVat": true,
        "createdAt": "2025-12-22 14:15:34",
        "updatedAt": "2025-12-23 06:34:06",
        "marked": true,
        "facturaType": "1",
        "note": "Eski hisobvaraq-faktura kontragent tomonidan tasdiqlanmagan. Eski hisobvaraq-faktura holat kodi: 20"
      },
      {
        "docId": "69458bb2f9b1aa8789aeaee2",
        "docDate": "2025-12-19",
        "docNo": "5600502480",
        "docType": "factura",
        "docStatus": "fail",
        "contractDocNo": "179/B",
        "contractDocDate": "2022-01-06",
        "totalDeliverySum": -759892,
        "totalVatSum": -91187.04,
        "totalDeliverySumWithVat": -851079.04,
        "ownerTin": "303303592",
        "ownerName": "\"GRAND  PHARM  TRADE\" MAS'ULIYATI CHEKLANGAN JAMIYAT",
        "partnerTin": "302147961",
        "partnerName": "Azim Farm Med MCHJ",
        "hasVat": true,
        "createdAt": "2025-12-19 22:30:40",
        "updatedAt": "2025-12-19 22:42:37",
        "marked": false,
        "facturaType": "1",
        "note": "Прежний счёт-фактура не подтверждён второй стороной. Код статуса прежнего ЭСФ: 20"
      },
      {
        "docId": "693ff7030e27bc442a476e62",
        "docDate": "2025-12-15",
        "docNo": "5600492712",
        "docType": "factura",
        "docStatus": "fail",
        "contractDocNo": "ОПТ/28",
        "contractDocDate": "2025-01-09",
        "totalDeliverySum": -2740375,
        "totalVatSum": -328845,
        "totalDeliverySumWithVat": -3069220,
        "ownerTin": "303303592",
        "ownerName": "\"GRAND  PHARM  TRADE\" MAS'ULIYATI CHEKLANGAN JAMIYAT",
        "partnerTin": "305335876",
        "partnerName": "DRUG-PROMOTION MCHJ",
        "hasVat": true,
        "createdAt": "2025-12-15 16:54:44",
        "updatedAt": "2025-12-19 12:14:26",
        "marked": true,
        "facturaType": "1",
        "note": "Общая сумма НДС всех товаров основного и дополнительного ЭСФ не должна быть меньше нуля!"
      }
    ],
    "count": 0,
    "totalDeliverySum": 0,
    "totalVatSum": 0,
    "totalDeliverySumWithVat": 0,
    "hasMarking": true,
    "hasNote": true
  },
  "error": "",
  "requestId": "019b497c-1fbb-78d3-9321-daeaf24021ac"
}
