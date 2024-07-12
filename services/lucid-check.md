# Lucid-Prüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Gültige Lucid Id](#gültige-lucid-id)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Lucid-Prüfung muss ```lucidCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld    | Erwarteter Wert | Pflicht | Bedeutung                                                                  |
|---------|-----------------|---------|----------------------------------------------------------------------------|
| lucidId | String          | Ja      | Enthält die zu prüfende Registrierungsnummer vom Verpackungsregister Lucid |

## Ausgabefelder

| Feld                              | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                                                                          |
|-----------------------------------|-----------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| predictions                       | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                                                                                     |
| predictions.lucidId               | String          | Ja      | Enthält die formatierte Registrierungsnummer vom Verpackungsregister Lucid                                                                                                                         |
| predictions.companyName           | String          | Ja      | Enthält den Firmennamen zu diesem Vorschlag.                                                                                                                                                       |
| predictions.kindOfReferenceNumber | String          | Ja      | Enthält den Typ der Referenznummer <br> vat: Umsatzsteuer-Identifikationsnummer <br> tax: Steuernummer <br> unknown: Unbekannt Firmenadresse zu diesem Vorschlag.                                  |
| predictions.referenceNumber       | String          | Ja      | Enthält die Referenznummer                                                                                                                                                                         |
| predictions.packagingDetailCode   | Int             | Ja      | Angabe, ob es Verpackungen mit Systembeteiligungspflicht sind <br>1: Verpackungen mit Systembeteiligungspflicht <br>2: Verpackungen ohne Systembeteiligungspflicht <br>3: beides <br> 4: Unbekannt |
| status                            | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes).                                                                          |

## Statuscodes

| Code          | Bedeutung                                                                 |
|---------------|---------------------------------------------------------------------------|
| lucid_correct | Diese Registrierungsnummer ist im Verpackungsregister Lucid bekannt       |
| lucid_invalid | Diese Registrierungsnummer ist im Verpackungsregister Lucid nicht bekannt |

# Beispiele

## Gültige Lucid Id

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "lucidCheck",
  "params": {
    "lucidId": "DE3151155944772"
  }
}
```

### Antwort

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [
      {
        "lucidId": "DE3151155944772",
        "companyName": "foodsbest GmbH",
        "kindOfReferenceNumber": "vat",
        "referenceNumber": "DE311849520",
        "packagingDetailCode": 1
      }
    ],
    "status": [
      "lucid_correct"
    ]
  }
}
```