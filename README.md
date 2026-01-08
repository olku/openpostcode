# Open Nederlandse Postcode API – Documentatie

[Open Postcode](https://openpostcode.nl) is een eenvoudige API waarmee je op basis van een Nederlandse postcode en huisnummer het bijbehorende adres en woonplaats kunt opvragen. Superhandig om automatisch formulieren aan te vullen, bijvoorbeeld.

De **OpenPostcode API** biedt gratis toegang tot postcode- en adresdata voor heel Nederland. Geen registratie of API key vereist.

## Features

- **Volledig gratis**
- **Geen API key nodig**
- **JSON output die makkelijk te verwerken is**
- **Ideaal voor webformulieren, CRM-integratie of datavalidatie**

## Beschikbare Endpoints

### 1. Adres op basis van postcode en huisnummer

`GET /api/address?postcode=1017XN&huisnummer=42`

**Voorbeeld response:**
```json
{
  "postcode": "1017XN",
  "huisnummer": "42",
  "straat": "Frederiksplein",
  "buurt": "Frederikspleinbuurt",
  "wijk": "De Weteringschans",
  "woonplaats": "Amsterdam",
  "gemeente": "Amsterdam",
  "provincie": "Noord-Holland",
  "latitude": 52.35999,
  "longitude": 4.898108
}
```

### 2. Beschikbare huisnummers op basis van postcode

`GET /api/address?postcode=1017XN`

**Voorbeeld response:**
```json
{
  "error": "Huisnummer not found",
  "suggestions": [
    "26A",
    "26B",
    "26C",
    "26D",
    "26E",
    "26F",
    "26G",
    "26H",
    "26K",
    "28",
    "30",
    "32A",
    "32B",
    "32C",
    "32D",
    "32E",
    "32F",
    "32G",
    "32H",
    "32K",
    "34",
    "38A",
    "38B",
    "38C",
    "38D",
    "40A",
    "40B",
    "40C",
    "40D",
    "42",
    "52",
    "54"
  ]
}
```

### 3. Postcode zoeken op straat en plaats

`GET /api/postcode?straat=Damrak&plaats=Amsterdam`

(optioneel: huisnummer)

**Voorbeeld response:**
```json
{
  "postcodes": [
    "1012LG",
    "1012LH",
    "1012LJ",
    "1012LK",
    "1012LL",
    "1012LM",
    "1012LN",
    "1012LP",
    "1012ZH",
    "1012ZJ"
  ]
}
```

Optioneel met huisnummer:

`GET /api/postcode?straat=Damrak&plaats=Amsterdam&huisnummer=1`

**Voorbeeld response:**
```json
{
  "postcode": "1012LG",
  "huisnummer": "1",
  "straat": "Damrak",
  "buurt": "Nieuwendijk-Noord",
  "wijk": "Burgwallen-Nieuwe Zijde",
  "woonplaats": "Amsterdam",
  "gemeente": "Amsterdam",
  "provincie": "Noord-Holland",
  "latitude": 52.377144,
  "longitude": 4.8980384
}
```

### 4. Adres zoeken op coördinaten

`GET /api/nearest?latitude=52.3702&longitude=4.8952`

**Voorbeeld response:**
```json
{
  "postcode": "1017XN",
  "huisnummer": "42",
  "straat": "Frederiksplein",
  "buurt": "Frederikspleinbuurt",
  "wijk": "De Weteringschans",
  "woonplaats": "Amsterdam",
  "gemeente": "Amsterdam",
  "provincie": "Noord-Holland",
  "latitude": 52.35999,
  "longitude": 4.898108
}
```

---
