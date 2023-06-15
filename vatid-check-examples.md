# Beispiele für die Umsatzsteuer-ID Prüfung

## Inhaltsverzeichnis
- [Grundstruktur der JSON](#grundstruktur-der-json)
- [Einfache Prüfung mit gültiger Umsatzsteuer-ID](#einfache-pruefung-mit-gueltiger-umsatzsteuer-id)
- [Einfache Prüfung mit ungültiger Umsatzsteuer-ID](#einfache-pruefung-mit-ungueltiger-umsatzsteuer-id)
- [Qualifizierte Prüfung mit gültiger und richtig formatierter Umsatzsteuer-ID](#qualifizierte-pruefung-mit-gueltiger-und-richtig-formatierter-umsatzsteuer-id)
- [Qualifizierte Prüfung mit ungültiger, aber richtig formatierter Umsatzsteuer-ID](#qualifizierte-pruefung-mit-ungueltiger-aber-richtig-formatierter-umsatzsteuer-id)
- [Qualifizierte Prüfung mit gültiger, aber falsch formatierter Umsatzsteuer-ID](#qualifizierte-pruefung-mit-gueltiger-aber-falsch-formatierter-umsatzsteuer-id)
- [Qualifizierte Prüfung ohne Länderkürzel](#qualifizierte-pruefung-ohne-laenderkuerzel)
- [Qualifizierte Prüfung mit falschem Länderkürzel](#qualifizierte-pruefung-mit-falschem-laenderkuerzel)
- [Qualifizierte Prüfung mit ungültigem Format](#qualifizierte-pruefung-mit-ungueltigem-format)
- [Prüfung mit fehlenden Pflichtfeldern](#pruefung-mit-fehlenden-pflichtfeldern)

## Grundstruktur der JSON

Die Grundstruktur der JSON folgt der [JSON-RPC 2.0 Spezifikation](https://www.jsonrpc.org/specification).

Jedoch ist die Spezifikation nicht vollständig umgesetzt. Unter anderem gibt es aktuell keine Möglichkeit, mehrere
Datensätze (Bulk-Processing) zu übermitteln, und die Fehlercodes sind rudimentär und sollten in einer Integration nicht
verarbeitet werden.

| Feld    | Erwarteter Wert     | Bedeutung                                                                                                                                                    |
|---------|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| jsonrpc | "2.0" als String    | Mit diesem Feld wird die Version des Protokolls angegeben. Der Wert ist immer gleich.                                                                        |
| id      | Zahl größer als 0   | Mit diesem Feld wird die ID der Anfrage übergeben. Falls mehrere Anfragen asynchron versendet werden, kann über die ID die Sequenz wiederhergestellt werden. |
| method  | String              | Name [einer der Funktionen](./readme.md#verzeichnis-der-methoden-und-use-cases) der Endereco Services API, z.B. "addressCheck"                               |
| params  | Hash-Array / Object | Eine Sammlung der methodenspezifischen Parameter                                                                                                             |
| result  | Hash-Array / Object | Container für die Antwort bei erfolgreichem Verlauf der Anfrage.                                                                                             |
| error   | Hash-Array / Object | Container für die Fehlermeldung beim fehlerhaften Verlauf der Anfrage.                                                                                       |
| code    | Zahl                | Nummer des Fehlers. Aktuell rudimentär umgesetzt und soll ignoriert werden.                                                                                  |
| message | String              | Fehlermeldung in menschenlesbarer Form.                                                                                                                      |

Siehe auch:

- [Eingabeparameter](./fields.md#eingabeparameter-vatIdCheck)
- [Ausgabefelder](./fields.md#ausgabefelder-vatIdCheck)
- [Statuscodes](./statuscodes.md)

## Beispiele

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

### Qualifizierte Prüfung mit gültiger und richtig formatierter Umsatzsteuer-ID

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

### Qualifizierte Prüfung mit ungültiger, aber richtig formatierter Umsatzsteuer-ID

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

### Qualifizierte Prüfung mit gültiger, aber falsch formatierter Umsatzsteuer-ID

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