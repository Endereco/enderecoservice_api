[zur Übersicht](../readme.md)

# readinessCheck - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "readinessCheck"
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "readinessCheck"
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
        "status": "ready"
    }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | Antwort. Es kommt immer ready zurück. Wenn nichts zurück kommt ist es ein Fehlerfall. |

[zur Übersicht](../readme.md)
