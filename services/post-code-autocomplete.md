# Postleitzahlen-Autocomplete

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Beispiel mit Bundesländer](#beispiel-mit-bundesländer)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Postleitzahlen-Autocomplete muss ```postCodeAutocomplete``` als Methode angegeben werden

## Eingabeparameter

| Feld     | Erwarteter Wert | Pflicht | Bedeutung                                                                                |
|----------|-----------------|---------|------------------------------------------------------------------------------------------|
| country  | String          | Ja      | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./../country-codes.md) |
| postCode | String          | Ja      | Enthält die Postleitzahl.                                                                |

## Ausgabefelder

| Feld                        | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|-----------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions                 | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.postCode        | String          | Ja      | Enthält die Postleitzahl zu diesem Vorschlag.                                                                             |
| predictions.cityName        | String          | Ja      | Enthält den Ortsnamen zu diesem Vorschlag.                                                                                |
| predictions.subdivisionCode | String          | Nein    | Enthält den Bundesland-/Regionscode zu diesem Vorschlag. Format: Ländercode + "-" + Bundeslandkürzel                      |
| status                      | Array           | Ja      | Enthält eine Liste mit Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Statuscodes

| Code  | Bedeutung                                                  |
|-------|------------------------------------------------------------|
| A1000 | Es gibt nur ein Element in der Vorschlagsliste.            |
| A2000 | Es gibt mehrere Elemente in der Vorschlagsliste.           |
| A3000 | Es wurden keine Elemente für die Vorschlagsliste gefunden. |

# Beispiele

## Beispiel mit Bundesländer

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "postCodeAutocomplete",
  "params": {
    "country": "de",
    "language": "de",
    "postCode": "9723"
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
        "cityName": "Estenfeld",
        "postCode": "97230",
        "subdivisionCode": "DE-BY"
      },
      {
        "cityName": "Giebelstadt",
        "postCode": "97232",
        "subdivisionCode": "DE-BY"
      },
      {
        "cityName": "Reichenberg",
        "postCode": "97234",
        "subdivisionCode": "DE-BY"
      },
      {
        "cityName": "Randersacker",
        "postCode": "97236",
        "subdivisionCode": "DE-BY"
      },
      {
        "cityName": "Altertheim",
        "postCode": "97237",
        "subdivisionCode": "DE-BY"
      },
      {
        "cityName": "Aub",
        "postCode": "97239",
        "subdivisionCode": "DE-BY"
      }
    ],
    "status": [
      "A2000"
    ]
  }
}
```
