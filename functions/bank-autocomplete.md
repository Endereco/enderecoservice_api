[zur Übersicht](../readme.md)

# bankAutocomplete - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "bankAutocomplete",
  "params": {
    "bankCode": <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.bankCode | string | Bankleitzahl. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "bankAutocomplete",
  "params": {
    "bankCode": "79050"
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
    "status": ['A1000'],
    "predictions": [
      {
        "bankCode": "79050000",
        "bankName": "Sparkasse Mainfranken Würzburg"
      }
    ]
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - eine Variante gefunden, *A2000* - mehrere Varianten gefunden, *A3000* - keine Varianten gefunden |
| result.predictions | status | Liste der Varianten. |
| result.predictions.bankCode | string | Bankleitzahl. |
| result.predictions.bankName | string | Bankname. |


[zur Übersicht](../readme.md)
