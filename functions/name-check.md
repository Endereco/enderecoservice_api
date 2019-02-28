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
| auth.key | string | API Key. Ermöglicht Zugriff auf die Schnittstelle. |
| transaction.tid | string | Für erfolgsbasiertes Abrechnungsmodell müssen die Anfragen an die Schnittstelle mit Transaction-ID markiert werden. Wenn ein Besucher auf die Seite mit Formular kommt und mindestens eine der Funktionen der Schittstelle in Anspruch nimmt, bekommt er eine Transaction ID. Sobald er "konvertiert" (klickt auf "Weiter", speichert sein Profil, etc), soll eine doAccouting Anfrage abgeschickt werden und die Transaction-Id kann wieder gelöscht werden. |
| transaction.referer | string | URL der Seite, auf welcher die Dienste in Anspruch genommen werden. |
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
