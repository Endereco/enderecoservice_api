# Beispiele für die Umsatzsteuer-ID

# Info
- [Eingabefleder](./fields.md#eingabeparameter-vatIdCheck)
- [Ausgabefleder](./fields.md#ausgabenfelder-vatIdCheck)
- [Statuscodes](./statuscodes.md)

# Inhaltsverzeichnis
- [Einfache Prüfung mit gültiger Umsatzsteuer-ID](#einfache-prüfung-mit-gültiger-umsatzsteuer-id)
- [Einfache Prüfung mit ungültiger Umsatzsteuer-ID](#einfache-prüfung-mit-ungültiger-umsatzsteuer-id)
- [Qualifizierte Prüfung mit güliger und richtig formatierten Umsatzsteuer-ID](#qualifizierte-prüfung-mit-güliger-und-richtig-formatierten-umsatzsteuer-ID)
- [Qualifizierte Prüfung mit ungültiger aber richtig formatierter Umsatzsteuer-ID](#qualifizierte-prüfung-mit-ungültiger-aber-richtig-formatierter-umsatzsteuer-ID)
- [Qualifizierte Prüfung mit gültiger aber falsch formatierter Umsatzsteuer-ID](#qualifizierte-prüfung-mit-gültiger-aber-falsch-formatierter-umsatzsteuer-ID)
- [Qualifizierte Prüfung ohne Länderkürzel](#qualifizierte-prüfung-ohne-länderkürzel)
- [Qualifizierte Prüfung mit falschem Länderkürzel](#qualifizierte-prüfung-mit-falschem-länderkürzel)
- [Qualifizierte Prüfung mit ungültigem Format](#qualifizierte-prüfung-mit-ungültigem-format)
- [Prüfung mit fehlenden Pflichtfeldern](#prüfung-mit-fehlenden-pflichtfeldern)

## Aktuell

### Einfache Prüfung mit gültiger Umsatzsteuer-ID

#### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "vatIdCheck",
    "params": {
        "vatId": "DE297464149"
    }
}
```

#### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "vatId": "DE297464149"
            }
        ],
        "status": [
            "vat_id_valid",
            "vat_id_format_correct"
        ]
    }
}
```

### Einfache Prüfung mit ungültiger Umsatzsteuer-ID

#### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "vatIdCheck",
    "params": {
        "vatId": "DE297464148"
    }
}
```

#### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [],
        "status": [
            "vat_id_invalid",
            "vat_id_format_correct"
        ]
    }
}
```

### Qualifizierte Prüfung mit güliger und richtig formatierten Umsatzsteuer-ID

#### Anfrage

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

#### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "vatId": "DE297464149",
                "companyName": "endereco UG (haftungsbeschränkt) Gesellschaft für Master Data Quality Management",
                "companyAddress": "Balthasar-Neumann Str. 4b, 97236, Randersacker"
            }
        ],
        "cerification": {
            "timestamp": "2023-06-15 11:09:56",
            "source": "api.vat-search.eu"
        },
        "status": [
            "vat_id_valid",
            "vat_id_format_correct"
        ]
    }
}
```

### Qualifizierte Prüfung mit ungültiger aber richtig formatierter Umsatzsteuer-ID

#### Anfrage

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

#### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [],
        "cerification": {
            "timestamp": "2023-06-15 11:10:47",
            "source": "api.vat-search.eu"
        },
        "status": [
            "vat_id_invalid",
            "vat_id_format_correct"
        ]
    }
}
```

### Qualifizierte Prüfung mit gültiger aber falsch formatierter Umsatzsteuer-ID

#### Anfrage

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

#### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "vatId": "DE297464149",
                "companyName": "endereco UG (haftungsbeschränkt) Gesellschaft für Master Data Quality Management",
                "companyAddress": "Balthasar-Neumann Str. 4b, 97236, Randersacker"
            }
        ],
        "cerification": {
            "timestamp": "2023-06-15 11:11:13",
            "source": "api.vat-search.eu"
        },
        "status": [
            "vat_id_valid",
            "vat_id_format_invalid",
            "vat_id_body_needs_correction",
            "vat_id_body_minor_correction"
        ]
    }
}
```

### Qualifizierte Prüfung ohne Länderkürzel

#### Anfrage

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

#### Antwort

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

### Qualifizierte Prüfung mit falschem Länderkürzel

#### Anfrage

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

#### Antwort

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

### Qualifizierte Prüfung mit ungültigem Format

#### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "AA297464149a",
      "requesterVatID": "DE297464149",
      "companyName": "endereco",
      "companyPostalCode": "97236",
      "companyLocality": "Randersacker",
      "companyStreetFull": "Balthasar-Neumann Str. 4b"
   }
}
```

#### Antwort

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

### Prüfung mit fehlenden Pflichtfeldern

#### Anfrage

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

#### Antwort

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.vatId\" must be set."
    }
}
```