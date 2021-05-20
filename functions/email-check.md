[zur Übersicht](../readme.md)

# emailCheck - Beschreibung

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "emailCheck",
  "params": {
    "email": <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.email | string | eMail-Adresse. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "emailCheck",
  "params": {
    "email": "ilja@endereco.de"
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
    "status": ['A1000']
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | siehe Tabelle der Statuscodes |

*Bedeutung der Felder:*

| Status | Bedeutung |
| ---- | --------- |
| email_not_correct | Allgemeiner Statuscode. Irgendwas stimmt mit der E-Mail nicht. |
| email_syntax_error | Syntax-Fehler in der E-Mail. |
| email_no_mx | Kein MX Eintrag hinter der Domain. Vermutlich kein E-Mail-Server konfiguriert. |
| email_unknown_tld | Die Top-Level-Domain ist unbekannt. |
| A1000 | Alles OK. Die eMail soll zustellbar sein und es wurden keine Tricks auf dem Server erkannt. |
| A1100 | Die eMail werden zu jeder eMail-Adresse akzeptiert. |
| A1110 | Alle email zu einer bestimmten Domain werden akzeptiert. |
| A1400 | Die eMail werden akzeptiert, es handelt sich jedoch um eine "disposable" eMail-Adresse |
| A4000 | Fehler. |
| A4100 | SMTP Fehler. |
| A4110 | Man könnte sich zwar mit SMTP verbinden, aber danach bricht die Verbindung ab. |
| A4200 | Falsche Syntax |
| A4300 | Server kennt die eMail-Adresse nicht. |
| A4400 | eMail Zustellung fehlgeschlagen. |
| A4500 | Relay Fehler. |
| A4600 | Antispam blockiert die eMail. |
| A4700 | eMail-Konto existiert nicht, ist inactiv oder kann keine eMail empfangen. |
| A4800 | Kein eMail Server auf der Domain gefunden. |
| A4810 | eMail Server ist down. |
| A4900 | Span traps erkannt. |
| A5000 | Unbekannter Fehler. |


[zur Übersicht](../readme.md)
