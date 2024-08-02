# Namensprüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
    - [Gernelle Statuscodes](#generelle-statuscodes)
    - [Erweiterte Statuscodes](#erweiterte-statuscodes)
    - [Anredecodes](#anrede-codes)
- [Beispiele](#beispiele)
  - [Männlicher Vorname mit richtiger Anrede](#männlicher-vorname-mit-richtiger-anrede)
  - [Weiblicher Vorname mit richtige Anrede](#weiblicher-vorname-mit-richtige-anrede)
  - [Männlicher Vorname mit falscher Anrede](#männlicher-vorname-mit-falscher-anrede)
  - [Männlicher Vorname ohne Anrede](#männlicher-vorname-ohne-anrede)
  - [Weiblicher Vorname mit falscher Anrede](#weiblicher-vorname-mit-falscher-anrede)
  - [Weiblicher Vorname ohne Anrede](#weiblicher-vorname-ohne-anrede)
  - [Männlicher Vorname mit diverse Anrede](#männlicher-vorname-mit-diverse-anrede)
  - [Länderabhängige Anrede (weiblich)](#länderabhängige-anrede-weiblich)
  - [Länderabhängige Anrede (männlich)](#länderabhängige-anrede-männlich)
  - [Name wird nicht gefunden](#name-wird-nicht-gefunden)
  - [Person mit Doktor beim Vornamen ohne Title](#person-mit-doktor-beim-vornamen-ohne-title)
  - [Person mit Doktor beim Vornamen mit Title](#person-mit-doktor-beim-vornamen-mit-title)
  - [Fake-Name](#fake-name)
  - [Pflichtfelder fehlen](#pflichtfelder-fehlen)
  - [Leere Felder](#leere-felder)
  - [Falscher Inhalt](#falscher-inhalt)
  - [Nicht erreichbar](#nicht-erreichbar)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Namens-Prüfung muss ```nameCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld        | Erwarteter Wert | Pflicht                              | Bedeutung                                                                                                                                                                                                                                           |
|-------------|-----------------|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| title       | String          | Nein                                 | Enthält einen Titel, z.B. Dr.                                                                                                                                                                                                                       |
| salutation  | [m,f,d,x,s,o]   | Ja                                   | Enthält den Anrede-Code. Siehe [Tabelle der Anrede-Codes](#anrede-Codes).                                                                                                                                                                           |
| name        | String          | Ja, wenn firstName nicht gesetzt ist | Enthält den Nachnamen. Veraltet und sollte nicht mehr verwendet werden.                                                                                                                                                                             |
| firstName   | String          | Ja, wenn name nicht gesetzt ist      | Enthält den Vornamen.                                                                                                                                                                                                                               |
| lastName    | String          | Ja, wenn name nicht gesetzt ist      | Enthält den Nachnamen.                                                                                                                                                                                                                              |
| countryCode | String          | Nein                                 | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./../country-codes.md)                                                                                                                                                            |
| language    | String          | Nein                                 | Sprachkürzel für die Ausgabe. Aktuell wird nur die Ausgabe des nameChecks beeinflusst, zukünftig wird das bei weiteren Services möglich sein. Standardwert ist DE. [Liste der Sprachcodes](https://de.wikipedia.org/wiki/Liste_der_ISO-639-1-Codes) |


## Ausgabefelder

| Feld                   | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions            | Array           | Nein    | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.title      | String          | Nein    | Enthält einen Titel zu diesem Vorschlag.                                                                                  |
| predictions.salutation | [m,f,d,x,s,o]   | Ja      | Enthält den Anrede-Code zu diesem Vorschlag. Siehe [Tabelle der Anrede-Codes](#anrede-Codes).                             |
| predictions.firstName  | String          | Ja      | Enthält den Vornamen zu diesem Vorschlag.                                                                                 |
| predictions.lastName   | String          | Ja      | Enthält den Nachnamen zu diesem Vorschlag.                                                                                |
| score                  | Float           | Ja      | Enthält eine Zahl zwischen 0.0 und 1.0. Repräsentiert die Wahrscheinlichkeit oder Qualität der Aussage.                   |
| status                 | Array           | Ja      | Enthält eine Liste mit Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Anrede-Codes

| Code | Bedeutung                                               |
|------|---------------------------------------------------------|
| m    | männlich                                                |
| f    | weiblich                                                |
| d    | [divers](https://de.wikipedia.org/wiki/Divers)          |
| x    | unbekannt (kann sowohl männlich als auch weiblich sein) |
| s    | Familie                                                 |
| o    | Organisation                                            |

## Statuscodes

### Generelle Statuscodes

| Code                        | Bedeutung                                                                                                                 |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------|
| name_is_natural_person      | Bei dem übermittelten Namen handelt es sich um eine natürliche Person. z.B. "Mario Fischer"                               |
| name_is_family              | Bei dem Namen handelt es sich um eine Familie, z.B. "Familie Müller"                                                      |
| name_is_organisation        | Bei dem Namen handelt es sich um eine Organisation, z.B. "endereco UG"                                                    |
| name_is_group               | Bei dem Namen handelt es sich um eine Gruppe.                                                                             |
| name_multiple_variants      | Bei dem Namen wurden mehrere Personen entdeckt, z.B. "Maria und Fritz Fleischmann"                                        |
| name_correct                | Der Name ist bekannt und korrekt geschrieben.                                                                             |
| name_needs_correction       | Die bekannte Schreibweise des Namens unterscheidet sich von der übermittelten.                                            |
| salutation_needs_correction | Anrede soll korrigiert werden.                                                                                            |
| first_name_needs_correction | Vorname soll korrigiert werden.                                                                                           | 
| last_name_needs_correction  | Nachname soll korrigiert werden.                                                                                          |
| name_transpositioned        | Vorname und Nachname wurden vertauscht und stehen in falschen Feldern.                                                    |
| name_has_duplicates         | Teile des Namens sind in mehreren Feldern gleichzeitig anwesend, z.B. "Ilja Weber" im Vornamenfeld, "Weber" Nachnamenfeld |
| name_not_found              | Die Schreibweise des Namens ist unbekannt.                                                                                |

### Erweiterte Statuscodes

| Code                   | Bedeutung                                                           |
|------------------------|---------------------------------------------------------------------|
| name_is_real           | Der Name ist 100% echt.                                             |
| name_is_probably_real  | Der Name ist mit Wahrscheinlichkeit von 75% oder mehr echt.         |
| name_is_probably_fake  | Der Name ist mit Wahrscheinlichkeit von 50% oder mehr echt.         |
| name_is_fake           | Der Name ist definitiv fake.                                        |
| name_has_placeholder   | Der Name ist ein Platzhalter, z.B. "Max Musterman"                  |
| name_has_random_typing | Der Name ist eine Zufallskette an Zeichen, z.B. "Asdasdasd fAsfasf" |

# Beispiele

## Männlicher Vorname mit richtiger Anrede

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "m",
        "firstName": "Max",
        "lastName": "Dreßler"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "salutation": "m",
                "firstName": "Max",
                "lastName": "Dreßler"
            }
        ],
        "status": [
            "name_is_natural_person",
            "name_correct"
        ]
    }
}
```

## Weiblicher Vorname mit richtige Anrede

### Anfrage
```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "f",
        "firstName": "Sophie",
        "lastName": "Dreßler"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "salutation": "f",
                "firstName": "Sophie",
                "lastName": "Dreßler"
            }
        ],
        "status": [
            "name_is_natural_person",
            "name_correct"
        ]
    }
}
```

## Männlicher Vorname mit falscher Anrede

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "f",
        "firstName": "Max",
        "lastName": "Dreßler"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "title": "",
                "salutation": "m",
                "firstName": "Max",
                "lastName": "Dreßler"
            }
        ],
        "status": [
            "name_is_natural_person",
            "salutation_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

## Männlicher Vorname ohne Anrede

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "",
        "firstName": "Max",
        "lastName": "Dreßler"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "salutation": "m",
                "firstName": "Max",
                "lastName": "Dreßler"
            }
        ],
        "status": [
            "name_is_natural_person",
            "salutation_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

## Weiblicher Vorname mit falscher Anrede

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "m",
        "firstName": "Sophie",
        "lastName": "Dreßler"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "salutation": "f",
                "firstName": "Sophie",
                "lastName": "Dreßler"
            }
        ],
        "status": [
            "name_is_natural_person",
            "salutation_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

## Weiblicher Vorname ohne Anrede

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "",
        "firstName": "Sophie",
        "lastName": "Dreßler"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "salutation": "f",
                "firstName": "Sophie",
                "lastName": "Dreßler"
            }
        ],
        "status": [
            "name_is_natural_person",
            "salutation_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

## Männlicher Vorname mit diverse Anrede

### Anfrage
```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "d",
        "firstName": "Max",
        "lastName": "Bauer"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "salutation": "d",
                "firstName": "Max",
                "lastName": "Bauer"
            }
        ],
        "status": [
            "name_is_natural_person",
            "name_correct"
        ]
    }
}
```

## Länderabhängige Anrede (weiblich)

### Anfrage für Deutschland

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "countryCode": "DE",
        "language": "de",
        "salutation": "",
        "firstName": "Miro",
        "lastName": "Bauer"
    }
}
```

### Antwort für Deutschland

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "title": "",
                "salutation": "m",
                "firstName": "Miro",
                "lastName": "Bauer"
            }
        ],
        "status": [
            "name_is_natural_person",
            "salutation_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

### Anfrage für USA

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "countryCode": "US",
        "language": "en",
        "salutation": "",
        "firstName": "Miro",
        "lastName": "Bauer"
    }
}
```

### Antwort für USA

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "title": "",
                "salutation": "f",
                "firstName": "Miro",
                "lastName": "Bauer"
            }
        ],
        "status": [
            "name_is_natural_person",
            "salutation_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

## Länderabhängige Anrede (männlich)

### Anfrage für Deutschland

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "countryCode": "DE",
        "language": "de",
        "salutation": "m",
        "firstName": "Miro",
        "lastName": "Bauer"
    }
}
```

### Antwort für Deutschland

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "title": "",
                "salutation": "m",
                "firstName": "Miro",
                "lastName": "Bauer"
            }
        ],
        "status": [
            "name_is_natural_person",
            "name_correct"
        ]
    }
}
```

### Anfrage für USA

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "countryCode": "US",
        "language": "en",
        "salutation": "m",
        "firstName": "Miro",
        "lastName": "Bauer"
    }
}
```

### Antwort für USA

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "title": "",
                "salutation": "f",
                "firstName": "Miro",
                "lastName": "Bauer"
            }
        ],
        "status": [
            "name_is_natural_person",
            "salutation_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

## Name wird nicht gefunden

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "m",
        "firstName": "d",
        "lastName" : ""
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [],
        "status": [
            "name_not_found"
        ]
    }
}
```

## Person mit Doktor beim Vornamen ohne Title

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "m",
        "firstName": "Doktor Markus",
        "lastName" : "Schmitt"
    }
}
```

### Antwort
```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "salutation": "m",
                "firstName": "Doktor Markus",
                "lastName": "Schmitt"
            }
        ],
        "status": [
            "name_is_natural_person",
            "name_correct"
        ]
    }
}
```

## Person mit Doktor beim Vornamen mit Title

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "title": "",
        "salutation": "m",
        "firstName": "Doktor Markus",
        "lastName" : "Schmitt"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [
            {
                "title": "Doktor",
                "salutation": "m",
                "firstName": "Markus",
                "lastName": "Schmitt"
            }
        ],
        "status": [
            "name_is_natural_person",
            "first_name_needs_correction",
            "title_needs_correction",
            "name_needs_correction"
        ]
    }
}
```

## Fake-Name

### Anfrage

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "",
        "firstName": "Fake",
        "lastName" : "Fake"
    }
}
```

### Antwort

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [],
        "status": [
            "name_not_found"
        ]
    }
}
```

## Pflichtfelder fehlen

### Anfrage alle fehlen

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {}
}
```

### Antwort alle fehlen

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.name\" or \"params.salutation\" \"params.firstName\",\"params.lastName\" must be set."
    }
}
```

### Anfrage nur mit Vorname

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "firstName": "Florian"
    }
}
```

### Antwort nur mit Vorname

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.salutation\", \"params.firstName\",\"params.lastName\" must be set."
    }
}
```

## Leere Felder

### Anfrage mit firstName und lastName

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "m",
        "firstName": "",
        "lastName": ""
    }
}
```

### Antwort mit firstName und lastName

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.firstName\" or \"params.lastName\" should contain value."
    }
}
```

### Anfrage mit name

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "name": ""
    }
}
```

### Antwort mit name

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.name\" should contain value."
    }
}
```

## Falscher Inhalt

### Anfrage mit emailadresse

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {
        "salutation": "",
        "firstName": "florian@endereco.de",
        "lastName" : "Dreßler"
    }
}
```

### Antwort mit emailadresse

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "predictions": [],
        "status": [
            "name_not_found"
        ]
    }
}
```

## Nicht erreichbar

### Antwort

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Could not verify person"
    }
}
```
