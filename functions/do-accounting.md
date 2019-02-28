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
| auth.key | string | API Key. Ermöglicht Zugriff auf die Schnittstelle. |
| transaction.tid | string | Für erfolgsbasiertes Abrechnungsmodell müssen die Anfragen an die Schnittstelle mit Transaction-ID markiert werden. Wenn ein Besucher auf die Seite mit Formular kommt und mindestens eine der Funktionen der Schittstelle in Anspruch nimmt, bekommt er eine Transaction ID. Sobald er "konvertiert" (klickt auf "Weiter", speichert sein Profil, etc), soll eine doAccouting Anfrage abgeschickt werden und die Transaction-Id kann wieder gelöscht werden. |
| transaction.referer | string | URL der Seite, auf welcher die Dienste in Anspruch genommen werden. |
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
