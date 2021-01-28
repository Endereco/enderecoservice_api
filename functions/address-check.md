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
oder
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
    "streetfull": <string>
  }  
}
```

*Bedeutung der Felder:*

| Feld | Typ | Bedeutung |
| ---- | --- | --------- |
| jsonrpc | string | Gibt den Protokol und Version an. |
| id | int | Eindeutige Kennung des Request. Kann zum Beispiel ein Counter sein. Man brauch Id um ein Request einem Response zuordnen zu können, was bei Bulk Processing möglicherweise gebraucht werden kann. Man kann Id weglassen, wenn keine Antwort erwartet wird. |
| params.country | string | ISO 3166-1 Alpha-2 Code des Landes, z.B. "de", "ro" oder "fr" |
| params.language | string | Sprachkürzel für die Ausgabe, z.B. "de" oder "en". Dadurch kann bei internationalen Adressen andere Schreibweisen verwendet werden. Vienna oder Wien. |
| params.postCode | string | Postleitzahl. |
| params.cityName | string | Stadtname. |
| params.streetFull | string | Straße inklusive Hausnummer und eventuell auch Zusatzinformationen. |
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
  "method": "addressCheck",
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

```javascript
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "addressCheck",
  "params": {
    "country": "de",
    "language": "de",
    "postCode": "97232",
    "cityName": "Giebelstadt",
    "streetFull": "Lindenstraße 1"
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
        "predictions": [
            {
                "cityName": "Giebelstadt",
                "postCode": "97232",
                "houseNumber": "1",
                "street": "Lindenstr.",
                "country": "de"
            }
        ],
            "status": [
            "A1000",
            "address_needs_correction",
            "A1100",
            "country_code_correct",
            "postal_code_correct",
            "locality_correct",
            "street_name_needs_correction",
            "building_number_correct"
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
| result.predictions.country | string | Landescode. |

*Statuscodes:*

Legacy codes:

| Code | Bedeutung |
| ---- | --------- |
| A1000 | Adresse korrekt. |
| A1100 | Adresse korrekt, aber korrigiert. Gibt es, wenn es nur eine Variante der Adresse gibt, diese sich jedoch einwenig unterscheidet. Kommt mit A1000 zusammen.|
| A2000 | Mehrere Varianten. |
| A3000 | Keine Varianten. Die Adresse existiert nicht in unseren Datenbanken. |

Aktuelle Codes:

| Code | Bedeutung |
| ---- | --------- |
| address_correct | Die eingegebene Adresse war korrekt und brauchte keine Korrektur. |
| address_needs_correction | Die eingegebene Adresse ist nicht korrekt, im “predictions” liegt ein Korrekturvorschlag. |
| address_multiple_variants | Für die Eingabe gab es mehrere Varianten. Der Nutzer soll eine Auswählen. |
| address_not_found | Die Adresse wurde gar nicht gefunden.  |
| address_of_not_supported_type | Bei der Adressat geht es um einen Typ der von uns nicht unterstützt wird, z.B. eine Postfiliale, eine Packstation. |
| address_selected_by_customer | Die Adresse wurde von einem Nutzer manuell bestätigt. |
| country_code_needs_correction | Ländercode ist falsch und muss korrigiert werden. |
| country_code_correct | Ländercode ist korrekt. |
| postal_code_needs_correction | Postleitzahl ist falsch und muss korrigiert werden. |
| postal_code_correct | Postleitzahl ist korrekt. |
| locality_needs_correction | Ortsname ist falsch und muss korrigiert werden. |
| locality_correct | Ortsname ist korrekt. |
| street_name_needs_correction | Straßenname ist falsch und muss korrigiert werden. |
| street_name_correct | Straßenname ist korrekt. |
| street_full_needs_correction | Volle Straße (inkl Hausnummer) ist falsch und muss korrigiert werden. |
| street_full_correct | Volle Straße ist korrekt. |
| building_number_needs_correction | Hausnummer ist falsch und muss korrigiert werden. |
| building_number_correct | Hausnummer ist korrekt. |
| additional_info_needs_correction | Zusatzinfos sind falsch und müssen korrigiert werden. |
| additional_info_correct | Zusatzinfos sind korrekt. |
| address_is_packstation | Bei dem Adress handelt es sich um eine Packstation. |
| address_is_postfiliale | Bei der Adresse handelt es sich um eine Postfiliale. |
| address_is_postfach | Bei der Adresse handelt es sich um ein Postfach. |
| address_is_paketshop | Bei der Adresse handelt es sich um ein Paketshop. |

[zur Übersicht](../readme.md)
