[zur Übersicht](../readme.md)

# startSession - Beschreibung

## Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "startSession",
  "params": {
    "sessionType": <string>
  }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt das Protokoll und seine Version an. |

*Session Types*

Der Typ `default` soll niemals manuell gesetzt werden.

| Typ | Bedeutung |
| --- | --------- |
| default | Wenn kein Session-Typ gesetzt ist, wird der Typ `default` angenommen. Über einen Cronjob wird später der richtige Typ automatisch ermittelt. Dieser Typ soll nicht gesetzt werden. |
| inputassistant | Session-Typ, der nur Eingabeassistent-Anfragen enthält. |
| addresscheck | Session-Typ, der nur Addressprüfungen enthält. |
| emailservices | Session-Typ, der nur E-Mail-Services Anfragen enthält. |
| phoneservices | Session-Typ, der nur Telefon-Services Anfragen enthält. |
| ams | Enthält sowohl Adressprüfungen, als auch die Eingabeassistent-Anfragen. |
| nameservices | Session Typ der nur Personen-Services enthält. (zZ nur Anrede-Prüfung) |


## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
X-Transaction-Id: not_required
```

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "startSession",
  "params": {
    "sessionType": "inputassistant"
  }
}
```

## Beispiel der Antwort (Erfolg)

```
200 Ok
```

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "newTid": "cf66276e222f97886d272cb0as9ec2f3916251976c4d49cdef4f66083f464abe51011a109f06aa3b63fa34e45e965841bc62b12e25f65dac45854e36ccbafccd",
        "status": [
            "A1000"
        ]
    }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.newTid | string | Das ist die ID der Session, die bei allen folgenden Anfragen des gewählten Session-Typs in `X-Transaction-Id` Header angegeben werden muss. |

## Beispiel der Antwort (Fehler)

Die Funktion ist für den aktuellen API-Key nicht freigegeben. Wenn dieser Fehler erscheint, wende dich an unseren Support: info@endereco.de.

```
400 Bad Request
```

```json
{
    "jsonrpc": "2.0",
    "id": 1,
    "error": {
        "code": -32700,
        "message": "No such method allowed for this user."
    }
}
```

[zur Übersicht](../readme.md)
