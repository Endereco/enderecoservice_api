# Endereco JSON API 3.0

## Überblick

| Funktion | Beschreibung |
| -------- | ------------ |
| [nameCheck](functions/name-check.md) | Sucht den Namen in unserer Datenbank und gibt das Geschlecht, sofern bekannt, zurück. |
| [postCodeAutocomplete](functions/postcode-autocomplete.md) | Gibt mögliche Varianten der PLZ für bestimmte Eingabe aus. Sofern die Stadt bekannt, auch die Stadt. |
| [cityNameAutocomplete](functions/cityname-autocomplete.md) | Gibt mögliche Varianten der Stadt für bestimmte Eingabe aus. Sofert die PLZ bekannt ist, wird auch die PLZ ausgegeben. |
| [streetAutocomplete](functions/street-autocomplete.md) | Gibt mögliche Varianten der Straße zurück. |
| [addressCheck](functions/address-check.md) | Prüft eine angegebene Adresse. Gibt Varianten zurück, falls die Adresse nicht eindeutig ist oder es mögliche Berichtigungen gibt. |
| [bankAutocomplete](functions/name-check.md) | Gibt den Namen und volle Bankleitzahl anhand eines Teils der Bankleitzahl. |
| [emailCheck](functions/email-check.md) | Prüft die eMail-Adresse. |
| [prephoneCheck](functions/prephone-check.md) | Prüft die Vorwahl oder volle Telefonnummer. Erkennt ob es eine Mobil- oder Festnetznummer ist, gibt die Nummer normalisiert zurück. |
| [doAccounting](functions/do-accounting.md) | Markiert eine Transaction zur Abrechnung.  |

## Headers

Folgende Headers müssen bei jeder Anfrage übermittelt werden: **Content-Type**, **X-Auth-Key**, **X-Transaction-Id**, **X-Transaction-Referer**

| Header | Bedeutung | Beispiel |
| ------ | --------- | -------- |
| Content-Type | Content type markierung. Soll immer *application/json* sein. | application/json |
| X-Auth-Key | Auth Key. Wird zur Authentifizierung verwendet. | Beispiel: *a0835c4d0b31b7df976d1153f336d6086b130560e72092808f15b464702ac0fa* |
| X-Transaction-Id | Transaction ID. Soll für die aktuelle Session des Clients eindeutig sein und nach doAccounting gelöscht werden. es gibt im Moment keine Vorgabe zum Format. | 976d1153f336d6086b130560e |
| X-Transaction-Referer | Angabe von wo aus die Anfragen geschickt werden. z.B. die Unterseite oder eine spezielle Seite in der App | https://domain.de/konto-anlegen |

## Aufbau einer Anfrage

Es wird ein valides JSON erwartet.

Als Basis soll folgende JSON Struktur verwendet werden:

```javascript
{
  "jsonrpc": "2.0",
  "id": <int>,
  "meta": {
    ...
  }  
  "method": <string>,
  "params": {
    <string>: <string|int>
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
| meta | hash | Optionales Feld. Darin können irgendwelche weitere Infos angegeben werden, die bei Antwort wieder zurückgegeben werden. |
| method | string | Name der Funktion der Schnittstelle. |
| params | hash | Spezifische Parameter. Jede Funktion hat eigene. |


*Beispiel:*

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,  
  "method": "nameCheck",
  "params": {
    "name": "Robert"
  }  
}
```

## Aufbau einer Antwort (Erfolg)

Es kann ein valides JSON erwartet werden.

Aufbau soll dabei folgender Struktur entsprechen:


```javascript
{
  "jsonrpc": "2.0",
  "id": <int>,
  "meta": {
    ...
  },
  "result": {
    "status": <array>,
    "payload": <null|array|hash>
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. |
| meta | hash | Optionales Feld. Darin können irgendwelche weitere Infos angegeben werden, die bei Antwort wieder zurückgegeben werden. |
| result.status | array | Statusmeldungen für das Ergebnis. Sind für jede Funktion spezifisch. |
| result.payload | null|array|hash | Rückgabe der Funktion. Enthält die Ergebnise der Funktion. Typ und Struktur sind je Funktion spezifisch. |

*Beispiel:*

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

## Aufbau einer Antwort (Fehler)

Es kann ein valides JSON erwartet werden.

Aufbau soll dabei folgender Struktur entsprechen:


```javascript
{
  "jsonrpc": "2.0",
  "id": <int>,
  "meta": {
    ...
  } ,
  "error": {
    "code": <int>,
    "message": <string>
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. |
| meta | hash | Optionales Feld. Darin können irgendwelche weitere Infos angegeben werden, die bei Antwort wieder zurückgegeben werden. |
| error.code | int | Fehlercode. |
| error.message | string | Fehlermeldung. |

*Beispiel:*

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32700,
    "message": "Fehler beim Parsen der JSON."
  }
}
```
