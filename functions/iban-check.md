[zur Übersicht](../readme.md)

# ibanCheck - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "ibanCheck",
  "params": {
    "iban" : <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.iban | string | Vollständige IBAN. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "ibanCheck",
  "params": {
    "iban": "DE80790500000047750831"
  }
}
```

## Beispiel der Antwort (Erfolg)

```
200 Ok
```

```javascript
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "status": [
            "A1000"
        ]
    }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - Iban korrekt, *A4000* - Iban falsch, *A4100* - Iban länge nicht korrekt (muss 22 Zeichen sein, nur Deutschland unterstützt), *A4200* - Prüfnummer falsch bzw. tippfehler in BLZ oder Kontonummer |

[zur Übersicht](../readme.md)
