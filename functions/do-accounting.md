[zur Übersicht](../readme.md)

# doAccounting - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "method": "doAccounting"
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
  "method": "doAccounting"
}
```

## Beispiel der Antwort

Auf die Antwort muss man nicht warten.

[zur Übersicht](../readme.md)
