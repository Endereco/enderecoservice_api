# Einfache Umsatzsteuer-Id-Prüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Gültiger Umsatzsteuer-ID](#gültiger-umsatzsteuer-id)
  - [Ungültiger Umsatzsteuer-ID](#ungültiger-umsatzsteuer-id)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Einfache Umsatzsteuer-Id-Prüfung muss ```vatIdCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld              | Erwarteter Wert | Pflicht | Bedeutung                               |
|-------------------|-----------------|---------|-----------------------------------------|
| vatId             | String          | Ja      | Enthält die zu prüfende Umsatzsteuer-ID |

## Ausgabefelder

| Feld              | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|-------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions       | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.vatId | String          | Ja      | Enthält die formatierte Umsatzsteuer-ID der zu prüfenden Firma.                                                           |
| status            | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

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

# Beispiele

## Gültiger Umsatzsteuer-ID

### Anfrage

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

### Antwort

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

## Ungültiger Umsatzsteuer-ID

### Anfrage

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

### Antwort

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