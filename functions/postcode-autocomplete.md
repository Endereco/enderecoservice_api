[zur Übersicht](../readme.md)

# postCodeAutocomplete - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "postCodeAutocomplete",
  "params": {
    "country": <string>,
    "language": <string>,
    "postCode": <string>
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
| params.postCode | string | Ein Teil der Postleitzahl, ohne Sternchen, z.B. "970" |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "postCodeAutocomplete",
  "params": {
    "country": "de",
    "language": "de",
    "postCode": "972"
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
        "postCode": "97204",
        "cityName": "Höchberg"
      },
      {
        "postCode": "97209",
        "cityName": "Veitshöchheim"
      },
      {
        "postCode": "97215",
        "cityName": "Simmershofen"
      },
      {
        "postCode": "97218",
        "cityName": "Gerbrunn"
      },
      {
        "postCode": "97222",
        "cityName": "Rimpar"
      },
      {
        "postCode": "97225",
        "cityName": "Zellingen"
      },
      {
        "postCode": "97228",
        "cityName": "Rottendorf"
      }
    ]
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | *A1000* - eine Postleitzahl gefunden, *A2000* - mehrere Postleitzahlen gefunden, *A3000* - keine Varianten gefunden |
| result.predictions | status | Liste der Varianten. |
| result.predictions.postCode | string | Postleitzahl |
| result.predictions.cityName | string | Stadtname. Ist optional. |


[zur Übersicht](../readme.md)
