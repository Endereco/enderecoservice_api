# Beispiele für die Namensprüfung

# Info
- [Eingabefleder](./fields.md#eingabeparameter-nameCheck)
- [Ausgabefleder](./fields.md#ausgabenfelder-nameCheck)
- [Statuscodes](./statuscodes.md)

## Inhaltsverzeichnis
- [Männlicher Vorname mit richtiger Anrede](#männlicher-vorname-mit-richtiger-anrede)
- [Weiblicher Vorname mit richtige Anrede](#weiblicher-vorname-mit-richtige-anrede)
- [Männlicher Vorname mit falscher Anrede](#männlicher-vorname-mit-falscher-anrede)
- [Männlicher Vorname ohne Anrede](#männlicher-vorname-ohne-anrede)
- [Weiblicher Vorname mit falscher Anrede](#weiblicher-vorname-mit-falscher-anrede)
- [Weiblicher Vorname ohne Anrede](#weiblicher-vorname-ohne-anrede)
- [Männlicher Vorname mit diverse Anrede](#männlicher-vorname-mit-diverse-anrede)
- [Länderabhängige Anrede weiblich](#länderabhängige-anrede-weiblich)
- [Länderabhängige Anrede männlich](#länderabhängige-anrede-männlich)
- [Name wird nicht gefunden](#name-wird-nicht-gefunden)
- [Person mit Doktor beim Vornamen ohne Title](#person-mit-doktor-beim-vornamen-ohne-title)
- [Person mit Doktor beim Vornamen mit Title](#person-mit-doktor-beim-vornamen-mit-title)
- [Fake-Name](#fake-name) 
- [Pflichtfelder fehlen](#pflichtfelder-fehlen) 
- [Leere Felder](#leere-felder)
- [Falscher Inhalt](#falscher-inhalt)
- [Nicht erreichbar](#nicht-erreichbar)

### Männlicher Vorname mit richtiger Anrede

#### Anfrage

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

#### Antwort

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

### Weiblicher Vorname mit richtige Anrede

#### Anfrage
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

#### Antwort

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

### Männlicher Vorname mit falscher Anrede

#### Anfrage 

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

#### Antwort

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

### Männlicher Vorname ohne Anrede

#### Anfrage

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

#### Antwort

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

### Weiblicher Vorname mit falscher Anrede

#### Anfrage

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

#### Antwort

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

### Weiblicher Vorname ohne Anrede

#### Anfrage

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

#### Antwort

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

### Männlicher Vorname mit diverse Anrede

#### Anfrage
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

#### Antwort

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

### Länderabhängige Anrede weiblich

#### Anfrage für Deutschland

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

#### Antwort für Deutschland

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

#### Anfrage für USA

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

#### Antwort für USA

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

### Länderabhängige Anrede männlich

#### Anfrage für Deutschland

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

#### Antwort für Deutschland

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

#### Anfrage für USA

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

#### Antwort für USA

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

### Name wird nicht gefunden

#### Anfrage

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

#### Antwort

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

### Person mit Doktor beim Vornamen ohne Title

#### Anfrage

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

#### Antwort
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

### Person mit Doktor beim Vornamen mit Title

#### Anfrage

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

#### Antwort

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

### Fake-Name

#### Anfrage

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

#### Antwort

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

### Pflichtfelder fehlen

#### Anfrage alle fehlen

```
{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "nameCheck",
    "params": {}
}
```

#### Antwort alle fehlen

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.name\" or \"params.salutation\" \"params.firstName\",\"params.lastName\" must be set."
    }
}
```

#### Anfrage nur mit Vorname

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

#### Antwort nur mit Vorname

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.salutation\", \"params.firstName\",\"params.lastName\" must be set."
    }
}
```

### Leere Felder

#### Anfrage mit firstName und lastName

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

#### Antwort mit firstName und lastName

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.firstName\" or \"params.lastName\" should contain value."
    }
}
```

#### Anfrage mit name

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

#### Antwort mit name

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Field \"params.name\" should contain value."
    }
}
```

### Falscher Inhalt

#### Anfrage mit emailadresse

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

#### Antwort mit emailadresse 

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

### Nicht erreichbar

#### Antwort

```
{
    "jsonrpc": "2.0",
    "error": {
        "code": -32603,
        "message": "Could not verify person"
    }
}
```