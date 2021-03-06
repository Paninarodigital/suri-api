# SURI podnikatele - API dokumentace

*Paninaro digital - Jan Půlpán - verze 1 - update 14.5.2017*


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
    * [pay-insurance](#pay-insurance)
    * [liability-rates](#liability-rates)
    * [assets-rates](#assets-rates)
    * [assets-participations](#assets-participations)
    * [liability-limits](#liability-limits)
    * [liability-participations](#liability-participations)
    * [ares-info](#ares-info)


## 1. Základní info
API je veřejné, bez přihlašování. Id pojištění jsou zvoleny tak aby nebylo snadné "odhadnout" přístup k datům o pojištění. 

## 2. Kroky pro uzavření pojištění
Toto je doporučený postup pro uzavření pojištění:
- načíst informace o firmě pomocí GET [ares-info](#ares-info)
- založit pojištění po nastavení základních parametrů POST [insurance](#vytvoření-insurance)
- načíst a uživateli nabídnout varianty pojištění odpovědnosti i majetku GET [liability-rates](#liability-rates) a GET [assets-rates](#assets-rates)
- updatovat (i opakovaně) parametry pojištění PATCH [insurance](#vytvoření-insurance) - je třeba nastavovat jak všechny parametry, tak i vybranou cenu pojištění podle tabulek
- nastavení kontakních údajů, potvrzení o souhlasu se zpracováním soukromých údajů PATCH [insurance](#vytvoření-insurance)
- uzavřít pojištění [conclude-insurance](#conclude-insurance) (jednou uzavřené pojištění již nelze měnit)
- zaplatit pojištění [pay-insurance](#pay-insurance) nepovinný krok. Link na zaplacení se posílá i do mailu. 


## 3. Návratové hodnoty
- 200 - OK (GET a PUT)
- 201 - created (v případě POST, PATCH)
- 400 - špatný dotaz (např při uzavírání již uzavřeného pojištění)
- 404 - nenalezeno


## 4. Endpoints

### insurance
#### Insurance info
Vrací dostupné info o Pojistění. V případě, že je některé pojištění neaktivní (nebylo vybráno) tak hodnoty, které se k tomuto pojištění vztahují jsou sice ve výpise uvedeny, ale nemají na pojištění vliv.

|   |   |
|---|---|
| GET | /api/v1/insurance/<insurance_id>/ |

| Parametr | Popis | Hodnota |
|---|---|---
| insurance_id | id pojištění   |   |
| ico | IČO  |   |
| company_name | jméno společnosti   |   |
| address_street | adresa - ulice + číslo  |   |
| address_city | adresa - město  |   |
| address_zip | adresa - PSČ  |   |
| liability | vybáno pojištění odpovědnosti  | true/false  |
| liability_limit | limit plnění  |   |
| liability_income | příjem/obrat   |   |
| liability_territory | oblast | true=EU false=CZ  |
| liability_price | částka za pojištění odpovědnosti   |   |
| liability_program | vybraný program   |  1..basic, 2..standard, 3..premium 0 .. nic nevybráno |
| assets |  vybráno pojištění majetku | true/false   |
| assets_limit | limit plnění  |   |
| assets_price | částka za pojištění majetku  |   |
| assets_program | vybraný program   |  1..basic, 2..standard, 3..premium 0 .. nic nevybrano |
| acting_name | jméno jednajícího za právnickou osobu  |   |
| customer_phone | telefonní číslo  |   |
| customer_email | email  |   |
| customer_postal_address_street | korespondenční adresa - ulice  |   |
| customer_postal_address_zip | korespondenční adresa - PSČ |   |
| customer_postal_address_city | korespondenční adresa - město |   |    
| contract_number | číslo uzavřené smlouvy  |  např: 0026213915 |    
| confirm_personal_data | soouhlas se zpracováním osobních údajů  | true/false  |    
| start| začátek pojištění  |  např: 2017-06-01 |    
| end | konec pojištění (začátek + 1 rok) | např: 2018-05-31  |    
| price | celková cena  |   |    
| installment | velikost splátky  |   |    
| calendar | splátkový kalendář  |   |    
| installment_date | nejzaší datum první splátky  |   |    
| branches | seznam adres provozoven (maximálně 5)   |   |

*Request*
```
GET /api/v1/insurance/KCP6HK43OBSZ231LNG74IB7IBEIB0G/
```

*Response*
```
HTTP 200 OK
{
    "insurance_id": "KCP6HK43OBSZ231LNG74IB7IBEIB0G",
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 210",
    "address_city": "Liberec",
    "address_zip": "46001",
    "liability": true,
    "liability_limit": 1000000,
    "liability_income": 1000000,
    "liability_territory": true,
    "liability_price": 4399,
    "liability_program": 3,
    "assets": true,
    "assets_limit": 5000000,
    "assets_price": 3627,
    "assets_program": 3,
    "acting_name": "Karel Test",
    "acting_designation": "",
    "customer_phone": "603123456",
    "customer_email": "karel@test.cz",
    "customer_postal_address_street": "",
    "customer_postal_address_zip": "",
    "customer_postal_address_city": "",
    "contract_number": "100",
    "confirm_personal_data": true,
    "start": "2017-06-01",
    "end": "2018-05-31",
    "price": 8026,
    "installment_frequency": 2,
    "installment": 4013,
    "calendar": "15. 6. 2017, 1. 12. 2017",
    "installment_date": "2017-06-15",
    "branches": [
        {
            "id": 103,
            "address_street": "Frýdlantská 210",
            "address_city": "Liberec",
            "address_zip": "46001"
        },
        {
            "id": 104,
            "address_street": "Jankovcova 1595",
            "address_city": "Praha 7",
            "address_zip": "170 00"
        }
    ]
}
```


#### Vytvoření Insurance

Vytvoří nový objekt Insurance. Povinné jsou jen některá data.

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
| liability | required |   |  true/false |
| liability_income | optional |   | číslo: 1, 5, 10, 20 miliónů  |
| liability_limit | optional |   |  číslo: 1, 3, 5, 10 miliónů |
| assets | required |   | true/false  |
| assets_limit | optional |   |  rozsah: 500000..20000000 | 
| branches | optional | seznam adres provozoven (maximálně 5)   |   |

*Request*
```
POST /api/v1/insurance/

{
    "address_city": "Liberec",
    "address_street": "Frýdlantská 210",
    "address_zip": "46001",
    "assets": true,
    "assets_limit": "5000000",
    "assets_participation": 5000,
    "branches": [
        {
            "address_city": "Liberec",
            "address_street": "Frýdlantská 210",
            "address_zip": "46001"
        },
        {
            "address_city": "Praha 7",
            "address_street": "Jankovcova 1595",
            "address_zip": "170 00"
        }
    ],
    "company_name": "PFP s.r.o.",
    "ico": "25918184",
    "liability": true,
    "liability_income": "1000000",
    "liability_limit": "1000000",
    "liability_participation": 5000
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


*Request - update parametrů*
```
PATCH /api/v1/insurance/KCP6HK43OBSZ231LNG74IB7IBEIB0G/

{
    "assets": true,
    "assets_price": 3627,
    "assets_program": 3,
    "liability": true,
    "liability_price": 4399,
    "liability_program": 3,
    "liability_territory": 1
}
```

*Response*
```
HTTP 201 Created

{
    "acting_designation": "",
    "acting_name": "",
    "address_city": "Liberec",
    "address_street": "Frýdlantská 210",
    "address_zip": "46001",
    "assets": true,
    "assets_limit": 5000000,
    "assets_price": 3627,
    "assets_program": 3,
    "branches": [
        {
            "address_city": "Liberec",
            "address_street": "Frýdlantská 210",
            "address_zip": "46001",
            "id": 103
        },
        {
            "address_city": "Praha 7",
            "address_street": "Jankovcova 1595",
            "address_zip": "170 00",
            "id": 104
        }
    ],
    "calendar": "29. 5. 2017",
    "company_name": "PFP s.r.o.",
    "confirm_personal_data": false,
    "contract_number": "",
    "customer_email": "",
    "customer_phone": "",
    "customer_postal_address_city": "",
    "customer_postal_address_street": "",
    "customer_postal_address_zip": "",
    "end": "2018-05-14",
    "ico": "25918184",
    "installment": 8026,
    "installment_date": "2017-05-29",
    "installment_frequency": 1,
    "insurance_id": "KCP6HK43OBSZ231LNG74IB7IBEIB0G",
    "liability": true,
    "liability_income": 1000000,
    "liability_limit": 1000000,
    "liability_price": 4399,
    "liability_program": 3,
    "liability_territory": true,
    "price": 8026,
    "start": "2017-05-15"
}
```

*Request - update kontaktních údajů + nastavení začátku a splátek*
```
PATCH /api/v1/insurance/KCP6HK43OBSZ231LNG74IB7IBEIB0G/

{
    "acting_name": "Karel Test",
    "confirm_personal_data": true,
    "customer_email": "karel@test.cz",
    "customer_phone": "603123456",
    "customer_postal_address_city": "",
    "customer_postal_address_street": "",
    "customer_postal_address_zip": "",
    "installment_frequency": 2,
    "start": "2017-06-01"
}
```

*Response*
```
HTTP 201 Created

{
    "acting_designation": "",
    "acting_name": "Karel Test",
    "address_city": "Liberec",
    "address_street": "Frýdlantská 210",
    "address_zip": "46001",
    "assets": true,
    "assets_limit": 5000000,
    "assets_price": 3627,
    "assets_program": 3,
    "branches": [
        {
            "address_city": "Liberec",
            "address_street": "Frýdlantská 210",
            "address_zip": "46001",
            "id": 103
        },
        {
            "address_city": "Praha 7",
            "address_street": "Jankovcova 1595",
            "address_zip": "170 00",
            "id": 104
        }
    ],
    "calendar": "15. 6. 2017, 1. 12. 2017",
    "company_name": "PFP s.r.o.",
    "confirm_personal_data": true,
    "contract_number": "",
    "customer_email": "karel@test.cz",
    "customer_phone": "603123456",
    "customer_postal_address_city": "",
    "customer_postal_address_street": "",
    "customer_postal_address_zip": "",
    "end": "2018-05-31",
    "ico": "25918184",
    "installment": 4013,
    "installment_date": "2017-06-15",
    "installment_frequency": 2,
    "insurance_id": "KCP6HK43OBSZ231LNG74IB7IBEIB0G",
    "liability": true,
    "liability_income": 1000000,
    "liability_limit": 1000000,
    "liability_price": 4399,
    "liability_program": 3,
    "liability_territory": true,
    "price": 8026,
    "start": "2017-06-01"
}
```




### conclude-insurance
Uzavře vybrané insurance, vygeneruje dokumenty, pošle email a insurance označí jako uzavřené. Jednou uzavřené pojištění již nelze měnit (PUT a PATCH na [insurance](#vytvoření-insurance) vrací 400 BAD REQUEST.

Response obsahuje i názvy vygenerovaných souborů a jejich popis. Sobory lze získat na adrese http://<adresa serveru>/media/tmp/<nazev souboru>.

|   |   |
|---|---|
| GET | /api/v1/conclude-insurance/<insurance_id>/ |

*Request*
```
GET /api/v1/conclude-insurance/KCP6HK43OBSZ231LNG74IB7IBEIB0G/
```

*Response*
```
HTTP 200 OK
{
    "insurance_id": "E1GFVBINNVQW2V25EUL9V8ICGLOFUR",
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 210",
    "address_city": "Liberec",
    "address_zip": "46001",
    "liability": true,
    "liability_limit": 1000000,
    "liability_income": 5000000,
    "liability_territory": false,
    "liability_price": 1375,
    "liability_program": 2,
    "assets": true,
    "assets_limit": 1000000,
    "assets_price": 1856,
    "assets_program": 2,
    "exc_subjects_supl": false,
    "acting_name": "Karel Test",
    "acting_designation": "",
    "customer_phone": "777 777 777",
    "customer_email": "karel@firma.zeme",
    "customer_postal_address_street": "",
    "customer_postal_address_zip": "",
    "customer_postal_address_city": "",
    "contract_number": "154",
    "confirm_personal_data": true,
    "start": "2017-06-01",
    "end": "2018-05-31",
    "price": 3231,
    "installment_frequency": 3,
    "installment": 807,
    "calendar": "11. 6. 2017, 1. 9. 2017, 1. 12. 2017, 1. 3. 2018",
    "installment_date": "2017-06-11T08:30:16.269039Z",
    "branches": [
        {
            "id": 159,
            "address_street": "Frýdlantská 210",
            "address_city": "Liberec",
            "address_zip": "46001"
        }
    ],
    "documents": [
        {
            "link": "154_SURI_PODNIKATELE_MAJETEK_ODPOVEDNOST.pdf",
            "name": "pojistná smlouva"
        },
        {
            "link": "154_SURI_PODNIKATELE_ZAZNAM_JEDNANI.pdf",
            "name": "záznam z jednání"
        },
        {
            "link": "154_1701_SURI_PRIKAZKUHRADE.pdf",
            "name": "prikaz k uhrade"
        }
    ]
}
```


### pay-insurance
Vytvoří link pro zaplacení vybraného pojištění. Tento link je pak třeba z klienta otevřít. Po dokončení platby přesměrovává brána zpět na klienta a je třeba toto přesměrování obsloužit. Více informací na https://data.comgate.cz/assets/payments/doc/ComGate_Payments_interface_protocol_HTTP_POST_cs_v1.0.pdf?v=113

|   |   |
|---|---|
| GET | /api/v1/pay-insurance/<insurance_id>/ |

*Request*
```
GET /api/v1/pay-insurance/KCP6HK43OBSZ231LNG74IB7IBEIB0G/
```

*Response*
```
HTTP 200 OK
{
    "url": "https://payments.comgate.cz/client/instructions/index?id=CPGB-BTN9-FKMI"
}
```


### liability-rates
Vrací tabulku všech možných hodnot pojištění odpovědnosti podle vybraného limitu, spoluúčasti, teritoria a případného připojištění nepojistitelných činností.

|   |   |
|---|---|
| GET | /api/v1/liability-rates/ |


*Request*

```
GET /api/v1/liability-rates/
```

*Response*
Jen část, celá odpověď má 96 bloků.
```
HTTP 200 OK

[
    {
        "id": 96,
        "limit": 1000000,
        "income": 1000000,
        "territory_coef": 1.0,
        "participation_coef": 1.0,
        "basic": 700,
        "standard": 1175,
        "premium": 1650,
        "exc_subjects_coef": 1.0
    },
    {
        "id": 97,
        "limit": 1000000,
        "income": 5000000,
        "territory_coef": 1.0,
        "participation_coef": 1.0,
        "basic": 875,
        "standard": 1375,
        "premium": 1875,
        "exc_subjects_coef": 1.0
    },
    {
        "id": 98,
        "limit": 1000000,
        "income": 10000000,
        "territory_coef": 1.0,
        "participation_coef": 1.0,
        "basic": 1200,
        "standard": 1700,
        "premium": 2218,
        "exc_subjects_coef": 1.0
    },
    ... a tak dále. Celkem 144 položek

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
Vrací seznam možných spoluúčastí pro pojištění majetku. Coeficient v odpovědi určuje koeficient pro přepočet částky pojištění a linkuje do tabulky assets-rates.

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
        "desc": "5000 Kc",
        "coeficient": 1.0
    },
    {
        "id": 2,
        "value": 3000,
        "desc": "3000 Kc",
        "coeficient": 1.15
    },
    {
        "id": 3,
        "value": 1000,
        "desc": "1000 Kc",
        "coeficient": 1.3
    }
]
```


### liability-limits
Vrací seznam možných hodnot limitů plnění pro pojištění odpovědnosti.

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
Vrací seznam možných spoluúčastí pro pojištění odpovědnosti.

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
Na základě IČO vrátí informace o firmě. Součástí jsou i činnosti, které nelze pojistit. To zda činnost nelze pojistit je rozlišeno pro odpovědnost a majetek (majetek=assets, odpovědnost=liability). False znamená že pro dannou čínnost nelze pojištění uzavřít. Pokud lze činnost pro odpovědnost připojistit, tak suplement = true)

|   |   |
|---|---|
| GET | /api/v1/ares-info/<ico_nr>/ |


*Request*
```
GET /api/v1/ares-info/25918184/
```

*Response*
```
HTTP 200 OK
{
    "ico": "25918184",
    "company_name": "PFP s.r.o.",
    "address_street": "Frýdlantská 210",
    "address_city": "Liberec",
    "address_zip": "46001",
    "subjects": [
        {
            "liability": false,
            "suplement": true,
            "assets": false,
            "subject": "Velkoobchod a maloobchod"
        }
    ]
}
```







