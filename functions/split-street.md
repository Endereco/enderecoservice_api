[zur Übersicht](../readme.md)

# spliStreet - Beschreibung

## Anfrage

```javascript
{
    "jsonrpc":"2.0",
        "id":1,
        "method":"splitStreet",
        "params":{
            "formatCountry": <string>,
            "language": <string>,
            "street": <string>
    }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.formatCountry | string | ISO 3166-1 Alpha-2 Code des Landes, z.B. "de", "ro" oder "fr" |
| params.language | string | Sprachkürzel für die Ausgabe, z.B. "de" oder "en". Dadurch kann bei internationalen Adressen andere Schreibweisen verwendet werden. Vienna oder Wien. |
| params.street | string | Volle Straße mit Hausnummer und sonstigen Zusatzinformationen. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
    "jsonrpc":"2.0",
        "id":1,
        "method":"splitStreet",
        "params":{
        "formatCountry":"de",
            "language":"de",
            "street":"Via del Tuf 7"
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
            "country": null,
            "postCode": null,
            "cityName": null,
            "street": "Via del Tuf 7",
            "streetName": "Via del Tuf",
            "houseNumber": "7",
            "additionalInfo": null,
            "status": [
                "A1000"
            ]
    }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | siehe Tabelle Statuscodes |
| result.country | status | Liste der Varianten. |
| result.street | string | Volle Straße. |
| result.streetName | string | Straßenname. |
| result.houseNumber | string | Hausnummer. |
| result.additionalInfo | string | Zusatzinfos. |

*Statuscodes:*

| Code | Bedeutung |
| ---- | --------- |
| A1000 | Straße wurde zerlegt. |

[zur Übersicht](../readme.md)
