# Ortsnamen-Autocomplete

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

Für die Ortsnamen-Autocomplete muss ```cityNameAutocomplete``` als Methode angegeben werden

## Eingabeparameter

| Feld     | Erwarteter Wert | Pflicht | Bedeutung                                                                                |
|----------|-----------------|---------|------------------------------------------------------------------------------------------|
| country  | String          | Ja      | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./../country-codes.md) |
| cityName | String          | Ja      | Enthält den Ortsnamen.                                                                   |

## Ausgabefelder

| Feld                        | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|-----------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions                 | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.postCode        | String          | Ja      | Enthält die Postleitzahl zu diesem Vorschlag.                                                                             |
| predictions.cityName        | String          | Ja      | Enthält den Ortsnamen zu diesem Vorschlag.                                                                                |
| predictions.subdivisionCode | String          | Nein    | Enthält den Bundesland-/Regionscode zu diesem Vorschlag. Format: Ländercode + "-" + Bundeslandkürzel                      |
| status                      | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

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
  "method": "cityNameAutocomplete",
  "params": {
    "country": "DE",
    "language": "de",
    "cityName": "Ran"
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
        "cityName": "Randersacker",
        "postCode": "97236",
        "subdivisionCode": "DE-BY"
      },
      {
        "cityName": "Randowtal",
        "postCode": "17291",
        "subdivisionCode": "DE-BB"
      },
      {
        "cityName": "Rangendingen",
        "postCode": "72414",
        "subdivisionCode": "DE-BW"
      },
      {
        "cityName": "Rangsdorf",
        "postCode": "15834",
        "subdivisionCode": "DE-BB"
      },
      {
        "cityName": "Ranis",
        "postCode": "07389",
        "subdivisionCode": "DE-TH"
      },
      {
        "cityName": "Rankwitz",
        "postCode": "17406",
        "subdivisionCode": "DE-MV"
      },
      {
        "cityName": "Rannungen",
        "postCode": "97517",
        "subdivisionCode": "DE-BY"
      },
      {
        "cityName": "Ransbach-Baumbach",
        "postCode": "56235",
        "subdivisionCode": "DE-RP"
      },
      {
        "cityName": "Ranschbach",
        "postCode": "76829",
        "subdivisionCode": "DE-RP"
      },
      {
        "cityName": "Ranstadt",
        "postCode": "63691",
        "subdivisionCode": "DE-HE"
      },
      {
        "cityName": "Ransweiler",
        "postCode": "67808",
        "subdivisionCode": "DE-RP"
      },
      {
        "cityName": "Rantrum",
        "postCode": "25873",
        "subdivisionCode": "DE-SH"
      },
      {
        "cityName": "Rantzau",
        "postCode": "24329",
        "subdivisionCode": "DE-SH"
      }
    ],
    "status": [
      "A2000"
    ]
  }
}
```