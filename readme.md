# Endereco JSON API 3.0

## Überblick

| Funktion | Beschreibung |
| -------- | ------------ |
| [nameCheck](functions/name-check.md) | Sucht den Vornamen in unserer Datenbank und gibt das Geschlecht, sofern bekannt, zurück. |
| [postCodeAutocomplete](functions/postcode-autocomplete.md) | Liefert mögliche Autocomplete-Vorschläge für die eingegebene PLZ-Fragmente zurück, sofern der zugehörige Ort bekannt ist , auch den Ort. |
| [cityNameAutocomplete](functions/cityname-autocomplete.md) | Gibt mögliche Autocomplete-Vorschläge für die eigegebenen Ort-Fragmente zurück, sofert die PLZ bekannt ist, wird auch die PLZ zurückgeliefert. |
| [streetAutocomplete](functions/street-autocomplete.md) | Gibt mögliche Autocomplete-Vorschläge der Straße zurück. |
| [addressCheck](functions/address-check.md) | Prüft eine angegebene Adresse. Gibt Varianten zurück, falls die Adresse nicht eindeutig ist oder es mögliche Korrekturen der Eingabe gibt. |
| [bankAutocomplete](functions/bank-autocomplete.md) | Gibt mögliche Autocomplete-Vorschläge für den Namen der Bank und die zugehörige Bankleitzahl zurück, anhand der eingegebenen Bankleitzahl-Fragmente. |
| [bankCodeAutocomplete](functions/bankcode-autocomplete.md) | (neu) Gibt den Namen, volle Bankleitzahl und BIC anhand eines Teils der Bankleitzahl zurück. Neuster Stand. |
| [ibanCheck](functions/iban-check.md) | Prüft die eingegebene IBAN. |
| [emailCheck](functions/email-check.md) | Prüft die eMail-Adresse anhand diverser Kriterien auf Syntax und auf Zustellbarkeit |
| [prephoneCheck](functions/prephone-check.md) | Prüft die Vorwahl oder gesamte Telefonnummer. Erkennt ob es sich um eine Mobil- oder Festnetznummer handelt, und gibt die Nummer normalisiert zurück. |
| [doAccounting](functions/do-accounting.md) | Markiert eine Transaktion zur Abrechnung.  |
| [startSession](functions/start-session.md) | Startet eine Session.  |
| [ibanConverter](functions/iban-converter.md) | Konvertiert IBAN zu KTO und BLZ oder umgekehrt. |
| [readinessCheck](functions/readiness-check.md) | Prüft, ob der Service erreichbar ist. |

## Headers

Folgende Header müssen mit jeder Anfrage an den Enderco Service übermittelt werden: **Content-Type**, **X-Auth-Key**, **X-Transaction-Id**, **X-Transaction-Referer**

| Header | Bedeutung | Beispiel |
| ------ | --------- | -------- |
| Content-Type | Content type markierung. Soll immer *application/json* sein. | application/json |
| X-Auth-Key | Auth Key. Wird zur Authentifizierung verwendet. | Beispiel: *a0835c4d0b31b7df976d1153f336d6086b130560e72092808f15b464702ac0fa* |
| X-Transaction-Id | Bei Web Clients werden mehrere logisch zusammenhängende Anfragen unter einer Transaction erfasst, welche mit TID markiert ist. Ab 1.1 generiert der Server die TID und gibt sie in der ersten Antwort-JSON zurück. Dabei soll der Client `not_set` als TID übermitteln. Wenn gar keine Transaction aufgebaut werden soll, sol lder Client `not_required` als TID übermitteln. | 976d1153f336d6086b130560e |
| X-Transaction-Referer | Information von wo innerhalb der Anwendung die Anfragen abgesendet werden. z.B. die Unterseite oder eine spezielle Seite innerhalb einer App | https://domain.de/konto-anlegen |

Folgende Header sollen mitübermittelt werden: **X-Agent**

| Header | Bedeutung | Beispiel |
| ------ | --------- | -------- |
| X-Agent | Eine Kennung des verwendeten Clients. Die Version soll mitangegeben werden. Idealerweise soll auch das Shopsystem und verwendetes Theme mit Version angegeben werden. | client:enderecoclientox 3.2, shop:OXID eShop CE 6.1.0, theme:flow |

## Aufbau einer Anfrage

Es wird ein valides JSON erwartet.

Als Basis sollte folgende JSON Struktur verwendet werden:

```javascript
{
  "jsonrpc": "2.0",
  "id": <int>, 
  "method": <string>,
  "params": {
    <string>: <string|int>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt Protokol und Version an. |
| id | int | Eindeutige Kennung des Requests. Kann zum Beispiel ein Counter sein. Man benötigt eine Id um ein Request einer Response zuordnen zu können, was bei Bulk Processing möglicherweise benötigt werden kann. Man kann die Id weglassen, wenn keine Antwort erwartet wird. |
| method | string | Name der Funktion des Webservices. |
| params | hash | Spezifische Parameter. Je Webservice unterschiedlich. |


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

Aufbau entspricht folgender Struktur:


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

Manchmal kann noch ein `cmd` Feld angehängt werden.

Beispiel:
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
  },
  "cmd": {
    "use_tid": "0sb6a717690d139d034eb817704e22994e639d24a9b5a56bbf79223e36a15193dd8f6369680d2fbcd0436fd73b282ca571ca50f0204b0ae35f1766b27163ba16"
  }
}
```

Dieses Feld enthält ein Befehl für den Client. Zur Zeit wird nur der `use_tid` verwendet, der dem Client mitteilt, dass er die angegebene TID nutzen soll. Sinn dahinter ist, dass ab 1.1 Version der Schnittstelle die TID's serverseitig generiert werden.

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt Protokoll und Version an. |
| id | int | Eindeutige Kennung des Requests. Kann zum Beispiel ein Counter sein. Man benötigt eine Id um ein Request einer Response zuordnen zu können, was bei Bulk Processing möglicherweise benötigt werden kann. |
| meta | hash | Optionales Feld. Hier können zusätzlichen Informationen übergeben werden, welche bei der Response wieder zurückgegeben werden. |
| result.status | array | Statusmeldungen für das Ergebnis. Spezifische Parameter. Je Webservice unterschiedlich. |
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

Der Aufbau soll dabei folgender Struktur entsprechen:


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
| id | int | Eindeutige Kennung des Requests. Kann zum Beispiel ein Counter sein. Man benötigt eine Id um ein Request einer Response zuordnen zu können, was bei Bulk Processing möglicherweise benötigt werden kann. |
| meta | hash | Optionales Feld. Hier können zusätzlichen Informationen übergeben werden, welche bei der Response wieder zurückgegeben werden. |
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
