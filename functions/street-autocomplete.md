[zur Übersicht](../readme.md)

# streetAutocomplete - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "streetAutocomplete",
  "params": {
    "country": <string>,
    "language": <string>,
    "postCode": <string>,
    "cityName": <string>,
    "street": <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.country | string | ISO 3166-1 Alpha-2 Code des Landes, z.B. "de", "ro" oder "fr" |
| params.language | string | Sprachkürzel für die Ausgabe, z.B. "de" oder "en". Dadurch kann bei internationalen Adressen andere Schreibweisen verwendet werden. Vienna oder Wien. |
| params.postCode | string | Vollständige Postleitzahl. |
| params.cityName | string | Vollständiger Stadtname. |
| params.street | string | Teil des Straßennamens. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "streetAutocomplete",
  "params": {
    "country": "de",
    "language": "de",
    "postCode": "97232",
    "cityName": "Giebelstadt",
    "street": "Lud"
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
    "status": ['A2000'],
    "predictions": [
      {
        "street": "Ludwig-Bauer-Str."
      },
      {
        "street": "Ludwig-Pfeuffer-Ring"
      }
    ]
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - eine Straße gefunden, *A2000* - mehrere Straßen gefunden, *A3000* - keine Varianten gefunden |
| result.predictions | status | Liste der Varianten. |
| result.predictions.street | string | Straßenname. |


[zur Übersicht](../readme.md)
