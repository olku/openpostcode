# Open Nederlandse Postcode API v2

[Open Postcode](https://openpostcode.nl) is een API voor het opzoeken van Nederlandse adressen op basis van postcode, huisnummer, straat, plaats of coordinaten. De API is gratis te gebruiken en vereist geen registratie of API-key.

Alle voorbeelden gebruiken de versie 2 API. De basis-URL is `https://openpostcode.nl`; gebruik daarachter de paden die hieronder staan.

## Algemene eigenschappen

- Alle endpoints accepteren `GET`-requests en retourneren JSON.
- Queryparameters mogen als URL-encoded parameters worden meegestuurd.
- `limit` bepaalt het maximale aantal resultaten. Voor adres- en postcodesearch is de standaard 20 en het maximum 100. Voor de dichtstbijzijnde adressen is de standaard 1 en het maximum 20.
- Bij gepagineerde resultaten bevat `meta.nextCursor` een cursor voor de volgende pagina. Geef die ongewijzigd mee als `cursor`; zolang `nextCursor` ontbreekt, is er geen volgende pagina.
- Een adresobject bevat onder andere `straat`, `huisnummer`, `huisletter`, `huisnummertoevoeging`, `postcode`, `woonplaats`, `gemeente`, `provincie`, `lat` en `lon`. Huisletter en toevoeging kunnen `null` zijn.

Succesvolle antwoorden bevatten doorgaans `meta.apiVersion` met waarde `2`, plus query- en normalisatiegegevens. Fouten bevatten een HTTP-foutstatus en een gestructureerd `error`-object met `code` en `message`; aanvullende informatie staat eventueel in `error.details`.

## 1. Adres opzoeken

Zoek een adres met postcode en huisnummer:

```http
GET /api/v2/address?postcode=1017XN&huisnummer=42
```

Voorbeeldantwoord:

```json
{
  "meta": {
    "apiVersion": 2,
    "query": {
      "postcode": "1017XN",
      "huisnummer": "42"
    },
    "normalized": {
      "postcode": "1017XN",
      "huisnummer": 42,
      "huisletter": null,
      "huisnummertoevoeging": null
    },
    "limit": 20
  },
  "postcode": "1017XN",
  "huisnummer": "42",
  "count": 1,
  "results": [
    {
      "straat": "Frederiksplein",
      "huisnummer": 42,
      "huisletter": null,
      "huisnummertoevoeging": null,
      "postcode": "1017XN",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland",
      "lat": 52.35999,
      "lon": 4.898108
    }
  ]
}
```

Voor adressen met een huisletter of toevoeging kun je `huisletter` en `huisnummertoevoeging` apart meegeven. Samengestelde huisnummers zoals `3d` worden ook geaccepteerd.

### Huisnummers bij een postcode

Laat `huisnummer` weg om beschikbare huisnummers op te vragen:

```http
GET /api/v2/address?postcode=1017XN
```

Voorbeeldantwoord:

```json
{
  "meta": {
    "apiVersion": 2,
    "query": {
      "postcode": "1017XN"
    },
    "normalized": {
      "postcode": "1017XN"
    },
    "limit": 20
  },
  "postcode": "1017XN",
  "count": 13,
  "suggestions": ["26", "28", "30", "32", "34", "38", "40", "42", "44", "46", "48", "52", "54"]
}
```

Dit is een succesvolle response (`200`), geen foutmelding. Als er meer resultaten zijn dan de `limit`, gebruik dan de `meta.nextCursor` voor de volgende pagina.

## 2. Postcodes zoeken op straat en plaats

Zoek postcodes voor een straat en woonplaats:

```http
GET /api/v2/postcode?straat=Damrak&plaats=Amsterdam
```

Voorbeeldantwoord:

```json
{
  "meta": {
    "apiVersion": 2,
    "query": {
      "straat": "Damrak",
      "plaats": "Amsterdam"
    },
    "normalized": {
      "huisnummer": null
    },
    "limit": 20
  },
  "straat": "Damrak",
  "plaats": "Amsterdam",
  "huisnummer": null,
  "count": 10,
  "results": [
    {
      "postcode": "1012LG",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012LH",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012LJ",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012LK",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012LL",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012LM",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012LN",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012LP",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012ZH",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    },
    {
      "postcode": "1012ZJ",
      "straat": "Damrak",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland"
    }
  ]
}
```

Met de optionele parameter `huisnummer` beperk je de zoekopdracht tot dat huisnummer:

```http
GET /api/v2/postcode?straat=Damrak&plaats=Amsterdam&huisnummer=1
```

Postcodezoekopdrachten kunnen gepagineerd worden met `limit` en `cursor`. De `results`-array bevat postcodegegevens; gebruik `/api/v2/address` als je de volledige adres- en coordinatengegevens nodig hebt.

## 3. Dichtstbijzijnde adres zoeken

Zoek het dichtstbijzijnde adres met breedtegraad (`latitude`) en lengtegraad (`longitude`):

```http
GET /api/v2/nearest?latitude=52.3702&longitude=4.8952
```

Voorbeeldantwoord:

```json
{
  "meta": {
    "apiVersion": 2,
    "query": {
      "latitude": "52.3702",
      "longitude": "4.8952"
    },
    "normalized": {
      "latitude": 52.3702,
      "longitude": 4.8952
    },
    "limit": 1
  },
  "latitude": 52.3702,
  "longitude": 4.8952,
  "count": 1,
  "results": [
    {
      "straat": "Sint Agnietenstraat",
      "huisnummer": 14,
      "huisletter": null,
      "huisnummertoevoeging": null,
      "postcode": "1012EG",
      "woonplaats": "Amsterdam",
      "gemeente": "Amsterdam",
      "provincie": "Noord-Holland",
      "lat": 52.370315,
      "lon": 4.895204,
      "distance2": 0.00000001
    }
  ]
}
```

Gebruik `limit` om meer dan één nabij adres op te vragen. De adressen staan op volgorde van nabijheid. `distance2` is de berekende kwadratische afstand; de resultaatcoordinaten heten `lat` en `lon`.

## Foutafhandeling

Veelvoorkomende foutstatussen zijn:

- `400 Bad Request`: ontbrekende of ongeldige parameters, of een ongeldige cursor.
- `404 Not Found`: geen passend adres of geen postcode-resultaten.
- `409 Conflict`: het opgegeven huisnummer verwijst naar meerdere varianten. Gebruik de suggesties in `error.details` om de huisletter of toevoeging te specificeren.

Voorbeeld van een foutresponse:

```json
{
  "meta": {
    "apiVersion": 2,
    "query": {
      "postcode": "1017XN",
      "huisnummer": "999999"
    },
    "normalized": {
      "postcode": "1017XN",
      "huisnummer": 999999,
      "huisletter": null,
      "huisnummertoevoeging": null
    },
    "limit": 20
  },
  "error": {
    "code": "HUISNUMMER_NOT_FOUND",
    "message": "Huisnummer not found",
    "details": {
      "postcode": "1017XN",
      "huisnummer": "999999",
      "suggestions": ["26", "28", "30"]
    }
  }
}
```
