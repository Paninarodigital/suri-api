# SURI podnikatele - API dokumentace

*Paninaro digital - Jan Půlpán - verze 1 - 18.4.2017*

## 1. Základní info
  - api je veřejné, bez přihlašování

## 2. Kroky pro uzavření pojištění


## 4. Návratové hodnoty
- 200 - OK (GET a PUT)
- 201 - created (v případě POST, PATCH)


## 5. Endpoints

### Insurance
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

#### Response
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






#### assets-participations
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


#### liability-limits
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


#### liability-participations
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


#### ares-info
Na základě IČO vrátí informace o firmě.

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
            "allowed": true,
            "subject": "Činnost pojišťovacích zprostředkovatelů a samostatných likvidátorů pojistných událostí ve smyslu zákona č. 38/2004 Sb."
        },
        {
            "allowed": true,
            "subject": "Činnost investičního zprostředkovatele v rozsahu povolení uděleného podle zvláštního zákona"
        },
        {
            "allowed": true,
            "subject": "Výroba, obchod a služby neuvedené v přílohách 1 až 3 živnostenského zákona"
        }
    ]
}
```






