# Qualifizierte Umsatzsteuer-ID-Prüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Gültiger und richtig formatierter Umsatzsteuer-ID](#gültige-und-richtig-formatierte-umsatzsteuer-id)
  - [Ungültiger, aber richtig formatierter Umsatzsteuer-ID](#ungültige-aber-richtig-formatierte-umsatzsteuer-id)
  - [Gültiger, aber falsch formatierter Umsatzsteuer-ID](#gültige-aber-falsch-formatierte-umsatzsteuer-id)
  - [Ohne Länderkürzel](#ohne-länderkürzel)
  - [Falschem Länderkürzel](#falsches-länderkürzel)
  - [Ungültigem Format](#ungültiges-format)
  - [Prüfung mit fehlenden Pflichtfeldern](#fehlendes-pflichtfelder)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die qualifizierte Umsatzsteuer-ID-Prüfung muss ```vatIdCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld              | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                                                                                                                                                                                                                                          |
|-------------------|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| vatId             | String          | Ja      | Enthält die zu prüfende Umsatzsteuer-ID                                                                                                                                                                                                                                                                                                                            |
| requesterVatId    | String          | Ja      | Enthält die eigene Umsatzsteuer-ID                                                                                                                                                                                                                                                                                                                                 |
| companyName       | String          | Ja      | Enthält den Firmennamen der zu prüfenden Umsatzsteuer-ID                                                                                                                                                                                                                                                                                                           |
| companyPostalCode | String          | Ja      | Enthält die Postleitzahl der zu prüfenden Firma                                                                                                                                                                                                                                                                                                                    |
| companyLocality   | String          | Ja      | Enthält den Ortsnamen der zu prüfenden Firma                                                                                                                                                                                                                                                                                                                       |
| companyStreetFull | String          | Ja      | Enthält den Straßennamen inkl. Hausnummer der zu prüfenden Firma                                                                                                                                                                                                                                                                                                   |
| timeout           | Int             | Nein    | (Beta) Der Timeout bezieht sich nicht auf die Abfrage zum Endereco-Server. Er gibt die Anzahl an Sekunden an, innerhalb derer die Antwort vom Finanzamt eintreffen muss. Sollte diese Zeit überschritten werden, wird ein Fehler zurückgegeben. Es wird eine Zahl zwischen 5 und 60 erwartet. Wenn dieser Parameter nicht gesetzt ist, wird der Wert 60 verwendet. |
| allowCache        | Boolean         | Nein    | (Beta) Mit diesem Parameter kann gesteuert werden, ob in der Antwort nur Live-Daten vom Bundeszentralamt für Steuern oder auch Daten aus dem Cache enthalten sein dürfen. Hier wird entweder true oder false erwartet. Wenn dieser Parameter nicht gesetzt ist, wird false verwendet.                                                                              |


## Ausgabefelder

| Feld                                               | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                                                                                                                                                                      |
|----------------------------------------------------|-----------------|---------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| predictions                                        | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                                                                                                                                                                                 |
| predictions.vatId                                  | String          | Ja      | Enthält die formatierte Umsatzsteuer-ID der zu prüfenden Firma.                                                                                                                                                                                                                                |
| predictions.companyName                            | String          | Ja      | Enthält den Firmennamen zu diesem Vorschlag.                                                                                                                                                                                                                                                   |
| predictions.companyAddress                         | String          | Ja      | Enthält die unstrukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted                | String          | Ja      | Enthält die strukturierte Firmenadresse zu diesem Vorschlag. Wird nur zurückgegeben, wenn die Adresse in strukturierter Form vorliegt                                                                                                                                                          |
| predictions.companyAddressFormatted.streetFull     | String          | Ja      | Enthält die Straße inkl. Hausnummer der strukturierten Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                      |
| predictions.companyAddressFormatted.street         | String          | Ja      | Enthält den Straßennamen der strukturierten Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.houseNumber    | String          | Ja      | Enthält die Hausnummer der strukturierten Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                   |
| predictions.companyAddressFormatted.additionalInfo | String          | Ja      | Enthält den Adresszusatz der strukturierten Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.postCode       | String          | Ja      | Enthält die Postleitzahl der strukturierten Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.cityName       | String          | Ja      | Enthält den Ortsname der strukturierten Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                     |
| predictions.companyAddressFormatted.country        | String          | Ja      | Enthält den Ländercode der strukturierten Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                   |
| certification                                      | JSON            | Ja      | Enthält das Prüfzertifikat.                                                                                                                                                                                                                                                                    |
| certificationStatus                                | JSON            | Ja      | Enthält Informationen zum Prüfzertifikat.                                                                                                                                                                                                                                                      |
| certificationStatus.source                         | String          | Ja      | Enthält die Quelle des Prüfzertifikats. Wenn dieser "realtime" ist, dann kommen die Daten direkt vom Bundeszentralamt für Stuern und es handelt sich laut §18e UStG und 18e.1 Abs. 2 Satz 4 UStAE um eine qualifizierte Prüfung. Wenn dieser "cache" ist, dann kommen die Daten aus dem cache. |
| status                                             | Array           | Ja      | Enthält eine Liste mit Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes).                                                                                                                                                                      |

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
| vat_id_company_address_match              | Die angegebene Firmenanschrift passt zu der Firmenanschrift, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                                         |
| vat_id_company_address_partial_match      | Die angegebene Firmenanschrift passt teilweise zu der Firmenanschrift, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                               |
| vat_id_company_address_mismatch           | Die angegebene Firmenanschrift passt nicht zu der Firmenanschrift, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                                   |
| vat_id_company_postal_code_match          | Die angegebene Postleitzahl der Firmenanschrift passt zu der Postleitzahl, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                           |
| vat_id_company_postal_code_partial_match  | Die angegebene Postleitzahl der Firmenanschrift passt teilweise zu der Postleitzahl, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                 |
| vat_id_company_postal_code_mismatch       | Die angegebene Postleitzahl der Firmenanschrift passt nicht zu der Postleitzahl, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                     |
| vat_id_company_locality_match             | Der angegebene Ortsname der Firmenanschrift passt zu dem Ortsnamen, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                                  |
| vat_id_company_locality_partial_match     | Der angegebene Ortsname der Firmenanschrift passt teilweise zu dem Ortsname, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                         |
| vat_id_company_locality_mismatch          | Der angegebene Ortsname der Firmenanschrift passt nicht zu dem Ortsname, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                             |
| vat_id_company_street_full_match          | Die angegebene Straßenname mit Hausnummer der Firmenanschrift passt zu dem Straßenname mit Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.               |
| vat_id_company_street_full_partial_match  | Die angegebene Straßenname mit Hausnummer der Firmenanschrift passt teilweise zu dem Straßenname mit Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt      |
| vat_id_company_street_full_mismatch       | Die angegebene Straßenname mit Hausnummer der Firmenanschrift passt nicht zu dem Straßenname mit Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt          |
| vat_id_company_street_match               | Die angegebene Straßenname der Firmenanschrift passt zu dem Straßenname, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                             |
| vat_id_company_street_partial_match       | Die angegebene Straßenname der Firmenanschrift passt teilweise zu dem Straßenname, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nichtdes BZSt                                     |
| vat_id_company_street_mismatch            | Die angegebene Straßenname der Firmenanschrift passt nicht zu dem Straßenname, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt                                        |
| vat_id_company_house_number_match         | Die angebegene Hausnummer der Firmenanschrift passt zu der Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                               |
| vat_id_company_house_number_partial_match | Die angegebene Hausnummer der Firmenanschrift passt teilweise zu der Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt                                      |
| vat_id_company_house_number_mismatch      | Die angegebene Hausnummer der Firmenanschrift passt nicht zu der Hausnummer, die bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt                                          |
| vat_id_company_name_match                 | Der angegebene Firmenname passt zum Firmennamen, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                                                     |
| vat_id_company_name_partial_match         | Der angegebene Firmenname passt teilweise zum Firmennamen, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                                           |
| vat_id_company_name_mismatch              | Der angegebene Firmenname passt nicht zum Firmennamen, der bei der Handelskammer hinterlegt ist. Das ist die Interpretation von endereco, nicht des BZSt.                                                               |

## Error Codes im Zertifikat:

Im Teil der Zertifikation werden Errorcodes ausgegeben. Diese sind 1:1 vom entsprechenden Datenprovider übernommen, und bewerten die Gültigkeit der abgefragten USt-IdNr.

Beim [Bundeszentralamt für Steuern](https://evatr.bff-online.de/eVatR/xmlrpc/codes) sind folgende Statuscodes (ErrorCodes) für die USt-IdNr. hinterlegt:

| ErrorCode | Beschreibung                                                                                                                                                                                                                                                                                                                                                          |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 200       | Die angefragte USt-IdNr. ist gültig.                                                                                                                                                                                                                                                                                                                                  |
| 201       | Die angefragte USt-IdNr. ist ungültig.                                                                                                                                                                                                                                                                                                                                |
| 202       | Die angefragte USt-IdNr. ist ungültig. Sie ist nicht in der Unternehmerdatei des betreffenden EU-Mitgliedstaates registriert.<br>**Hinweis:** Ihr Geschäftspartner kann seine gültige USt-IdNr. bei der für ihn zuständigen Finanzbehörde in Erfahrung bringen. Möglicherweise muss er einen Antrag stellen, damit seine USt-IdNr. in die Datenbank aufgenommen wird. |
| 203       | Die angefragte USt-IdNr. ist ungültig. Sie ist erst ab dem … gültig (siehe Feld ‚Gueltig_ab‘).                                                                                                                                                                                                                                                                        |
| 204       | Die angefragte USt-IdNr. ist ungültig. Sie war im Zeitraum von … bis … gültig (siehe Felder ‚Gueltig_ab‘ und ‚Gueltig_bis‘).                                                                                                                                                                                                                                          |
| 205       | Ihre Anfrage kann derzeit durch den angefragten EU-Mitgliedstaat oder aus anderen Gründen nicht beantwortet werden. Bitte versuchen Sie es später noch einmal. Bei wiederholten Problemen wenden Sie sich bitte an das Bundeszentralamt für Steuern – Dienstsitz Saarlouis.                                                                                           |
| 206       | Ihre deutsche USt-IdNr. ist ungültig. Eine Bestätigungsanfrage ist daher nicht möglich. Den Grund hierfür können Sie beim Bundeszentralamt für Steuern – Dienstsitz Saarlouis – erfragen.                                                                                                                                                                             |
| 208       | Für die von Ihnen angefragte USt-IdNr. läuft gerade eine Anfrage von einem anderen Nutzer. Eine Bearbeitung ist daher nicht möglich. Bitte versuchen Sie es später noch einmal.                                                                                                                                                                                       |
| 209       | Die angefragte USt-IdNr. ist ungültig. Sie entspricht nicht dem Aufbau, der für diesen EU-Mitgliedstaat gilt.                                                                                                                                                                                                                                                         |
| 210       | Die angefragte USt-IdNr. ist ungültig. Sie entspricht nicht den Prüfziffernregeln, die für diesen EU-Mitgliedstaat gelten.                                                                                                                                                                                                                                            |
| 211       | Die angefragte USt-IdNr. ist ungültig. Sie enthält unzulässige Zeichen (wie z.B. Leerzeichen, Punkt oder Bindestrich).                                                                                                                                                                                                                                                |
| 212       | Die angefragte USt-IdNr. ist ungültig. Sie enthält ein unzulässiges Länderkennzeichen.                                                                                                                                                                                                                                                                                |
| 213       | Sie sind nicht zur Abfrage einer deutschen USt-IdNr. berechtigt.                                                                                                                                                                                                                                                                                                      |
| 214       | Ihre deutsche USt-IdNr. ist fehlerhaft. Sie beginnt mit ‚DE‘ gefolgt von 9 Ziffern.                                                                                                                                                                                                                                                                                   |
| 215       | Ihre Anfrage enthält nicht alle notwendigen Angaben für eine einfache Bestätigungsanfrage (Ihre deutsche USt-IdNr. und die ausländische USt-IdNr.). Ihre Anfrage kann deshalb nicht bearbeitet werden.                                                                                                                                                                |
| 216       | Ihre Anfrage enthält nicht alle notwendigen Angaben für eine qualifizierte Bestätigungsanfrage (Ihre deutsche USt-IdNr., die ausländische USt-IdNr., Firmenname einschl. Rechtsform und Ort). Es wurde eine einfache Bestätigungsanfrage durchgeführt mit folgendem Ergebnis: Die angefragte USt-IdNr. ist gültig.                                                    |
| 217       | Bei der Verarbeitung der Daten aus dem angefragten EU-Mitgliedstaat ist ein Fehler aufgetreten. Ihre Anfrage kann deshalb nicht bearbeitet werden.                                                                                                                                                                                                                    |
| 218       | Eine qualifizierte Bestätigung ist zurzeit nicht möglich. Es wurde eine einfache Bestätigungsanfrage mit folgendem Ergebnis durchgeführt: Die angefragte USt-IdNr. ist gültig.                                                                                                                                                                                        |
| 219       | Bei der Durchführung der qualifizierten Bestätigungsanfrage ist ein Fehler aufgetreten. Es wurde eine einfache Bestätigungsanfrage mit folgendem Ergebnis durchgeführt: Die angefragte USt-IdNr. ist gültig.                                                                                                                                                          |
| 221       | Die Anfragedaten enthalten nicht alle notwendigen Parameter oder einen ungültigen Datentyp. Weitere Informationen erhalten Sie bei den Hinweisen zum Schnittstelle – Aufruf.                                                                                                                                                                                          |
| 223       | Die angefragte USt-IdNr. ist gültig. Die Druckfunktion steht nicht mehr zur Verfügung, da der Nachweis gem. UStAE zu § 18e.1 zu führen ist.                                                                                                                                                                                                                           |
| 999       | Eine Bearbeitung Ihrer Anfrage ist zurzeit nicht möglich. Bitte versuchen Sie es später noch einmal.                                                                                                                                                                                                                                                                  |


# Beispiele

## Gültige Daten - Antwort kommt vom Bundeszentralamt für Steuern

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
      "companyStreetFull": "Balthasar-Neumann Str. 4b",
      "allowCache": false,
      "timeout": "30"
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
                "companyAddress": "Balthasar-Neumann Str. 4 b, 97236, Randersacker",
                "companyAddressFormatted": {
                    "streetFull": "Balthasar-Neumann Str. 4 b",
                    "additionalInfo": "",
                    "postCode": "97236",
                    "cityName": "Randersacker",
                    "country": "DE",
                    "street": "Balthasar-Neumann Str.",
                    "houseNumber": "4 b"
                }
            }
        ],
        "certification": {
            "ErrorCode": "213",
            "Date": "08.12.2025",
            "Time": "11:31:57",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "certificationStatus": {
            "source": "realtime"
        },
        "status": [
            "vat_id_valid",
            "vat_id_format_correct",
            "vat_id_company_name_mismatch",
            "vat_id_company_postal_code_match",
            "vat_id_company_locality_match",
            "vat_id_company_street_full_match",
            "vat_id_company_street_match",
            "vat_id_company_house_number_match",
            "vat_id_company_address_match"
        ]
    }
}
```

## Gültige Daten - Antwort aus dem Cache

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
      "companyStreetFull": "Balthasar-Neumann Str. 4b",
      "allowCache": true,
      "timeout": "5"
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
                "companyAddress": "Balthasar-Neumann Str. 4 b, 97236, Randersacker",
                "companyAddressFormatted": {
                    "streetFull": "Balthasar-Neumann Str. 4 b",
                    "additionalInfo": "",
                    "postCode": "97236",
                    "cityName": "Randersacker",
                    "country": "DE",
                    "street": "Balthasar-Neumann Str.",
                    "houseNumber": "4 b"
                }
            }
        ],
        "certification": {
            "ErrorCode": "213",
            "Date": "08.12.2025",
            "Time": "11:31:57",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "certificationStatus": {
            "source": "cache"
        },
        "status": [
            "vat_id_valid",
            "vat_id_format_correct",
            "vat_id_company_name_mismatch",
            "vat_id_company_postal_code_match",
            "vat_id_company_locality_match",
            "vat_id_company_street_full_match",
            "vat_id_company_street_match",
            "vat_id_company_house_number_match",
            "vat_id_company_address_match"
        ]
    }
}
```

## Gültige und richtig formatierte Umsatzsteuer-ID

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
                "companyAddress": "Balthasar-Neumann Str. 4 b, 97236, Randersacker",
                "companyAddressFormatted": {
                    "streetFull": "Balthasar-Neumann Str. 4 b",
                    "additionalInfo": "",
                    "postCode": "97236",
                    "cityName": "Randersacker",
                    "country": "DE",
                    "street": "Balthasar-Neumann Str.",
                    "houseNumber": "4 b"
                }
            }
        ],
        "certification": {
            "ErrorCode": "213",
            "Date": "08.12.2025",
            "Time": "11:31:57",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "certificationStatus": {
            "source": "realtime"
        },
        "status": [
            "vat_id_valid",
            "vat_id_format_correct",
            "vat_id_company_name_mismatch",
            "vat_id_company_postal_code_match",
            "vat_id_company_locality_match",
            "vat_id_company_street_full_match",
            "vat_id_company_street_match",
            "vat_id_company_house_number_match",
            "vat_id_company_address_match"
        ]
    }
}
```

## Ungültige, aber richtig formatierte Umsatzsteuer-ID

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
            "Date": "08.12.2025",
            "Time": "11:32:46",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "certificationStatus": {
            "source": "realtime"
        },
        "status": [
            "vat_id_invalid",
            "vat_id_format_correct"
        ]
    }
}
```

## Gültige, aber falsch formatierte Umsatzsteuer-ID

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
                "companyAddress": "Balthasar-Neumann Str. 4 b, 97236, Randersacker",
                "companyAddressFormatted": {
                    "streetFull": "Balthasar-Neumann Str. 4 b",
                    "additionalInfo": "",
                    "postCode": "97236",
                    "cityName": "Randersacker",
                    "country": "DE",
                    "street": "Balthasar-Neumann Str.",
                    "houseNumber": "4 b"
                }
            }
        ],
        "certification": {
            "ErrorCode": "213",
            "Date": "08.12.2025",
            "Time": "11:33:23",
            "Name": [],
            "City": [],
            "PostalCode": [],
            "Street": [],
            "Type": "BZST"
        },
        "certificationStatus": {
            "source": "realtime"
        },
        "status": [
            "vat_id_valid",
            "vat_id_format_invalid",
            "vat_id_body_needs_correction",
            "vat_id_body_minor_correction",
            "vat_id_company_name_mismatch",
            "vat_id_company_postal_code_match",
            "vat_id_company_locality_match",
            "vat_id_company_street_full_match",
            "vat_id_company_street_match",
            "vat_id_company_house_number_match",
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

## Falsches Länderkürzel

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

## Ungültiges Format

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

## Fehlendes Pflichtfelder

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
