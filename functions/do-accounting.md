[zur Übersicht](../readme.md)

# doAccounting - Beschreibung

## Anfrage

**Wichtig!** Bei jeder "doAccounting" Anfrage muss die Session ID im "X-Transaction-Id" Header übermittelt werden. Zukünftig werden wir 
jedoch auf die Variante wechseln, bei der die Session ID allein oder als eine Liste aus mehreren Session ID's als Parameter
übergeben wird.

Für die Zwischenzeit und für die beste Kompatibilität empfehlen wir sowohl den Header zu setzten, als auch den Parameter.

```javascript
{
  "jsonrpc": "2.0",
  "method": "doAccounting",
  "params": {
    "sessionId": String
  }
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
  "method": "doAccounting", 
  "params": {
    "sessionId": "2fd8db0c-7ede-41cc-a9c1-0de7fb511c33"
  }
}
```

## Beispiel der Antwort

Auf die Antwort muss man nicht warten.

[zur Übersicht](../readme.md)
