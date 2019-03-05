[zur Übersicht](../readme.md)

# nameCheck - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "nameCheck",
  "params": {
    "name" : <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.name | string | Vorname. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "nameCheck",
  "params": {
    "name" : "Robert"
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
    "status": ['A1000', 'A1100'],
    "name": "Robert",
    "gender": 'M'
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - Name gefunden, *A1100* - Name männlich, *A1200* - Name weiblich, *A1300* - Name ist unisex, *A2000* - Name nicht gefunden |
| result.name | status | Eingegebener Vorname. |
| result.gender | string | *F* - weiblich, *M* - männlich, *N* - unisex|unbekannt |

[zur Übersicht](../readme.md)
