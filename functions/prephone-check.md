[zur Übersicht](../readme.md)

# prephoneCheck - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "prephoneCheck",
  "params": {
    "prephoneNumber": <string>,
    "format": <int>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.prephoneNumber | string | Vorwahl bzw. Telefonnummer. |
| params.format | int | Format der Ausgabe. Default: 0 |

*Mögliche Formatierungen:*

| Code | Beschreibung |
| ---- | ------------ |
| 0 | wie Eingabe |
| 1 | nationale Rufnummer nur Zahlen |
| 2 | nationale Rufnummer, / als Trennzeichen |
| 3 | nationale Rufnummer, - als Trennzeichen |
| 4 | nationale Rufnummer nach DIN 5008 |
| 5 | nationale Rufnummer nach E.123 |
| 6 | internationale Rufnummer nur Zahlen |
| 7 | internationale Rufnummer, - als Trennzeichen |
| 8 | internationale Rufnummer nach DIN 5008 |
| 9 | internationale Rufnummer nach E.123 |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "prephoneCheck",
  "params": {
    "prephoneNumber": "017678131341",
    "format": 8
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
            "type": "M", 
            "prephoneNumber": "+49 176 78131341",
            "status": [
                "A1000",
                "A1200"
            ]
    }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | siehe Statuscode Tabelle. |
| result.prephoneNumber | array | Telefonnummer, eventuell formatiert. |
|type|string|*T* - Festnetz, *M* - Mobil, *X* - Vorwahl nicht gefunden

*Statuscodes:*

| Code | Beschreibung |
| ---- | ------------ |
| A1000 | Vorwahl gefunden. |
| A1100 | Vorwahl ist Festnetz. |
| A1200 | Vorwahl ist Mobil. |
| A2000 | Vorwahl nicht gefunden. |


[zur Übersicht](../readme.md)
