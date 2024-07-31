# Address-Single-Line-Autocomplete

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
    - [Generelle Statuscodes](#generelle-statuscodes)
    - [Veraltete Statuscodes](#veraltete-statuscodes)
- [Beispiele](#beispiele)
  - [Normales Beispiel](#normales-beispiel)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für den Address-Single-Line-Autocomplete muss ```addressAutocomplete``` als Methode angegeben werden

## Eingabeparameter

| Feld        | Erwarteter Wert | Pflicht | Bedeutung                                                                                |
|-------------|-----------------|---------|------------------------------------------------------------------------------------------|
| country     | String          | Ja      | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./../country-codes.md) |
| addressFull | String          | Ja      | Enthält die Adresseingabe. Kann jeden Bestandteil der Adresse beinhalten                 |

## Ausgabefelder

| Feld                        | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|-----------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions                 | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.postCode        | String          | Ja      | Enthält die Postleitzahl zu diesem Vorschlag.                                                                             |
| predictions.cityName        | String          | Ja      | Enthält den Ortsnamen zu diesem Vorschlag.                                                                                |
| predictions.street          | String          | Ja      | Enthält den Straßennamen zu diesem Vorschlag.                                                                             |
| predictions.houseNumber     | String          | Ja      | Enthält die Hausnummer zu diesem Vorschlag.                                                                               |
| predictions.additionalInfo  | String          | Ja      | Enthält den Adresszusatz zu diesem Vorschlag.                                                                             |
| predictions.streetFull      | String          | Ja      | Enthält die formatierte Straße mit Hausnummer inkl. Adresszusatz                                                          |
| predictions.addressFull     | String          | Ja      | Enthält die formatierte Straße mit Hausnummer inkl. Adresszusatz und Postleitzahl mit Ortsnamen                           |
| predictions.subdivisionCode | String          | Nein    | Enthält den Bundesland-/Regionscode zu diesem Vorschlag. Format: Ländercode + "-" + Bundeslandkürzel                      |
| status                      | Array           | Ja      | Enthält eine Liste mit Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Statuscodes

### Generelle Statuscodes

| Code                      | Bedeutung                                                  |
|---------------------------|------------------------------------------------------------|
| address_single_variant    | Es gibt nur ein Element in der Vorschlagsliste.            |
| address_multiple_variants | Es gibt mehrere Elemente in der Vorschlagsliste.           |
| address_no_variants       | Es wurden keine Elemente für die Vorschlagsliste gefunden. |

### Veraltete Statuscodes

!! **DEPRECATED** !!

Falls du noch die alten Statuscodes mit Zahlen nutzt, empfehlen wir diese mit neuen Statuscodes zu
ersetzen, da die alten Statuscodes Mitte/Ende 2022 nicht mehr unterstützt werden.

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
  "method": "addressAutocomplete",
  "params": {
    "country": "de",
    "addressFull": "Gerbrunner Str. 22 97236 Randers"
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
        "country": "DE",
        "postCode": "97236",
        "cityName": "Randersacker",
        "street": "Gerbrunner Str.",
        "houseNumber": "22",
        "additionalInfo": "",
        "streetFull": "Gerbrunner Str. 22",
        "addressFull": "Gerbrunner Str. 22, 97236 Randersacker",
        "subdivisionCode": "DE-BY"
      },
      {
        "country": "DE",
        "postCode": "97236",
        "cityName": "Randersacker",
        "street": "Gerbrunner Str.",
        "houseNumber": "22A",
        "additionalInfo": "",
        "streetFull": "Gerbrunner Str. 22A",
        "addressFull": "Gerbrunner Str. 22A, 97236 Randersacker",
        "subdivisionCode": "DE-BY"
      }
    ],
    "status": [
      "A2000",
      "address_multiple_variants"
    ]
  }
}
```
