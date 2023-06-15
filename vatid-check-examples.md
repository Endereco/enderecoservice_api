# Beispiele für die Umsatzsteuer ID

# Info
- [Ein- und Ausgabefleder](./fields.md)
- [Statuscodes](./statuscodes.md)

# Inhaltsverzeichnis
- [Einfache Prüfung mit gültiger Umsatzsteuer ID](#einfache-prüfung-mit-gültiger-umsatzsteuer-id)
- [Einfache Prüfung mit ungültiger Umsatzsteuer ID](#einfache-prüfung-mit-ungültiger-umsatzsteuer-id)
- [Qualifizierte Prüfung mit güliger und richtig formatierten Umsatzsteuer ID](#qualifizierte-prüfung-mit-güliger-und-richtig-formatierten-umsatzsteuer-ID)
- [Qualifizierte Prüfung mit ungültiger aber richtig formatierter Umsatzsteuer ID](#qualifizierte-prüfung-mit-ungültiger-aber-richtig-umsatzsteuer-ID)
- [Qualifizierte Prüfung mit gültiger aber falsch formatierter Umsatzsteuer ID](#qualifizierte-prüfung-mit-gültiger-aber-falsch-umsatzsteuer-ID)
- [Qualifizierte Prüfung ohne Länderkürzel](#qualifizierte-prüfung-ohne-länderkürzel)
- [Qualifizierte Prüfung mit falschem Länderkürzel](#qualifizierte-prüfung-mit-falschem-länderkürzel)
- [Qualifizierte Prüfung mit ungültigem Format](#qualifizierte-prüfung-mit-ungültigem-format)
- [Prüfung mit fehlenden Pflichtfeldern](#prüfung-mit-fehlenden-pflichtfeldern)

## Aktuell

### Einfache Prüfung mit gültiger Umsatzsteuer ID

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

### Einfache Prüfung mit ungültiger Umsatzsteuer ID

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

### Qualifizierte Prüfung mit güliger und richtig formatierten Umsatzsteuer ID

#### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "ATU76571345",
      "requesterVatID": "DE297464149",
      "companyName": "IQ Digital Health GmbH",
      "companyPostalCode": "5082",
      "companyLocality": "Grödig",
      "companyStreetFull": "Via Sanitas,1"
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
            "vatId": "ATU76571345",
            "companyName": "BS Marketing Solution GmbH",
            "companyAddress": "Pranzing 27, AT-4861 Aurach/Hongar"
         }
      ],
      "cerification": {
         "timestamp": "2023-05-19 13:04:50",
         "source": "api.vat-search.eu"
      },
      "status": [
         "vat_id_valid",
         "vat_id_format_correct"
      ]
   }
}
```

### Qualifizierte Prüfung mit ungültiger aber richtig formatierter Umsatzsteuer ID

#### Anfrage

```
{
   "jsonrpc": "2.0",
   "id": 1,
   "method": "vatIdCheck",
   "params": {
      "vatId": "ATU74538118",
      "requesterVatID": "DE297464149",
      "companyName": "IQ Digital Health GmbH",
      "companyPostalCode": "5082",
      "companyLocality": "Grödig",
      "companyStreetFull": "Via Sanitas,1"
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
         "timestamp": "2023-05-19 13:29:04",
         "source": "api.vat-search.eu"
      },
      "status": [
         "vat_id_invalid",
         "vat_id_format_correct"
      ]
   }
}
```

### Qualifizierte Prüfung mit gültiger aber falsch formatierter Umsatzsteuer ID

#### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "vatIdCheck",
    "params": {
        "vatId": "AT U74538119",
        "requesterVatID": "DE297464149",
        "companyName": "Ey",
        "companyPostalCode": "5082",
        "companyLocality": "Grödig",
        "companyStreetFull": "Via Sanitas,1"
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
            "timestamp": "2023-05-19 13:32:54",
            "source": "api.vat-search.eu"
        },
        "status": [
            "vat_id_invalid",
            "vat_id_format_invalid"
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
        "vatId": "U74538119",
        "requesterVatID": "DE297464149",
        "companyName": "IQ Digital Health GmbH",
        "companyPostalCode": "5082",
        "companyLocality": "Grödig",
        "companyStreetFull": "Via Sanitas,1"
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
        "vatId": "ÜÜU74538118",
        "requesterVatID": "DE297464149",
        "companyName": "IQ Digital Health GmbH",
        "companyPostalCode": "5082",
        "companyLocality": "Grödig",
        "companyStreetFull": "Via Sanitas,1"
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
        "vatId": "ATU7453811ASDF8",
        "requesterVatID": "DE297464149",
        "companyName": "IQ Digital Health GmbH",
        "companyPostalCode": "5082",
        "companyLocality": "Grödig",
        "companyStreetFull": "Via Sanitas,1"
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
        "öljk": "ATU74538119"
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