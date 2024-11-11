# Adressprüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
    - [Generelle Statuscodes](#generelle-statuscodes)
    - [Erweiterte Statuscodes](#erweiterte-statuscodes)
    - [Veraltete Statuscodes](#veraltete-statuscodes)
- [Beispiele](#beispiele)
  - [Straße und Hausnummer in getrennten Feldern](#straße-und-hausnummer-in-getrennten-feldern)
  - [Straße und Hausnummer zusammen in einem Feld](#straße-und-hausnummer-zusammen-in-einem-feld)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Adressprüfung muss ```addressCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld            | Erwarteter Wert | Pflicht                                            | Bedeutung                                                                              |
|-----------------|-----------------|----------------------------------------------------|----------------------------------------------------------------------------------------|
| country         | String          | Ja                                                 | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](../country-codes.md) |
| postCode        | String          | Ja                                                 | Enthält die Postleitzahl.                                                              |
| cityName        | String          | Ja                                                 | Enthält den Ortsnamen.                                                                 |
| streetFull      | String          | Ja, wenn street und houseNumber nicht gesetzt sind | Enthält die Straße mit Hausnummer und ggf. Adresszusatz.                               |
| street          | String          | Ja, wenn streetFull nicht gesetzt ist              | Enthält nur den Straßennamen.                                                          |
| houseNumber     | String          | Ja, wenn streetFull nicht gesetzt ist              | Enthält die Hausnummer.                                                                |
| additionalInfo  | String          | Nein                                               | Enthält den Adresszusatz.                                                              |
| subdivisionCode | String          | Nein                                               | Enthält den Bundesland-/Regionscode. Format: Ländercode + "-" + Bundeslandkürzel       |

## Ausgabefelder

| Feld                        | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|-----------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| predictions                 | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                            |
| predictions.country         | String          | Ja      | Enthält den Ländercode zu diesem Vorschlag, siehe [Liste der Ländercodes](../country-codes.md)                            |
| predictions.postCode        | String          | Ja      | Enthält die Postleitzahl zu diesem Vorschlag.                                                                             |
| predictions.cityName        | String          | Ja      | Enthält den Ortsnamen zu diesem Vorschlag.                                                                                |
| predictions.street          | String          | Ja      | Enthält nur den Straßennamen zu diesem Vorschlag.                                                                         |
| predictions.houseNumber     | String          | Ja      | Enthält die Hausnummer zu diesem Vorschlag.                                                                               |
| predictions.additionalInfo  | String          | Nein    | Enthält den Adresszusatz zu diesem Vorschlag.                                                                             |
| predictions.subdivisionCode | String          | Nein    | Enthält den Bundesland-/Regionscode zu diesem Vorschlag. Format: Ländercode + "-" + Bundeslandkürzel                      |
| status                      | Array           | Ja      | Enthält eine Liste von Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Statuscodes

### Generelle Statuscodes

| Code                              | Bedeutung                                                                                                         |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------|
| address_correct                   | Die eingegebene Adresse war korrekt und brauchte keine Korrektur.                                                 |
| address_needs_correction          | Die eingegebene Adresse ist nicht korrekt, im “predictions” liegt ein Korrekturvorschlag.                         |
| address_multiple_variants         | Für die Eingabe gab es mehrere Varianten. Der Nutzer soll eine auswählen.                                         |
| address_not_found                 | Die Adresse ist nicht bekannt.                                                                                    |
| address_of_not_supported_type     | Bei der Adresse geht es um einen Typ der von uns nicht unterstützt wird, z.B. eine Postfiliale, eine Packstation. |
| address_selected_by_customer      | Die Adresse wurde von einem Nutzer manuell bestätigt.                                                             |
| country_code_needs_correction     | Ländercode ist falsch und muss korrigiert werden.                                                                 |
| country_code_correct              | Ländercode ist korrekt.                                                                                           |
| subdivision_code_needs_correction | Bundesland-/Regionscode ist falsch und muss korrigiert werden.                                                                 |
| subdivision_code_correct          | Bundesland-/Regionscode ist korrekt.                                                                                           |
| postal_code_needs_correction      | Postleitzahl ist falsch und muss korrigiert werden.                                                               |
| postal_code_correct               | Postleitzahl ist korrekt.                                                                                         |
| locality_needs_correction         | Ortsname ist falsch und muss korrigiert werden.                                                                   |
| locality_correct                  | Ortsname ist korrekt.                                                                                             |
| street_name_needs_correction      | Straßenname ist falsch und muss korrigiert werden.                                                                |
| street_name_correct               | Straßenname ist korrekt.                                                                                          |
| street_full_needs_correction      | Volle Straße (inkl. Hausnummer) ist falsch und muss korrigiert werden.                                            |
| street_full_correct               | Volle Straße ist korrekt.                                                                                         |
| building_number_needs_correction  | Hausnummer ist falsch und muss korrigiert werden.                                                                 |
| building_number_correct           | Hausnummer ist korrekt.                                                                                           |
| building_number_is_missing        | Hausnummer wurde nicht angegeben.                                                                                 |
| building_number_not_found         | (optional) Hausnummer wurde auf der Straße und in dem PLZ-Bereich nicht gefunden.                                 |
| additional_info_needs_correction  | Zusatzinfos sind falsch und müssen korrigiert werden.                                                             |
| additional_info_correct           | Zusatzinfos sind korrekt.                                                                                         |
| address_is_packstation            | Bei der Adresse handelt es sich um eine Packstation.                                                              |
| address_is_postoffice             | Bei der Adresse handelt es sich um eine Postfiliale.                                                              |

### Erweiterte Statuscodes

Um den manuellen Aufwand bei der Bearbeitung von Adressprüfungen zu minimieren, kann zusätzlich zu Ergebnissen der
Adressprüfung eine serverseitige Klassifizierung die Änderungen bewerten.
So kann leichter entschieden werden, ob eine vorgeschlagene Korrektur automatisch übernommen werden darf oder bestätigt werden muss.

Die Statuscodes werden dabei nach einem einheitlichen Muster gebaut: [TEIL]_[minor|major]_correction

| Code                              | Bedeutung                                                                                                  |
|-----------------------------------|------------------------------------------------------------------------------------------------------------|
| address_minor_correction          | Es handelt sich um eine "kleine" Korrektur, die automatisch übernommen werden kann.                        |
| address_major_correction          | Es handelt sich um eine "kritische" Korrektur, bei der eine automatische Korrekturübernahme unmöglich ist. |
| country_code_minor_correction     | Kleine Landkorrektur. Wird aktuell nie ausgegeben.                                                         |
| country_code_major_correction     | Wichtige Landeskorrektur.                                                                                  |
| subdivision_code_minor_correction | Kleine Korrektur von Bundesland-/Regionscode.                                                              |
| subdivision_code_major_correction | Wichtige Korrektur von Bundesland-/Regionscode.                                                            |
| postal_code_minor_correction      | Kleine Korrektur der PLZ.                                                                                  |
| postal_code_major_correction      | Wichtige Korrektur der PLZ.                                                                                |
| locality_minor_correction         | Kleine Korrektur des Ortes.                                                                                |
| locality_major_correction         | Wichtige Korrektur des Ortes.                                                                              |
| street_name_minor_correction      | Kleine Korrektur der Straße.                                                                               |
| street_name_major_correction      | Wichtige Korrektur der Straße.                                                                             |
| building_number_minor_correction  | Kleine Korrektur der Hausnummer.                                                                           |
| building_number_major_correction  | Wichtige Korrektur der Hausnummer.                                                                         |

### Veraltete Statuscodes

!! **DEPRECATED** !!

Falls du noch die alten Statuscodes mit Zahlen nutzt, empfehlen wir diese mit neuen Statuscodes zu
ersetzen, da die alten Statuscodes Mitte/Ende 2022 nicht mehr unterstützt werden.

Das Mapping ist 1-zu-1, bis auf den Statuscode `A1100`. Bei ihm entspricht die Kombination `A1000` + `A1100` dem
Statuscode `address_needs_correction`. Das heißt bei einer korrekturbedürftigen Adresse kommt der
Statuscode `address_correct` nicht vor.

| Veralteter Code | Newer Code                | Bedeutung                                                                                                                                                  |
|-----------------|---------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A1000           | address_correct           | Adresse korrekt.                                                                                                                                           |
| A1100           | address_needs_correction  | Adresse korrekt, aber korrigiert. Gibt es, wenn es nur eine Variante der Adresse gibt, diese sich jedoch leicht unterscheidet. Kommt mit A1000 zusammen.   |
| A2000           | address_multiple_variants | Mehrere Varianten.                                                                                                                                         |
| A3000           | address_not_found         | Keine Varianten. Die Adresse existiert nicht in unseren Datenbanken.                                                                                       |

# Beispiele

## Straße und Hausnummer in getrennten Feldern

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "addressCheck",
  "params": {
    "country": "DE",
    "language": "de",
    "postCode": "97232",
    "cityName": "Giebelstadt",
    "street": "Lindenstraße",
    "houseNumber": "28"
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
        "cityName": "Giebelstadt",
        "postCode": "97232",
        "houseNumber": "28",
        "street": "Lindenstr.",
        "country": "de"
      }
    ],
    "status": [
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

## Straße und Hausnummer zusammen in einem Feld

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "addressCheck",
  "params": {
    "country": "DE",
    "language": "de",
    "postCode": "97232",
    "cityName": "Giebelstadt",
    "streetFull": "Lindenstraße 28"
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
        "cityName": "Giebelstadt",
        "postCode": "97232",
        "houseNumber": "28",
        "street": "Lindenstr.",
        "country": "de"
      }
    ],
    "status": [
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
