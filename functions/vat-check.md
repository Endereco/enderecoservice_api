[zur Übersicht](../readme.md)

# vatCheck - Beschreibung

<span style="color:red;font-weight: bold;">Dieser Service befindet sich in Staging Phase. Schreibe an support@endereco.de, wenn du dieses Feature einsetzen willst.</span>

## Anfrage

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "vatCheck",
  "params": {
    "vat": <string>
  }  
}
```
*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.vat | string | Die Umsatzsteuer ID ohne Leerzeichen. |

## Beispiel der Anfrage

```
POST https://endereco-service.de/rpc/v1/

Content-Type: application/json
```

```javascript
{
    "jsonrpc": "2.0",
        "id": 1,
        "method": "vatCheck",
        "params": {
            "vat": "DE298503575"
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
            "status": [
                "vat_invalid"
            ]
        }
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| result.status | array | siehe Tabelle Statuscodes |
| result.status | array | Liste der Statuscodes. |
| result.companyName | string | Falls bekannt. Firmenname. |

*Statuscodes:*

Aktuelle Codes:

| Code | Bedeutung |
| ---- | --------- |
| vat_valid | Die Umsatzsteuer ID ist syntaktisch korrekt und gültig. |
| vat_invalid | Die Umsatzsteuer ID wurde in der Datenbank der gültigen Umsatzsteuer ID's nicht gefunden. |
| vat_has_company_name | Firmenname ist bekannt und wurde in der Antwort im "companyName" Parameter zurückgegeben. |
| vat_needs_correction | Die Umsatzsteuer ID ist falsch geschrieben: das Land fehlt oder ist nicht bekannt und das Format ist falsch. |
| vat_unknown_country_code | Das Land bei der Umsatzsteuer ID ist nicht bekannt.  |
| vat_wrong_format | De Umsatzsteuer ID ist falsch geschrieben. Ein oder mehrere Zeichen sind zu viel oder zu wenig. |


[zur Übersicht](../readme.md)
