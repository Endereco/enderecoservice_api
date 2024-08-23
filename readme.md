# endereco Services-API

Die endereco Services-API ermöglicht einen automatisierbaren Zugriff in Echtzeit auf
die [Services der Firma Endereco UG (haftungsb.) - Gesellschaft für Master Data Quality Management](https://www.endereco.de/services/)
. Darunter fallen die Prüfungen von internationalen postalischen Adressen aus über 200 Ländern/Regionen, die Prüfung von E-Mail-
Adressen, Telefonnummern, Namen und anderen Stammdaten von Personen oder Firmen.

## Über 300 Unternehmen prüfen bereits ihre Stammdaten mit endereco

Egal ob Onlineshop, Warenwirtschaftssystem, CRM oder eine eigenentwickelte Softwarelösung — endereco optimiert die
Qualität der Stammdaten deiner Kunden und erspart Kunden und Mitarbeitern den Mehraufwand, der durch nicht normierte und
fehlerhafte Stammdaten entsteht.

Ein Auszug unserer Referenzen:

|                           |                                 |                                 |                              |
|---------------------------|---------------------------------|---------------------------------|------------------------------|
| ![](./imgs/Lavera.png "") | ![](./imgs/Ravensburger.png "") | ![](./imgs/BasteiLuebbe.png "") | ![](./imgs/Berentzen.png "") |
| ![](./imgs/Ergo.png "")   | ![](./imgs/TechniSat.png "")    | ![](./imgs/DB.png "")           | ![](./imgs/Drykorn.png "")   |

## Übersicht der Funktionen

| **Adresse**             | [Adressprüfung](#adressprüfung)                                       | [PLZ Vorschläge](#postleitzahlen-autocomplete) </br> [Ortsvorschläge](#orts-autocomplete) </br> [Straßenvorschläge](#straßen-autocomplete) | [Adressdetails](#address-details) | [Single Line Adressvorschläge](#single-line-input) |
|-------------------------|-----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|----------------------------------------------------|
| **E-Mail Adresse**      | [E-Mail Prüfung](#e-mail-prüfung)                                     |                                                                                                                                            |                                   |                                                    |
| **Person**              | [Namensprüfung](#namens-prüfung)                                      |                                                                                                                                            |                                   |                                                    |
| **Telefonnummer**       | [Rufnummernprüfung](#telefonnummern-prüfung)                          |                                                                                                                                            |                                   |                                                    |
| **IBAN**                | [IBAN-Prüfung](#iBan-prüfung)                                         |                                                                                                                                            |                                   |                                                    |
| **Unternehmen**         | [Einfache Umsatzsteuer-ID Prüfung](#einfache-umsatzsteuer-id-prüfung) | [Qualifizierte Umsatzsteuer-ID Prüfung](#qualifizierte-umsatzsteuer-id-prüfung)                                                            | [Firmensuche](#firmensuche)       |                                                    |
| **Verpackung**          | [Lucid Check](#lucid-check)                                           |                                                                                                                                            |                                   |                                                    |
| **Technische Methoden** | [Abschluss einer Session](guidelines/sessions-guideline.md#session)   |                                                                                                                                            |                                   |                                                    |

## Plugins und fertige Intergrationen

Auf Basis unserer API entwickeln wir gemeinsam mit Integrationspartnern schlüsselfertige Produkte für verschiedene
Systeme.

| **Shopware**   | [Shopware 5 Plugin](https://github.com/Endereco/endereco-shopware5-client)      | [Shopware 6 Plugin](https://github.com/Endereco/endereco-shopware6-client) |
|----------------|---------------------------------------------------------------------------------|----------------------------------------------------------------------------|
| **Oxid**       | [OXID eSales 4 Modul](https://github.com/Endereco/endereco-oxid4-client)        | [OXID eSales 6 Modul](https://github.com/Endereco/endereco-oxid6-client)   |
| **JTL**        | [JTL Shop 4 Plugin](https://github.com/Endereco/endereco-jtl4-client)           | [JTL Shop 5 Plugin](https://github.com/Endereco/endereco-jtl5-client)      |
| **Plenty**     | [Plentymarkets Extension](https://www.endereco.de/plentymarkets/)               |                                                                            |
| **DreamRobot** | [DreamRobot App](https://www.endereco.de/dreamrobot/)                           |                                                                            |
| **Magento**    | [Magento 2 Webshop Extension](https://www.endereco.de/magento/)                 |                                                                            |
| **Wordpress**  | [WordPress/WooCommerce Plugin](https://github.com/Endereco/endereco-wp5-client) |                                                                            |

Wenn du Interesse hast, eine eigene Integration gegen unsere API zu entwickeln, sende eine E-Mail an
support@endereco.de.

## DSGVO-konform

Unsere API ist von Grund auf so konzipiert, dass sie bei korrekter Implementierung den Ansprüchen der DSGVO gerecht
wird. Unser internes Datenschutz-Team prüft stets die aktuellen rechtlichen Anforderungen und sorgt für eine
gesetzeskonforme Umsetzung.

## Vorbereitung

Wie kannst du auf unsere API zugreifen?

1. Frage [über dieses Formular](https://www.endereco.de/apikey-beantragen/) einen API-Key an. Für einen
   API-Key muss mit der endereco UG eine Vereinbarung zur Auftragsverarbeitung abgeschlossen werden. Nur so haben wir als Verarbeiter und
   du als Verantwortlicher die rechtliche Grundlage dafür. Die Unterlagen senden wir dir zu.
2. Probiere
   unsere [Postman-Collection](./downloads/Endereco_API_Beispiele.postman_collection.json)
   aus. Dort sind alle gängigen Anfragen schon vorformuliert.
   Die [Import-Datei](./downloads/Endereco_API_Beispiele.postman_collection.json)
   wurde für die aktuelle Version von [Postman](https://www.postman.com/) erstellt.
3. Falls du noch Fragen hast, such dir den passenden Ansprechpartner aus folgender Liste aus.

### Ansprechpartner

| Art der Frage          | zuständige Person | Kontakt                 |
|------------------------|-------------------|-------------------------|
| Geschäftliche Fragen   | Robert Rieser     | info@endereco.de        |
| Technische Fragen      | Ilja Weber        | support@endereco.de     |
| Fragen zur Datenschutz | Lena Schmitt      | datenschutz@endereco.de |

## Request oder Session

Die Leistungen von endereco sind kostenpflichtig. Dabei unterscheiden wir zwischen zwei Arten der Abrechnung.

1. Die Abrechnung je API-Anfrage
2. Die erfolgsbasierte Abrechnung

Endereco ermöglicht es Nutzern, eine Prüfung erst dann in Rechnung zu stellen, wenn diese tatsächlich einen Mehrwert
liefert. Erst wenn ein bestimmter logisch zusammenhängender Datensatz (z.B. eine Lieferadresse) nach der Verarbeitung
durch endereco beim Nutzer verwendet oder gespeichert wird, rechnen wir ab.

Um die erfolgsbasierte Abrechnung zu ermöglichen, müssen die Anfragen in einer Session gebündelt werden. Dieses Vorgehen
haben wir in [einer Anleitung](guidelines/sessions-guideline.md) beschrieben. Jede sessionbasierte Umsetzung wird gemeinsam mit
euch von unserem Team abgenommen und vor dem Livegang freigegeben. Wenn du dazu noch Fragen hast, melde dich bei
unseren [Ansprechspartnern](#ansprechpartner).

## Authentifizierung

Jede API-Anfrage, die an uns gesendet wird, muss einem Nutzer zugeordnet werden können. Dafür muss in der Anfrage der
HTTP-Header "X-Auth-Key" bei jeder Anfrage mit dem Wert des API-Keys gesetzt werden, den wir für dich erstellt haben.

Um unsere API über Postman anzusprechen, kannst du auch in Postman den API-Key hinterlegen. Nachdem du die
Postman-Collection importiert hast, klicke auf "Endereco API-Beispiele", dann auf "Authorization" und dann wähle bei
Type "API-Key" aus. Trage nun bei Key "X-Auth-Key" ein und bei Value den API-Key, den wir für dich erstellt haben. Diese
werden dann bei jedem Request zu jedem API-Endpunkt im Header automatisch mitgesendet.

![](./imgs/postman_api.png "")

## Nutzung der API

Um mit der API zu arbeiten, muss eine POST Anfrage an unseren Server gesendet werden.
Die Struktur der API ist [hier](./structure-api.md) beschrieben

### Adressprüfung

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "addressCheck",
  "params": {
    "country": "DE",
    "language": "de",
    "postCode": "97232",
    "cityName": "Giebelstadt",
    "street": "Lindenstraße",
    "houseNumber": "28"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [
      {
        "cityName": "Giebelstadt",
        "postCode": "97232",
        "houseNumber": "28",
        "street": "Lindenstr.",
        "country": "de"
      }
    ],
    "status": [
      "address_needs_correction",
      "A1100",
      "country_code_correct",
      "postal_code_correct",
      "locality_correct",
      "street_name_needs_correction",
      "building_number_correct"
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/address-check.md)

### Postleitzahlen-Autocomplete

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "postCodeAutocomplete",
  "params": {
    "country": "DE",
    "language": "de",
    "postCode": "972"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [
      {
        "cityName": "Höchberg",
        "postCode": "97204"
      },
      {
        "cityName": "Veitshöchheim",
        "postCode": "97209"
      },
      {
        "cityName": "Simmershofen",
        "postCode": "97215"
      },
      {
        "cityName": "Uffenheim",
        "postCode": "97215"
      },
      {
        "cityName": "Weigenheim",
        "postCode": "97215"
      },
      {
        "cityName": "Gerbrunn",
        "postCode": "97218"
      },
      {
        "cityName": "Rimpar",
        "postCode": "97222"
      },
      {
        "cityName": "Zellingen",
        "postCode": "97225"
      },
      {
        "cityName": "Rottendorf",
        "postCode": "97228"
      }
    ],
    "status": [
      "A2000"
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/post-code-autocomplete.md)

### Orts-Autocomplete

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "cityNameAutocomplete",
  "params": {
    "country": "DE",
    "language": "de",
    "cityName": "Wü"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [
      {
        "cityName": "Wülfershausen",
        "postCode": "97618"
      },
      {
        "cityName": "Wülfrath",
        "postCode": "42489"
      },
      {
        "cityName": "Wülknitz",
        "postCode": "01609"
      },
      {
        "cityName": "Wülknitz",
        "postCode": "06369"
      },
      {
        "cityName": "Wülperode",
        "postCode": "38835"
      },
      {
        "cityName": "Wünsch",
        "postCode": "06255"
      },
      {
        "cityName": "Wünschbach",
        "postCode": "64753"
      }
    ],
    "status": [
      "A2000"
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/city-name-autocomplete.md)

### Straßen-Autocomplete

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "streetAutocomplete",
  "params": {
    "country": "DE",
    "language": "de",
    "cityName": "Würzburg",
    "postCode": "97070",
    "street": "Am "
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [
      {
        "street": "Am Bruderhof",
        "streetName": "Am Bruderhof",
        "buildingNumber": "",
        "additionalInfo": ""
      },
      {
        "street": "Am Pleidenturm",
        "streetName": "Am Pleidenturm",
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

Siehe [Ausführliche Dokumentation](./services/street-autocomplete.md)

### E-Mail-Prüfung

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "emailCheck",
  "params": {
    "email": "rodrigo@endereco.de"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "status": [
      "A1000",
      "email_correct"
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/email-check.md)

### Namens-Prüfung

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "nameCheck",
  "params": {
    "salutation": "m",
    "firstName": "julia",
    "lastName": "Schenk"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [
      {
        "salutation": "f",
        "firstName": "Julia",
        "lastName": "Schenk"
      }
    ],
    "status": [
      "name_is_natural_person",
      "salutation_needs_correction",
      "first_name_needs_correction",
      "name_needs_correction"
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/name-check.md)

### Telefonnummern-Prüfung

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "phoneCheck",
  "params": {
    "phone": "+491796862470"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "original": {
      "phone": "+491796862470"
    },
    "status": [
      "phone_format_e164",
      "phone_is_mobile",
      "phone_correct"
    ],
    "predictions": [
      {
        "phone": "+491796862470",
        "formatNational": "0179 6862470",
        "formatInternational": "+49 179 6862470",
        "formatE164": "+491796862470",
        "formatRFC3966": "tel:+49-179-6862470",
        "region": "Germany",
        "carrier": "O2",
        "countryPrefix": "+49",
        "nationalNumber": "1796862470",
        "nationalPrefix": "0179",
        "nationalBody": "6862470",
        "leadingZeros": 1
      }
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/phone-check.md)

### iBan-Prüfung

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "ibanCheck",
  "params": {
    "iban": "DE89 3704 0044 0532 0130 00"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "original": {
      "iban": "DE89 3704 0044 0532 0130 00"
    },
    "status": [
      "iban_needs_correction",
      "iban_checksum_correct",
      "iban_length_correct",
      "iban_is_sepa"
    ],
    "predictions": [
      {
        "iban": "DE89370400440532013000",
        "countryCode": "DE",
        "checksum": "89",
        "bankCode": "37040044",
        "bankName": "Commerzbank",
        "bankPostalCode": "50447",
        "bankLocality": "Köln",
        "bankShortName": "Commerzbank Köln",
        "bic": "COBADEFFXXX",
        "accountNumber": "0532013000",
        "countryCentralBankName": "Deutsche Bundesbank",
        "countryCentralBankUrl": "http://www.bundesbank.de/"
      }
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/iban-check.md)

### Einfache Umsatzsteuer-ID-Prüfung

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "vatIdCheck",
  "params": {
    "vatId": "DE214205098"
  }
}
```

#### Antwort Basis

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "status": [
      "vat_valid"
    ]
  }
}
```

Siehe [Ausführliche Dokumentation](./services/vat-id-check.md)

### Qualifizierte Umsatzsteuer-ID-Prüfung

#### Body raw (JSON)

```json
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

#### Antwort Basis

```json
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
               "country": "DE",
               "street": "Balthasar-Neumann Str.",
               "houseNumber": "4b"
            }
         }
      ],
      "certification": {
         "ErrorCode": "213",
         "Date": "22.04.2024",
         "Time": "08:58:43",
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
         "vat_id_company_street_full_match",
         "vat_id_company_address_match"
      ]
   }
}
```

Siehe [Ausführliche Dokumentation](./services/vat-id-check-extended.md)

### Firmensuche

#### Body raw (JSON)

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

#### Antwort Basis

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

Siehe [Ausführliche Dokumentation](./services/company-autocomplete.md)

### Lucid-Check

#### Body raw (JSON)

```json
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "lucidCheck",
   "params": {
      "lucidId": "DE3151155944772"
   }
}
```

#### Antwort Basis

```json
{
   "jsonrpc": "2.0",
   "id": 1,
   "result": {
      "predictions": [
         {
            "lucidId": "DE3151155944772",
            "companyName": "foodsbest GmbH",
            "kindOfReferenceNumber": "vat",
            "referenceNumber": "DE311849520",
            "packagingDetailCode": "10"
         }
      ],
      "status": [
         "lucid_correct"
      ]
   }
}
```

Siehe [Ausführliche Dokumentation](./services/lucid-check.md)

### Address-details

#### Body raw (JSON)

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

#### Antwort Basis

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

Siehe [Ausführliche Dokumentation](./services/address-details.md)

### Single-Line-Input

#### Body raw (JSON)

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

#### Antwort Basis

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

Siehe [Ausführliche Dokumentation](./services/address-single-line-autocomplete.md)
