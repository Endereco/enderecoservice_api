# Get-Address-Details

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Gültigen Adresse](#gültige-Adresse)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Get-Address-Details muss ```getAddressDetails``` als Methode angegeben werden

## Eingabeparameter

| Feld            | Erwarteter Wert | Pflicht                                            | Bedeutung                                                                              |
|-----------------|-----------------|----------------------------------------------------|----------------------------------------------------------------------------------------|
| country         | String          | Ja                                                 | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](../country-codes.md) |
| postCode        | String          | Ja                                                 | Enthält die Postleitzahl.                                                              |
| cityName        | String          | Ja                                                 | Enthält den Ortsnamen.                                                                 |
| streetFull      | String          | Ja, wenn street und houseNumber nicht gesetzt sind | Enthält die Straße mit Hausnummer und ggf. Adresszusatz.                               |
| street          | String          | Ja, wenn streetFull nicht gesetzt ist              | Enthält nur den Straßennamen.                                                          |
| houseNumber     | String          | Ja, wenn streetFull nicht gesetzt ist              | Enthält die Hausnummer.                                                                |

## Ausgabefelder

| Feld                                | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|-------------------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions                         | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.municipal_district_code | String          | Ja      | Enthält den Amtlichen Gemeindeschlüssel (AGS)                                                                             |
| status                              | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Statuscodes

| Code                      | Bedeutung                                                   |
|---------------------------|-------------------------------------------------------------|
| address_details_found     | Es wurden Details zu der angegebenen Adresse gefunden       |
| address_details_not_found | Es wurden keine Details zu der angegebenen Adresse gefunden |

# Beispiele

## Gültige Adresse

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 3,
  "method": "getAddressDetails",
  "params": {
    "country": "DE",
    "postCode": "97236",
    "cityName": "Randersacker",
    "streetFull": "Gerbrunner Str. 22"
  }
}
```

### Antwort

```json
{
  "jsonrpc": "2.0",
  "id": 3,
  "result": {
    "predictions": [
      {
        "municipal_district_code": "09679175"
      }
    ],
    "status": [
      "address_details_found"
    ]
  }
}
```