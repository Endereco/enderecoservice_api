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
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.country | string | ISO 3166-1 Alpha-2 Code des Landes, z.B. "de", "ro" oder "fr" |
| params.language | string | Sprachkürzel für die Ausgabe, z.B. "de" oder "en". Dadurch kann bei internationalen Adressen andere Schreibweisen verwendet werden. Vienna oder Wien. |
| params.cityName | string | Ein Teil des Stadtnamens, ohne Sternchen, z.B. "Wür" |

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
