[zur Übersicht](../readme.md)

# addressCheck - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "addressCheck",
  "params": {
    "country": <string>,
    "language": <string>,
    "postCode": <string>,
    "cityName": <string>,
    "street": <string>,
    "houseNumber": <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| auth.key | string | API Key. Ermöglicht Zugriff auf die Schnittstelle. |
| transaction.tid | string | Für erfolgsbasiertes Abrechnungsmodell müssen die Anfragen an die Schnittstelle mit Transaction-ID markiert werden. Wenn ein Besucher auf die Seite mit Formular kommt und mindestens eine der Funktionen der Schittstelle in Anspruch nimmt, bekommt er eine Transaction ID. Sobald er "konvertiert" (klickt auf "Weiter", speichert sein Profil, etc), soll eine doAccouting Anfrage abgeschickt werden und die Transaction-Id kann wieder gelöscht werden. |
| transaction.referer | string | URL der Seite, auf welcher die Dienste in Anspruch genommen werden. |
| params.country | string | ISO 3166-1 Alpha-2 Code des Landes, z.B. "de", "ro" oder "fr" |
| params.language | string | Sprachkürzel für die Ausgabe, z.B. "de" oder "en". Dadurch kann bei internationalen Adressen andere Schreibweisen verwendet werden. Vienna oder Wien. |
| params.postCode | string | Postleitzahl. |
| params.cityName | string | Stadtname. |
| params.street | string | Straße. |
| params.houseNumber | string | Hausnummer. |

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
    "postCode": "97232",
    "cityName": "Giebelstadt",
    "street": "Lindenstraße",
    "houseNumber": "1"
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
    "status": ['A1100'],
    "predictions": [
      {
        "postCode": "97232",
        "cityName": "Giebelstadt",
        "street": "Lindenstr.",
        "houseNumber": "1"
      }
    ]
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | siehe Tabelle Statuscodes |
| result.predictions | status | Liste der Varianten. |
| result.predictions.postCode | string | Postleitzahl |
| result.predictions.cityName | string | Stadtname. |
| result.predictions.street | string | Straße. |
| result.predictions.houseNumber | string | Hausnummer. |

*Statuscodes:*

| Code | Bedeutung |
| ---- | --------- |
| A1000 | Adresse korrekt. Gibt es wenn mindestens eine Variante der Adresse gefunden wurde und diese der Eingabe identisch ist. |
| A1100 | Adresse korrigiert. Gibt es, wenn es nur eine Variante der Adresse gibt, diese sich jedoch einwenig unterscheidet. |
| A2000 | Mehrere Varianten. |
| A3000 | Keine Varianten. Die Adresse existiert nicht in unseren Datenbanken. |

[zur Übersicht](../readme.md)
