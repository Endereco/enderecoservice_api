# Straßennamen-Autocomplete

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Normales Beispiel](#normales-beispiel)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Straßennamen-Autocomplete muss ```streetAutocomplete``` als Methode angegeben werden

## Eingabeparameter

| Feld     | Erwarteter Wert | Pflicht | Bedeutung                                                                                |
|----------|-----------------|---------|------------------------------------------------------------------------------------------|
| country  | String          | Ja      | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./../country-codes.md) |
| postCode | String          | Ja      | Enthält die Postleitzahl.                                                                |
| cityName | String          | Ja      | Enthält den Ortsnamen.                                                                   |
| street   | String          | Ja      | Enthält nur den Straßennamen.                                                            |

## Ausgabefelder

| Feld                       | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|----------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions                | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.street         | String          | Ja      | Enthält den Straßennamen zu diesem Vorschlag. Veraltet, das Feld "streetName" sollte verwendet werden.                    |
| predictions.streetName     | String          | Ja      | Enthält den Straßennamen zu diesem Vorschlag. Identisch mit dem Feld "street".                                            |
| predictions.buildingNumber | String          | Ja      | Enthält die Hausnummer zu diesem Vorschlag.                                                                               |
| predictions.additionalInfo | String          | Ja      | Enthält den Adresszusatz zu diesem Vorschlag.                                                                             |
| status                     | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Statuscodes

| Code  | Bedeutung                                                  |
|-------|------------------------------------------------------------|
| A1000 | Es gibt nur ein Element in der Vorschlagsliste.            |
| A2000 | Es gibt mehrere Elemente in der Vorschlagsliste.           |
| A3000 | Es wurden keine Elemente für die Vorschlagsliste gefunden. |

# Beispiele

## Normales Beispiel

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "streetAutocomplete",
  "params": {
    "country": "de",
    "language": "de",
    "postCode": "97209",
    "cityName": "Veitshöchheim",
    "street": "g"
  }
}
```

### Antwort

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [
      {
        "street": "Gartenstr.",
        "streetName": "Gartenstr.",
        "buildingNumber": "",
        "additionalInfo": ""
      },
      {
        "street": "Goethestr.",
        "streetName": "Goethestr.",
        "buildingNumber": "",
        "additionalInfo": ""
      },
      {
        "street": "Gablonzer Weg",
        "streetName": "Gablonzer Weg",
        "buildingNumber": "",
        "additionalInfo": ""
      },
      {
        "street": "Günterslebener Str.",
        "streetName": "Günterslebener Str.",
        "buildingNumber": "",
        "additionalInfo": ""
      },
      {
        "street": "Gadheimer Str.",
        "streetName": "Gadheimer Str.",
        "buildingNumber": "",
        "additionalInfo": ""
      }
    ],
    "status": [
      "A2000"
    ]
  }
}
```