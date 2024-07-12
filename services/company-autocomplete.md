# Firmen-Autocomplete

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Beispiel mit Ländercode](#beispiel-mit-ländercode)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Firmen-Autocomplete muss ```companyAutocomplete``` als Methode angegeben werden

## Eingabeparameter

| Feld        | Erwarteter Wert | Pflicht | Bedeutung                                                                                |
|-------------|-----------------|---------|------------------------------------------------------------------------------------------|
| countryCode | String          | Nein    | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./../country-codes.md) |
| companyName | String          | Ja      | Enthällt den Firmennamen nach dem gesucht werden soll                                    |

## Ausgabefelder

| Feld                                               | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                             |
|----------------------------------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------------------|
| predictions                                        | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                        |
| predictions.vatId                                  | String          | Ja      | Enthält die formatierte Umsatzsteuer-ID der zu prüfenden Firma.                                                                       |
| predictions.companyStatus                          | String          | Ja      | Enthält den Status zu der Firma (active, inaktive, unknown)                                                                           |
| predictions.companyName                            | String          | Ja      | Enthält den Firmennamen zu diesem Vorschlag.                                                                                          |
| predictions.companyAddress                         | String          | Ja      | Enthält die unstrukturierte Firmenadresse zu diesem Vorschlag.                                                                        |
| predictions.companyAddressFormatted                | String          | Ja      | Enthält die strukturierte Firmenadresse zu diesem Vorschlag. Wird nur zurückgegeben, wenn die Adresse in strukturierter Form vorliegt |
| predictions.companyAddressFormatted.streetFull     | String          | Ja      | Enthält die Straße inkl. Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                              |
| predictions.companyAddressFormatted.street         | String          | Ja      | Enthält den Straßennamen der strukturierte Firmenadresse zu diesem Vorschlag.                                                         |
| predictions.companyAddressFormatted.houseNumber    | String          | Ja      | Enthält die Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                                           |
| predictions.companyAddressFormatted.additionalInfo | String          | Ja      | Enthält den Adresszusatz der strukturierte Firmenadresse zu diesem Vorschlag.                                                         |
| predictions.companyAddressFormatted.postCode       | String          | Ja      | Enthält die Postleitzahl der strukturierte Firmenadresse zu diesem Vorschlag.                                                         |
| predictions.companyAddressFormatted.cityName       | String          | Ja      | Enthält den Ortsname der strukturierte Firmenadresse zu diesem Vorschlag.                                                             |
| predictions.companyAddressFormatted.country        | String          | Ja      | Enthält den Ländercode der strukturierte Firmenadresse zu diesem Vorschlag.                                                           |
| status                                             | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes).             |

## Statuscodes

| Code  | Bedeutung                                                  |
|-------|------------------------------------------------------------|
| A1000 | Es gibt nur ein Element in der Vorschlagsliste.            |
| A2000 | Es gibt mehrere Elemente in der Vorschlagsliste.           |
| A3000 | Es wurden keine Elemente für die Vorschlagsliste gefunden. |

# Beispiele

## Beispiel mit Ländercode

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "companyAutocomplete",
  "params": {
    "countryCode": "de",
    "companyName": "endereco"
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
        "vatId": "DE297464149",
        "companyStatus": "active",
        "companyName": "endereco UG (haftungsbeschränkt) Gesellschaft für Master Data Quality Management",
        "companyAddressFull": "Balthasar-Neumann Str. 4b, 97236, Randersacker",
        "companyAddressFormatted": {
          "streetFull": "Balthasar-Neumann Str. 4b",
          "additionalInfo": "",
          "postCode": "97236",
          "cityName": "Randersacker",
          "country": "DE",
          "street": "Balthasar-Neumann Str.",
          "houseNumber": "4b"
        }
      }
    ],
    "status": [
      "A1000",
      "company_single_variant"
    ]
  }
}
```