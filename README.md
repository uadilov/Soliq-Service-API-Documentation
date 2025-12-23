# API Документация Soliq Service

## 1. Аутентификация

(Basic Authentication)

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

## 8. Получение списка сохраненных фактур

### Endpoint

```http
GET /api/v3/lists
```

### Назначение

Получение списка сохраненных фактур с возможностью фильтрации и пагинации.

### Авторизация

- Basic Auth (обязательна)

### Query параметры

| Параметр | Тип | Описание | Пример |
|----------|-----|---------|---------|
| `path` | string | Тип папки (сохраненные документы) | `saved` |
| `docType` | string | Тип документа | `factura` |
| `limit` | integer | Количество результатов на странице | `20` |
| `offset` | integer | Смещение для пагинации (начиная с 0) | `0` |
| `fromDocDate` | string | Дата начала фильтра (YYYY-MM-DD) | `2025-11-01` |
| `folderId` | integer | ID папки (0 для всех папок) | `0` |

### Пример запроса

```http
GET /api/v3/lists?path=saved&offset=0&fromDocDate=2025-11-01&folderId=0&limit=20&docType=factura
```

### Ожидаемый ответ

```json
{
  "data": [
    {
      "id": "6949f02f526393dfb2d94a60",
      "docNo": "test2",
      "docDate": "2025-12-22",
      "sellerName": "\"GRAND PHARM TRADE\" MCHJ",
      "buyerName": "4-SON QURILISH TRESTI",
      "summa": 4032,
      "status": "draft"
    }
  ],
  "total": 100,
  "offset": 0,
  "limit": 20,
  "success": true
}
```

---

## 9. Получение списка отправленных фактур

### Endpoint

```http
GET /api/v3/lists
```

### Назначение

Получение списка отправленных (подписанных) фактур с возможностью фильтрации и пагинации.

### Авторизация

- Basic Auth (обязательна)

### Query параметры

| Параметр | Тип | Описание | Пример |
|----------|-----|---------|---------|
| `path` | string | Тип папки (отправленные документы) | `sent` |
| `docType` | string | Тип документа | `factura` |
| `limit` | integer | Количество результатов на странице | `20` |
| `offset` | integer | Смещение для пагинации (начиная с 0) | `0` |
| `fromDocDate` | string | Дата начала фильтра (YYYY-MM-DD) | `2025-01-01` |
| `folderId` | integer | ID папки (0 для всех папок) | `0` |

### Пример запроса

```http
GET /api/v3/lists?path=sent&offset=0&fromDocDate=2025-01-01&folderId=0&limit=20&docType=factura
```

### Ожидаемый ответ

```json
{
  "data": [
    {
      "id": "6949f02f526393dfb2d94a60",
      "docNo": "test2",
      "docDate": "2025-12-22",
      "sellerName": "\"GRAND PHARM TRADE\" MCHJ",
      "buyerName": "4-SON QURILISH TRESTI",
      "summa": 4032,
      "status": "sent",
      "signedDate": "2025-12-22T10:30:00Z"
    }
  ],
  "total": 150,
  "offset": 0,
  "limit": 20,
  "success": true
}
```
