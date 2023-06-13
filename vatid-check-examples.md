# Beispiele für die Namensprüfung

# Inhaltsverzeichnis
- [Aktuell](#aktuell)
  - [Einfache Prüfung mit gültiger Umsatzsteuerid](#einfache-prüfung-mit-gueltiger-umsatzsteuerid)
  - [Einfache Prüfung mit ungültiger Umsatzsteuerid](#einfache-prüfung-mit-ungueltiger-umsatzsteuerid)
  - [Qualifizierte Prüfung mit güliger und richtig formatierten UID](#qualifizierte-prüfung-mit-gueliger-und-richtig-formatierten-uid)
  - [Qualifizierte Prüfung mit ungültiger aber richtig formatierter UID](#qualifizierte-pruefung-mit-ungültiger-aber-richtig-formatierter-uid)
  - [Qualifizierte Prüfung mit gültiger aber falsch formatierter UID](#qualifizierte-pruefung-mit-gueltiger-aber-falsch-formatierter-uid)
  - [Qualifizierte Prüfung ohne Länderkürzel](#qualifizierte-pruefung-ohne-Länderkürzel)
  - [Qualifizierte Prüfung mit falschem Länderkürzel](#qualifizierte-prüfung-mit-falschem-laenderkuerzel)
  - [Qualifizierte Prüfung mit ungültigem Format](#qualifizierte-preufung-mit-ungueltigem-format)
  - [Prüfung mit fehlenden Pflichtfeldern](#pruefung-mit-fehlenden-pflichtfeldern) 
- [Veraltet](#veraltet)
    - [Gültige Umsatzsteuerid](#gueltige-umsatzsteuerid)
    - [Ungülite Umsatzsteuerid](#unguelite-umsatzsteuerid)
    - [Gültige Umsatzsteuerid mit Firmenname](#gueltige-umsatzsteuerid-mit-firmenname)

## Aktuell

### Einfache Prüfung mit gültiger Umsatzsteuerid

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

### Einfache Prüfung mit ungültiger Umsatzsteuerid

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

### Qualifizierte Prüfung mit güliger und richtig formatierten UID

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

### Qualifizierte Prüfung mit ungültiger aber richtig formatierter UID

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

### Qualifizierte Prüfung mit gültiger aber falsch formatierter UID

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

## Veraltet

### Gültige Umsatzsteuerid

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
        "status": [
            "vat_valid"
        ]
    }
}
```

### Ungülite Umsatzsteuerid

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
        "status": [
            "vat_invalid"
        ]
    }
}
```

### Gültige Umsatzsteuerid mit Firmenname

#### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "vatIdCheck",
    "params": {
        "vatId": "ATU76571345"
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
            "vat_valid",
            "vat_has_company_name"
        ],
        "companyName": "BS Marketing Solution GmbH"
    }
}
```