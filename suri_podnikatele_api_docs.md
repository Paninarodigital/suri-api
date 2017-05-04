# SURI podnikatele - API dokumentace

*Paninaro digital - Jan Půlpán - verze 1 - 18.4.2017*


Obsah
=================

* [1. Základní info](#1-základní-info)
* [2. Kroky pro uzavření pojištění](#2-kroky-pro-uzavření-pojištění)
* [3. Návratové hodnoty](#3-návratové-hodnoty)
* [4. Endpoints](#4-endpoints)
    * [insurance](#insurance)
        * [Insurance info](#insurance-info)
        * [Vytvoření Insurance](#vytvoření-insurance)
        * [Změna Insurance](#změna-insurance)
        * [Částečná změna Insurance](#Částečná-změna-insurance)
    * [conclude-insurance](#conclude-insurance)
    * [liability-rates](#liability-rates)
    * [assets-rates](#assets-rates)
    * [assets-participations](#assets-participations)
    * [liability-limits](#liability-limits)
    * [liability-participations](#liability-participations)
    * [ares-info](#ares-info)


## 1. Základní info
  - api je veřejné, bez přihlašování

## 2. Kroky pro uzavření pojištění


## 3. Návratové hodnoty
- 200 - OK (GET a PUT)
- 201 - created (v případě POST, PATCH)


## 4. Endpoints

### insurance
#### Insurance info
Vrací dostupné info o Pojistění.

|   |   |
|---|---|
| GET | /api/v1/insurance/<insurance_id>/ |

*Request*
```
GET /api/v1/insurance/1/
```

*Response*
```
HTTP 200 OK
Content-Type: application/json

{
    "ico": "12345678",
    "company_name": "Moje firma",
    "address_street": "Rybalkova 59",
    "address_city": "Praha 10",
    "address_zip": "10100",
    "liability_limit": 500000,
    "assets_limit": 500000
}
```


#### Vytvoření Insurance

Vytvoří nový Insurance. Povinné jsou jen některá data, viz příklad.

|   |   |
|---|---|
| POST | /api/v1/insurance/ |

| Parametr | Optional | Popis | Hodnota |
|---|---|---|---
| ico | required |   |   |
| company_name | required |   |   |
| address_street | required |   |   |
| address_city | required |   |   |
| address_zip | required |   |   |
| liability_limit | required |   |   |
| assets_limit | required |   |   |
| branches | optional | seznam adres provozoven (maximálně 5)   |   |

*Request*
```
POST /api/v1/insurance/

{
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 494",
    "address_city": "Liberec",
    "address_zip": "46001", 
    "liability_limit": 500000,
    "assets_limit": 500000,
    "branches": [
        {
            "address_street": "Sokolovské nám. 307",
            "address_city": "Liberec",
            "address_zip": "46001"
        }
    ]
}
```

*Response*
```
HTTP 201 Created
{
    "id": 11,
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 494",
    "address_city": "Liberec",
    "address_zip": "46001",
    "liability_limit": 500000,
    "assets_limit": 500000,
    "branches": [
        {
            "id": 15,
            "address_street": "Sokolovské nám. 307",
            "address_city": "Liberec",
            "address_zip": "46001"
        }
    ]
}
```



#### Změna Insurance
Nahrazuje info o Insuranceu novými daty. Povinné jsou všechny položky. Pokud je některá vynechána, nastaví se na prázdnou hodnotu.

|   |   |
|---|---|
| PUT | /api/v1/insurance/<insurance_id>/ |

| Parametr | Optional | Popis | Hodnota |
|---|---|---|---
| ico | required |   |   |
| company_name | required |   |   |
| address_street | required |   |   |
| address_city | required |   |   |
| address_zip | required |   |   |
| liability_limit | required |   |   |
| assets_limit | required |   |   |
| branches | optional | seznam adres provozoven (maximálně 5)   |   |

*Request*
```
PUT /api/v1/insurance/1/

{
    "ico": "12345678",
    "company_name": "Moje firma",
    "address_street": "Rybalkova 59",
    "address_city": "Praha 10",
    "address_zip": "10100",
    "liability_limit": 500000,
    "assets_limit": 500000
}
```

*Response*
```
HTTP 200 OK
Content-Type: application/json

{
    "ico": "12345678",
    "company_name": "Moje firma",
    "address_street": "Rybalkova 59",
    "address_city": "Praha 10",
    "address_zip": "10100",
    "liability_limit": 500000,
    "assets_limit": 500000
}
```



#### Částečná změna Insurance
Nastaví vybrané položky na nové hodnoty. Je možné upravovat jen některá data, ty která nejsou zahrnuta zůstanou bez změny.

|   |   |
|---|---|
| PATCH | /api/v1/insurance/<insurance_id>/ |

| Parametr | Optional | Popis | Hodnota |
|---|---|---|---
| ico | optional |   |   |
| company_name | optional |   |   |
| address_street | optional |   |   |
| address_city | optional |   |   |
| address_zip | optional |   |   |
| liability_limit | optional |   |   |
| liability_income | optional |   |   |
| liability_territory | optional |   |   |
| assets_limit | optional |   |   |
| branches | optional | seznam adres provozoven (maximálně 5)   |   |


*Request*
```
PATCH /api/v1/insurance/11/

{
    "liability_limit": 1000000,
    "liability_income": 1000000,
    "liability_territory": true,
    "assets_limit": 1000000
}
```

*Response*
```
HTTP 201 Created

{
    "id": 11,
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 494",
    "address_city": "Liberec",
    "address_zip": "46001",
    "liability_limit": 1000000,
    "liability_income": 1000000,
    "liability_territory": true,
    "assets_limit": 1000000,
    "branches": [
        {
            "id": 15,
            "address_street": "Sokolovské nám. 307",
            "address_city": "Liberec",
            "address_zip": "46001"
        }
    ]
}
```

*Request - update provozovny*
```
PATCH /api/v1/insurance/11/

{
    "branches": [
        {
            "id": 15,
            "address_street": "Sokolovské nám. 307",
            "address_city": "Liberec 2",
            "address_zip": "46001"
        }
    ]
}
```

*Response*
```
HTTP 201 Created

{
    "id": 11,
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 494",
    "address_city": "Liberec",
    "address_zip": "46001",
    "liability_limit": 1000000,
    "liability_income": 1000000,
    "liability_territory": true,
    "assets_limit": 1000000,
    "branches": [
        {
            "id": 15,
            "address_street": "Sokolovské nám. 307",
            "address_city": "Liberec 2",
            "address_zip": "46001"
        }
    ]
}
```




### conclude-insurance
Uzavře vybrané insurance, vygeneruje dokumenty, pošle email a insurance označí jako uzavřené. Dále už nepůjde měnit.

|   |   |
|---|---|
| GET | /api/v1/conclude-insurance/<insurance_id>/ |

*Request*
```
GET /api/v1/conclude-insurance/1/
```

*Response*
```
HTTP 200 OK
{
    "id": 1,
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 494",
    "address_city": "Liberec",
    "address_zip": "46001",
    "liability": true,
    "liability_limit": 1000000,
    "liability_income": 1000000,
    "liability_territory": true,
    "liability_price": 1234,
    "liability_program": 1,
    "assets": true,
    "assets_limit": 1000000,
    "assets_price": 1234,
    "assets_program": 2,
    "acting_firstname": "Jan",
    "acting_lasname": "Novák",
    "acting_designation": "CEO",
    "customer_phone": "12345678",
    "customer_email": "",
    "customer_postal_address_street": "",
    "customer_postal_address_zip": "",
    "customer_postal_address_city": "",
    "contract_number": "12345678",
    "confirm_personal_data": true,
    "branches": [
        {
            "id": 18,
            "address_street": "Sokolovské nám. 307",
            "address_city": "Liberec 2",
            "address_zip": "46001"
        }
    ]
}
```


### liability-rates
Vrací tabulku všech možných hodnot pojištění odpovědnosti podle vybraného limitu, spoluúčasti a teritoria.

|   |   |
|---|---|
| GET | /api/v1/liability-rates/ |


*Request*

```
GET /api/v1/assets-participations/
```

*Response*
Jen část, celá odpověď má 96 bloků.
```
HTTP 200 OK

[
    {
        "id": 1,
        "limit": 1000000,
        "income": 1000000,
        "territory_coef": 1.0,
        "participation_coef": 1.0,
        "basic": 700,
        "standard": 1175,
        "premium": 1650
    },
    {
        "id": 2,
        "limit": 1000000,
        "income": 5000000,
        "territory_coef": 1.0,
        "participation_coef": 1.0,
        "basic": 875,
        "standard": 1375,
        "premium": 1875
    },
    {
        "id": 3,
        "limit": 1000000,
        "income": 10000000,
        "territory_coef": 1.0,
        "participation_coef": 1.0,
        "basic": 1200,
        "standard": 1700,
        "premium": 2218
    },
    ...

```


### assets-rates
Vrací tabulku hodnot pojištění majetku pro 3 programy a zvolený limit pojištění a spoluúčast.

|   |   |
|---|---|
| GET | /api/v1/assets-rates/<assets-limit> |


*Request*

```
GET /api/v1/assets-rates/1534000/
```

*Response*
```
HTTP 200 OK

[
    {
        "limit": 1534000,
        "participation_coef": 1.0,
        "basic": 788,
        "standard": 2256,
        "premium": 1615
    },
    {
        "limit": 1534000,
        "participation_coef": 1.15,
        "basic": 907,
        "standard": 2594,
        "premium": 1857
    },
    {
        "limit": 1534000,
        "participation_coef": 1.3,
        "basic": 1025,
        "standard": 2933,
        "premium": 2100
    }
]

```


### assets-participations
Vrací seznam spoluúčastí pro pojištění majetku.

|   |   |
|---|---|
| GET | /api/v1/assets-participations/ |


*Request*
```
GET /api/v1/assets-participations/
```

*Response*
```
HTTP 200 OK
[
    {
        "id": 1,
        "value": 5000,
        "desc": "5000 Kc"
    },
    {
        "id": 2,
        "value": 3000,
        "desc": "3000 Kc"
    },
    {
        "id": 3,
        "value": 1000,
        "desc": "1000 Kc"
    }
]
```


### liability-limits
Vrací seznam hodnot limitů plnění pro pojištění odpovědnosti.

|   |   |
|---|---|
| GET | /api/v1/liability-limits/ |

*Request*
```
GET /api/v1/liability-limits/
```

*Response*
```
HTTP 200 OK
[
    {
        "id": 1,
        "value": 1,
        "desc": "1 milión"
    },
    {
        "id": 2,
        "value": 3,
        "desc": "3 milióny"
    },
    {
        "id": 3,
        "value": 5,
        "desc": "5 miliónů"
    },
    {
        "id": 4,
        "value": 10,
        "desc": "10 miliónů"
    }
]
```


### liability-participations
Vrací seznam spoluúčastí pro pojištění odpovědnosti.

|   |   |
|---|---|
| GET | /api/v1/liability-participations/ |

*Request*
```
GET /api/v1/liability-participations/
```

*Response*
```
HTTP 200 OK
[
    {
        "id": 1,
        "value": 5000,
        "desc": "5000 Kc"
    },
    {
        "id": 2,
        "value": 3000,
        "desc": "3000 Kc"
    },
    {
        "id": 3,
        "value": 1000,
        "desc": "1000 Kc"
    }
]
```


### ares-info
Na základě IČO vrátí informace o firmě. Součástí jsou i činnosti, které nelze pojistit. To zda činnost nelze pojistit je rozlišeno pro odpovědnost a majetek.

|   |   |
|---|---|
| GET | /api/v1/ares-info/<ico_nr>/ |


*Request*
```
GET /api/v1/ares-info/48391301/
```

*Response*
```
HTTP 200 OK
{
    "ico": "48391301",
    "company_name": "RENOMIA, a. s.",
    "address_street": "Holandská 874",
    "address_city": "Brno",
    "address_zip": "63900",
    "subjects": [
        {
            "liability": false,
            "assets": false,
            "subject": "Poradenská a konzultační činnost, zpracování odborných studií a posudků"
        }
    ]
}
```







