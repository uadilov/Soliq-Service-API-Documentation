# Soliq Service API — Руководство по интеграции

> Готовая для GitHub документация для разработчиков. Только главное, без лишнего.

## Аутентификация
- Схема: Basic Auth
- Данные (со стороны Soliq Service): `username=grand_pharm`, `password=7C6U0WsXqIw7FO71BqS`
- Заголовок: `Authorization: Basic <base64(username:password)>`

## Сценарии работы со счет-фактурами
- **Вариант 1: Сохранить → Подписать → Отправить**
	1) Сохранить черновик: `POST /api/factura-save`
	2) Подписать и отправить сохранённый черновик (PKCS#7): `POST /api/factura-create`
- **Вариант 2: Мгновенная отправка**
	- Сформировать JSON → подписать PKCS#7 → отправить через `POST /api/factura-create`

## 1) Получение GUID
- Endpoint: `GET /api/utils/guid`
- Пример:
```json
{ "data": "6949f02f526393dfb2d94a60", "success": true }
```
- Поле `data` используйте как `FacturaId` и `FacturaProductId`.

## 2) NP1-запрос (ИНН/ПИНФЛ)
- Endpoint: `GET /api/np1/bytin?tinOrPinfl=<TIN>`
- Карта важных полей:
	- `status.vatRegCode` → `VatRegCode`
	- `taxpayer.code` → `VatRegStatus`
	- Банковские реквизиты, наименование, адрес → в реквизиты продавца/покупателя

Пример (фрагмент):
```json
{
	"tin": "303303592",
	"name": "\"GRAND PHARM TRADE\" MAS'ULIYATI CHEKLANGAN JAMIYAT",
	"account": "20208000000458557001",
	"mfo": "01036",
	"oked": "46460",
	"status": { "vatPayer": true, "vatRegCode": "326090004631" },
	"taxpayer": { "code": 20, "nameRu": "Плательщик НДС+ (сертификат активный)" }
}
```

## 3) Сохранение счет-фактуры — `POST /api/factura-save`
- Назначение: сохранить счет-фактуру без ЭЦП.
- Авторизация: Basic Auth обязательна.
- Тело запроса: полная структура счет-фактуры. Пример:
```json
{
	"Version": 1,
	"FacturaType": 0,
	"SingleSidedType": 0,
	"HasMarking": false,
	"FacturaId": "6949c98a526393dfb2d94541",
	"FacturaDoc": { "FacturaNo": "test2", "FacturaDate": "2025-12-22" },
	"OldFacturaDoc": { "OldFacturaId": "", "OldFacturaNo": "", "OldFacturaDate": "" },
	"ContractDoc": { "ContractNo": "test", "ContractDate": "2025-12-01" },
	"FacturaEmpowermentDoc": { "EmpowermentNo": "", "EmpowermentDateOfIssue": "", "AgentFacturaId": "", "AgentFio": "", "AgentPinfl": "" },
	"ItemReleasedDoc": { "ItemReleasedTin": null, "ItemReleasedFio": "", "ItemReleasedPinfl": "" },
	"SellerTin": "303303592",
	"BuyerTin": "200523221",
	"Seller": {
		"Name": "\"GRAND PHARM TRADE\" MCHJ",
		"Account": "20208000000458557001",
		"BankId": "01036",
		"Address": "город Ташкент Алмазарский район UZUMBOG' 10-TOR KO'CHASI, 28-A-UY ",
		"Mobile": "",
		"WorkPhone": "",
		"Oked": "",
		"DistrictId": "",
		"Director": "RAXIMOV ALISHER ABDUXAKIM O‘G‘LI",
		"Accountant": "SHOMURZAYEV XUSNIDDIN G‘ULOM O‘G‘LI",
		"VatRegCode": "326090004631",
		"VatRegStatus": 20,
		"BranchCode": "",
		"BranchName": ""
	},
	"Buyer": {
		"Name": "4-SON QURILISH TRESTI",
		"Account": "20210000100176992001",
		"BankId": "00425",
		"Address": "город Ташкент Мирзо-Улугбекский район Xodjayeva 2a ",
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
	"ForeignCompany": { "CountryId": "", "Name": "", "Address": "", "Bank": "", "Account": "" },
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
	"FacturaInvestmentObjectDoc": { "ObjectId": "", "ObjectName": "" },
	"Notes": "",
	"CreatedOperator": "",
	"HasRent": false,
	"FacturaRentDoc": null,
	"MySoliqContractObjectDoc": null
}
```

## 4) Подпись счет-фактуры (ЭЦП)
1) Создайте подпись PKCS#7 над **минифицированным** JSON счет-фактуры.
2) Проставьте timestamp:
	- Endpoint: `POST /frontend/timestamp/pkcs7`
	- Результат: PKCS#7 с меткой времени.

## 5) Отправка счет-фактуры — `POST /api/factura-create`
- Назначение: подписать, отправить в Soliq Service/роуминг и оформить юридически.
- Авторизация: Basic Auth.
- Тело запроса:
```json
{ "sign": "<PKCS7_WITH_TIMESTAMP>" }
```

## Заметки
- Храните учетные данные в секрете (env/secret storage), не коммитьте их.
- GUID должен быть уникальным для каждой счет-фактуры и списка товаров.
- Валидируйте данные NP1 перед сохранением.
- Минифицируйте JSON перед подписью, чтобы избежать расхождений подписи.
