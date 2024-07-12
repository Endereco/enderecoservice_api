# iBan-Prüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
    - [Gernelle Statuscodes](#generelle-statuscodes)
    - [Erweiterte Statuscodes](#erweiterte-statuscodes)
- [Beispiele](#beispiele)
  - [Gültige iBan](#gültige-iban)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die iBan-Prüfung muss ```ibanCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld | Erwarteter Wert | Pflicht | Bedeutung         |
|------|-----------------|---------|-------------------|
| iban | String          | Ja      | Enthält die IBAN. |

## Ausgabefelder

| Feld                               | Erwarteter Wert | Pflicht                             | Bedeutung                                                                                                                 |
|------------------------------------|-----------------|-------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| original                           | JSON            | Ja                                  | Identisch mit der Eingabe                                                                                                 |
| original.iban                      | String          | Ja                                  | Identisch mit der Eingabe                                                                                                 |
| predictions                        | Array           | Nein                                | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.iban                   | String          | Ja, wenn predictions vorhanden sind | Enthält die formatierte IBAN zu diesem Vorschlag.                                                                         |
| predictions.checksum               | String          | Ja, wenn predictions vorhanden sind | Enthält die Prüfziffer der IBAN zu diesem Vorschlag.                                                                      |
| predictions.bankCode               | String          | Ja, wenn predictions vorhanden sind | Enthält eine Bankleitzahl zu diesem Vorschlag.                                                                            |
| predictions.bankName               | String          | Ja, wenn predictions vorhanden sind | Enthält den Namen der Bank zu diesem Vorschlag.                                                                           |
| predictions.bankPostalCode         | String          | Ja, wenn predictions vorhanden sind | Enthält die Postleitzahl der Bank zu diesem Vorschlag.                                                                    |
| predictions.bankLocality           | String          | Ja, wenn predictions vorhanden sind | Enthält den Ort der Bank zu diesem Vorschlag.                                                                             |
| predictions.bankShortName          | String          | Ja, wenn predictions vorhanden sind | Enthält die verkürzte Schreibweise der Bank zu diesem Vorschlag.                                                          |
| predictions.bic                    | String          | Ja, wenn predictions vorhanden sind | Enthält den BIC zu diesem Vorschlag.                                                                                      |
| predictions.accountNumber          | String          | Ja, wenn predictions vorhanden sind | Enthält die Kontonummer zu diesem Vorschlag.                                                                              |
| predictions.countryCentralBankName | String          | Ja, wenn predictions vorhanden sind | Enthält ggf. den Namen der Zentralbank zu diesem Vorschlag.                                                               |
| predictions.countryCentralBankUrl  | String          | Ja, wenn predictions vorhanden sind | Enthält ggf. die URL zur Website der Zentralbank zu diesem Vorschlag.                                                     |
| status                             | Array           | Ja                                  | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Statuscodes

### Generelle Statuscodes

| Code                  | Bedeutung                                     |
|-----------------------|-----------------------------------------------|
| iban_correct          | Die übermittelte IBAN ist korrekt.            |
| iban_needs_correction | Die übermittelte IBAN soll korrigiert werden. |
| iban_invalid          | Die übermittelte IBAN ist falsch.             |
| iban_checksum_correct | Die Prüfsumme der IBAN ist richtig.           |
| iban_checksum_invalid | Die Prüfsumme der IBAN ist falsch.            |
| iban_length_correct   | Die Länge der IBAN ist richtig.               |
| iban_length_invalid   | Die Länge der IBAN ist falsch.                |
| iban_is_sepa          | Die IBAN ist SEPA fähig.                      |

### Erweiterte Statuscodes

| Code                       | Bedeutung                                                                                                                  |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------|
| iban_account_correct       | Die Kontonummer ist korrekt.                                                                                               |
| iban_account_invalid       | Die Kontonummer ist falsch.                                                                                                |
| iban_account_checkmethod_X | Die Kontonummer wurde mit der Prüfziffermethode X geprüft. Anstatt von X steht die Nummer der Prüfziffermethode, z.B. "10" |

# Beispiele

## Gültige iBan

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "ibanCheck",
  "params": {
    "iban": "DE63790500000044649465"
  }
}
```

### Antwort

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "original": {
      "iban": "DE63790500000044649465"
    },
    "status": [
      "iban_correct",
      "iban_checksum_correct",
      "iban_length_correct",
      "iban_is_sepa"
    ],
    "predictions": [
      {
        "iban": "DE63790500000044649465",
        "countryCode": "DE",
        "checksum": "63",
        "bankCode": "79050000",
        "bankName": "Sparkasse Mainfranken Würzburg",
        "bankPostalCode": "97067",
        "bankLocality": "Würzburg",
        "bankShortName": "Spk Mainfranken Würzburg",
        "bic": "BYLADEM1SWU",
        "accountNumber": "0044649465",
        "countryCentralBankName": "Deutsche Bundesbank",
        "countryCentralBankUrl": "http://www.bundesbank.de/"
      }
    ]
  }
}
```