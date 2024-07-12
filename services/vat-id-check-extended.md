# Qualifizierte Umsatzsteuer-Id-Prüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Gültiger und richtig formatierter Umsatzsteuer-ID](#gültiger-und-richtig-formatierter-umsatzsteuer-id)
  - [Ungültiger, aber richtig formatierter Umsatzsteuer-ID](#ungültiger-aber-richtig-formatierter-umsatzsteuer-id)
  - [Gültiger, aber falsch formatierter Umsatzsteuer-ID](#gültiger-aber-falsch-formatierter-umsatzsteuer-id)
  - [Ohne Länderkürzel](#ohne-länderkürzel)
  - [Falschem Länderkürzel](#falschem-länderkürzel)
  - [Ungültigem Format](#ungültigem-format)
  - [Prüfung mit fehlenden Pflichtfeldern](#fehlenden-pflichtfeldern)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Qualifizierte Umsatzsteuer-Id-Prüfung muss ```vatIdCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld              | Erwarteter Wert | Pflicht | Bedeutung                                                        |
|-------------------|-----------------|---------|------------------------------------------------------------------|
| vatId             | String          | Ja      | Enthält die zu prüfende Umsatzsteuer-ID                          |
| requesterVatID    | String          | Ja      | Enthält die eigene Umsatzsteuer-ID                               |
| companyName       | String          | Ja      | Enthält den Firmennamen der zu prüfenden Umsatzsteuer-ID         |
| companyPostalCode | String          | Ja      | Enthält die Postleitzahl der zu prüfenden Firma                  |
| companyLocality   | String          | Ja      | Enthält den Ortsnamen der zu prüfenden Firma                     |
| companyStreetFull | String          | Ja      | Enthält den Straßennamen inkl. Hausnummer der zu prüfenden Firma |


## Ausgabefelder

| Feld                                               | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|-----------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| predictions                                        | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                                                                                                                                                                                                                                                                                |
| predictions.vatId                                  | String          | Ja      | Enthält die formatierte Umsatzsteuer-ID der zu prüfenden Firma.                                                                                                                                                                                                                                                                                                                               |
| predictions.companyName                            | String          | Ja      | Enthält den Firmennamen zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                                                  |
| predictions.companyAddress                         | String          | Ja      | Enthält die unstrukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                                |
| predictions.companyAddressFormatted                | String          | Ja      | Enthält die strukturierte Firmenadresse zu diesem Vorschlag. Wird nur zurückgegeben, wenn die Adresse in strukturierter Form vorliegt                                                                                                                                                                                                                                                         |
| predictions.companyAddressFormatted.streetFull     | String          | Ja      | Enthält die Straße inkl. Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                      |
| predictions.companyAddressFormatted.street         | String          | Ja      | Enthält den Straßennamen der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.houseNumber    | String          | Ja      | Enthält die Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                   |
| predictions.companyAddressFormatted.additionalInfo | String          | Ja      | Enthält den Adresszusatz der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.postCode       | String          | Ja      | Enthält die Postleitzahl der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.cityName       | String          | Ja      | Enthält den Ortsname der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                     |
| predictions.companyAddressFormatted.country        | String          | Ja      | Enthält den Ländercode der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                   |
| certification                                      | JSON            | Nein    | Enthält Informationen zum Prüfzertifikat. Wird nur zurückgegeben, wenn es sich laut §18e UStG und 18e.1 Abs. 2 Satz 4 UStAE um eine qualifizierte Prüfung handelt. Wenn Prüfzertifikat nicht ausgegeben wird, bedeutet es, dass die Prüfung mit Daten aus Cache verglichen wurde, da BZSt Server nicht verfügbar ist. Die erneute Prüfung von UID zum späteren Zeitpunkt kann notwendig sein. |
| status                                             | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes).                                                                                                                                                                                                                                                                     |

> Alle erfolgreichen Prüfanfragen werden auf Seiten von endereco gespeichert und archiviert.
> 
## Statuscodes

| Code                                      | Bedeutung                                                                                                                                                                                                               |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| vat_id_valid                              | Die Umsatzsteuer-ID ist korrekt.                                                                                                                                                                                        |
| vat_id_invalid                            | Die Umsatzsteuer-ID ist ungültig.                                                                                                                                                                                       |
| vat_id_unknown_status                     | Die Umsatzsteuer-ID konnte nicht auf Gültigkeit geprüft werden.                                                                                                                                                         |
| vat_id_body_needs_correction              | Die Schreibweise der Umsatzsteuer-ID ist korrekturbedürftig.                                                                                                                                                            |
| vat_id_body_minor_correction              | Kleine Änderung der Umsatzsteuer-ID.                                                                                                                                                                                    |
| vat_id_body_major_correction              | Große Änderung der Umsatzsteuer-ID.                                                                                                                                                                                     |
| vat_id_format_invalid                     | Format entspricht nicht der [Spezifikation](https://euipo.europa.eu/tunnel-web/secure/webdav/guest/document_library/Documents/COSME/VAT%20numbers%20EU.pdf).                                                            |
| vat_id_format_correct                     | Format entspricht der [Spezifikation](https://euipo.europa.eu/tunnel-web/secure/webdav/guest/document_library/Documents/COSME/VAT%20numbers%20EU.pdf).                                                                  |
| vat_id_company_address_match              | Die angebegene Firmenanschrift passt zu der Firmenanschrift, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                                         |
| vat_id_company_address_partial_match      | Die angebegene Firmenanschrift passt teilweise zu der Firmenanschrift, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                               |
| vat_id_company_address_mismatch           | Die angebegene Firmenanschrift passt nicht zu der Firmenanschrift, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                                   |
| vat_id_company_postal_code_match          | Die angebegene Postleitzahl der Firmenanschrift passt zu der Postleitzahl, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                           |
| vat_id_company_postal_code_partial_match  | Die angebegene Postleitzahl der Firmenanschrift passt teilweise zu der Postleitzahl, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                 |
| vat_id_company_postal_code_mismatch       | Die angebegene Postleitzahl der Firmenanschrift passt nicht zu der Postleitzahl, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                     |
| vat_id_company_locality_match             | Der angebegener Ortsname von der Firmenanschrift passt zu dem Ortsname, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                              |
| vat_id_company_locality_partial_match     | Der angebegener Ortsname von der Firmenanschrift passt teilweise zu dem Ortsname, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                    |
| vat_id_company_locality_mismatch          | Der angebegener Ortsname von der Firmenanschrift passt nicht zu dem Ortsname, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                        |
| vat_id_company_street_full_match          | Die angebegener Straßenname mit Hausnummer von der Firmenanschrift passt zu dem Straßenname mit Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.          |
| vat_id_company_street_full_partial_match  | Die angebegener Straßenname mit Hausnummer von der Firmenanschrift passt teilweise zu dem Straßenname mit Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt |
| vat_id_company_street_full_mismatch       | Die angebegener Straßenname mit Hausnummer von der Firmenanschrift passt nicht zu dem Straßenname mit Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt     |
| vat_id_company_street_match               | Die angebegener Straßenname von der Firmenanschrift passt zu dem Straßenname, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                        |
| vat_id_company_street_partial_match       | Die angebegener Straßenname von der Firmenanschrift passt teilweise zu dem Straßenname, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt                               |
| vat_id_company_street_mismatch            | Die angebegener Straßenname von der Firmenanschrift passt nicht zu dem Straßenname, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt                                   |
| vat_id_company_house_number_match         | Die angebegene Hausnummer von der Firmenanschrift passt zu der Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                           |
| vat_id_company_house_number_partial_match | Die angebegene Hausnummer von der Firmenanschrift passt teilweise zu der Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt                                  |
| vat_id_company_house_number_mismatch      | Die angebegene Hausnummer von der Firmenanschrift passt nicht zu der Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt                                      |
| vat_id_company_name_match                 | Der angebegener Firmenname passt zu dem Firmenname, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                                                  |
| vat_id_company_name_partial_match         | Der angebegener Firmenname passt teilweise zu dem Firmenname, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                                        |
| vat_id_company_name_mismatch              | Der angebegener Firmenname passt nicht zu dem Firmenname der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht von BZSt.                                                             |

# Beispiele

## Gültiger und richtig formatierter Umsatzsteuer-ID

### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "DE297464149",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "vatId": "DE297464149",
                "companyName": "endereco UG (haftungsbeschränkt) Gesellschaft für Master Data Quality Management",
                "companyAddress": "Balthasar-Neumann Str. 4b, 97236, Randersacker",
                "companyAddressFormatted": {
                    "streetFull": "Balthasar-Neumann Str. 4b",
                    "additionalInfo": "",
                    "postCode": "97236",
                    "cityName": "Randersacker",
                    "country": "DE"
                }
            }
        ],
        "certification": {
            "ErrorCode": "213",
            "Date": "22.04.2024",
            "Time": "10:13:54",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "status": [
            "vat_id_valid",
            "vat_id_format_correct",
            "vat_id_company_name_mismatch",
            "vat_id_company_postal_code_match",
            "vat_id_company_locality_match",
            "vat_id_company_street_full_partial_match",
            "vat_id_company_address_partial_match"
        ]
    }
}
```

## Ungültiger, aber richtig formatierter Umsatzsteuer-ID

### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "DE297464148",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [],
        "certification": {
            "ErrorCode": "213",
            "Date": "22.04.2024",
            "Time": "11:39:01",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "status": [
            "vat_id_invalid",
            "vat_id_format_correct",
            "vat_id_company_postal_code_mismatch",
            "vat_id_company_locality_mismatch",
            "vat_id_company_street_full_mismatch",
            "vat_id_company_address_mismatch"
        ]
    }
}
```

## Gültiger, aber falsch formatierter Umsatzsteuer-ID

### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "DE 297464149",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "vatId": "DE297464149",
                "companyName": "endereco UG (haftungsbeschränkt) Gesellschaft für Master Data Quality Management",
                "companyAddress": "Balthasar-Neumann Str. 4b, 97236, Randersacker",
                "companyAddressFormatted": {
                    "streetFull": "Balthasar-Neumann Str. 4b",
                    "additionalInfo": "",
                    "postCode": "97236",
                    "cityName": "Randersacker",
                    "country": "DE"
                }
            }
        ],
        "certification": {
            "ErrorCode": "213",
            "Date": "22.04.2024",
            "Time": "11:41:24",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "status": [
            "vat_id_unknown_status",
            "vat_id_format_invalid",
            "vat_id_body_needs_correction",
            "vat_id_body_minor_correction",
            "vat_id_company_name_mismatch",
            "vat_id_company_postal_code_match",
            "vat_id_company_locality_match",
            "vat_id_company_street_full_match",
            "vat_id_company_address_match"
        ]
    }
}
```

## Ohne Länderkürzel

### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "297464149",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "status": [
            "vat_id_invalid",
            "vat_id_format_invalid"
        ],
        "predictions": []
    }
}
```

## Falschem Länderkürzel

### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "AA297464149",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "status": [
            "vat_id_invalid",
            "vat_id_format_invalid"
        ],
        "predictions": []
    }
}
```

## Ungültigem Format

### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "DE297464149a",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "status": [
            "vat_id_invalid",
            "vat_id_format_invalid"
        ],
        "predictions": []
    }
}
```

## Fehlenden Pflichtfeldern

### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId2": "AA297464149",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.vatId\" must be set."
    }
}
```