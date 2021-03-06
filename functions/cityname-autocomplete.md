[zur Übersicht](../readme.md)

# cityNameAutocomplete - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "cityNameAutocomplete",
  "params": {
    "country": <string>,
    "language": <string>,
    "cityName": <string>
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
| params.cityName | string | Ein Teil des Stadtnamens, ohne Sternchen, z.B. "Wür" |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "cityNameAutocomplete",
  "params": {
    "country": "de",
    "language": "de",
    "cityName": "Wür"
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
        "postCode": "56858",
        "cityName": "Würrich"
      },
      {
        "postCode": "66917",
        "cityName": "Würschhauserhof"
      },
      {
        "postCode": "52146",
        "cityName": "Würselen"
      },
      {
        "postCode": "97070",
        "cityName": "Würzburg"
      },
      {
        "postCode": "97072",
        "cityName": "Würzburg"
      },
      {
        "postCode": "97074",
        "cityName": "Würzburg"
      },
      {
        "postCode": "97076",
        "cityName": "Würzburg"
      },
      {
        "postCode": "97078",
        "cityName": "Würzburg"
      },
      {
        "postCode": "97080",
        "cityName": "Würzburg"
      }
    ]
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - ein Stadtname gefunden, *A2000* - mehrere Stadtnamen gefunden, *A3000* - keine Varianten gefunden |
| result.predictions | status | Liste der Varianten. |
| result.predictions.postCode | string | Postleitzahl. Ist optional. |
| result.predictions.cityName | string | Stadtname. |


[zur Übersicht](../readme.md)
