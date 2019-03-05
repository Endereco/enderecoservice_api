[zur Übersicht](../readme.md)

# emailCheck - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "emailCheck",
  "params": {
    "email": <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.email | string | eMail-Adresse. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "emailCheck",
  "params": {
    "email": "ilja@endereco.de"
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
    "status": ['A1000']
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - eMail-Adresse korrekt, *A2000* - eMail-Adresse nicht korrekt |


[zur Übersicht](../readme.md)
