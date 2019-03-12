[zur Übersicht](../readme.md)

# ibanConverter - Beschreibung

Im Moment werden nur deutsche IBAN unterstützt.

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "ibanConverter",
  "params": {
    "iban" : <string>
  }  
}
```

oder

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "ibanConverter",
  "params": {
    "accountNr" : <string>,
    "bankCode" : <string>
  }  
}
```


*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.iban | string | Vollständige IBAN. |
| params.accountNr | string | Kontonummer. |
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
  "method": "ibanConverter",
  "params": {
    "iban" : "DE63790500000044649465"
  }
}
```

oder


```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "ibanConverter",
  "params": {
    "accountNr": "0044649465",
    "bankCode": "79050000"
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
            "A1000",
            "A1100"
        ],
        "iban": "DE63790500000044649465",
        "accountNr": "0044649465",
        "bankCode": "79050000"
    }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - Konvertierung war erfolgreich, *A1100* - Iban wurde zerlegt, *A1200* - Iban wurde zusammengebaut |
| result.iban | string | Vollständige IBAN. |
| result.accountNr | string | Kontonummer. |
| result.bankCode | string | Bankleitzahl. |

[zur Übersicht](../readme.md)
