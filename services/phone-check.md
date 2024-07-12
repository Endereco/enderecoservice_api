# Telefonnummern-Prüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Rufnummernformate](#rufnummernformate)
  - [Statuscodes](#statuscodes)
- [Beispiele](#beispiele)
  - [Festnetz](#festnetz)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die Telefonnummern-Prüfung muss ```phoneCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld   | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                     |
|--------|-----------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| phone  | String          | Ja      | Enthält die Rufnummer.                                                                                                                        |
| format | String          | Nein    | Enthält den Namen des Formats. Wird aktuell nur bei der Rufnummernprüfung verwendet. Siehe [Liste der Rufnummernformate](#rufnummernformate). |

## Ausgabefelder

| Feld                            | Erwarteter Wert | Pflicht                             | Bedeutung                                                                                                                   |
|---------------------------------|-----------------|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| original                        | JSON            | Ja                                  | Identisch mit der Eingabe                                                                                                   |
| original.phone                  | String          | Ja                                  | Identisch mit der Eingabe                                                                                                   |
| predictions                     | Array           | Nein                                | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                              |
| predictions.phone               | String          | Ja, wenn predictions vorhanden sind | Enthält die Original-Eingabe zu diesem Vorschlag.                                                                           |
| predictions.formatNational      | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im nationalen Format zu diesem Vorschlag.                                                                |
| predictions.formatInternational | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im internationalen Format zu diesem Vorschlag.                                                           |
| predictions.formatE164          | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im E164-Format zu diesem Vorschlag. [E.164](https://de.wikipedia.org/wiki/E.164)                         |
| predictions.formatRFC3966       | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im RFC3966-Format zu diesem Vorschlag. [RFC3966](https://datatracker.ietf.org/doc/html/rfc3966)          |
| predictions.region              | String          | Ja, wenn predictions vorhanden sind | Enthält die Region zu diesem Vorschlag.                                                                                     |
| predictions.carrier             | String          | Ja, wenn predictions vorhanden sind | Enthält den Netzanbieter zu diesem Vorschlag.                                                                               |
| predictions.countryPrefix       | String          | Ja, wenn predictions vorhanden sind | Enthält die Landesvorwahl zu diesem Vorschlag.                                                                              |
| predictions.nationalNumber      | String          | Ja, wenn predictions vorhanden sind | Enthält den nationalen Teil zu diesem Vorschlag.                                                                            |
| predictions.nationalPrefix      | String          | Ja, wenn predictions vorhanden sind | Enthält die Ortsvorwahl zu diesem Vorschlag.                                                                                |
| predictions.nationalBody        | String          | Ja, wenn predictions vorhanden sind | Enthält einen Teil der Telefonnummer ohne Landes- und Ortsvorwahl zu diesem Vorschlag.                                      |
| predictions.leadingZeros        | Int             | Ja, wenn predictions vorhanden sind | Enthält die Anzahl der führenden '0' in der nationalen Rufnummer zu diesem Vorschlag.                                       |
| status                          | Array           | Ja                                  | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes).   |

## Rufnummernformate

| Format        | Bedeutung                                                                         |
|---------------|-----------------------------------------------------------------------------------|
| E164          | Siehe [E.164-Format](https://de.wikipedia.org/wiki/E.164)                         |
| INTERNATIONAL | Internationale Schreibweise der Nummer                                            |
| NATIONAL      | Nationale Schreibweise der Nummer                                                 |
| RFC3966       | Schreibweise der Nummer, die im *href*-Attribut verwendet werden kann (tel: Link) |

## Statuscodes

| Code                          | Bedeutung                                                                                                                                                                                                                                    |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| phone_correct                 | Die Telefonnummer ist korrekt und richtig formatiert.                                                                                                                                                                                        |
| phone_invalid                 | Die Telefonnummer ist falsch geschrieben. Es gibt keine Korrekturvorschläge.                                                                                                                                                                 |
| phone_needs_correction        | Die Telefonnummer ist nicht ganz korrekt, kann aber korrigiert werden.                                                                                                                                                                       |
| phone_format_needs_correction | Die Telefonnummer ist falsch formatiert. Voraussetzung ist, dass ein Format in den Eingabeparametern spezifiziert wurde.                                                                                                                     |
| phone_format_e164             | Die übermittelte Telefonnummer ist im [Format E.164](https://www.bundesnetzagentur.de/SharedDocs/Downloads/DE/Sachgebiete/Telekommunikation/Unternehmen_Institutionen/Nummerierung/Rufnummern/NP_Nummernraum.pdf?__blob=publicationFile&v=3) |
| phone_format_international    | Die übermittelte Telefonnummer ist international formatiert.                                                                                                                                                                                 |
| phone_format_national         | Die übermittelte Telefonnummer ist national formatiert.                                                                                                                                                                                      |
| phone_format_rfc3966          | Die übermittelte Telefonnummer ist ein tel: Link.                                                                                                                                                                                            |
| phone_format_unknown          | Das Format konnte nicht erkannt werden.                                                                                                                                                                                                      |
| phone_is_mobile               | Die Telefonnummer ist eine Mobilfunknetznummer.                                                                                                                                                                                              |
| phone_is_fixed_line           | Die Telefonnummer ist eine Festnetznummer.                                                                                                                                                                                                   |
| phone_is_unknown_type         | Der Typ der Telefonnummer konnte nicht eindeutig erkannt werden.                                                                                                                                                                             |
| phone_hlr_lookup_succeeded    | Die Telefonnummer wurde über HLR-Lookup geprüft und ist aktiv.                                                                                                                                                                               |
| phone_hlr_lookup_failed       | Die Telefonnummer wurde über HLR-Lookup geprüft und ist inaktiv.                                                                                                                                                                             |
| phone_hlr_lookup_not_possible | HLR-Lookup ist für die Telefonnummer nicht möglich (z.B. weil es eine Festnetznummer ist).                                                                                                                                                   |

# Beispiele

## Festnetz

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "phoneCheck",
  "params": {
    "phone": "093166398390",
    "countryCode": "DE",
    "format": "E164"
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
      "phone": "093166398390"
    },
    "status": [
      "phone_format_unknown",
      "phone_is_fixed_line",
      "phone_needs_correction",
      "phone_format_needs_correction"
    ],
    "predictions": [
      {
        "phone": "+4993166398390",
        "formatNational": "0931 66398390",
        "formatInternational": "+49 931 66398390",
        "formatE164": "+4993166398390",
        "formatRFC3966": "tel:+49-931-66398390",
        "region": "Würzburg",
        "carrier": "",
        "countryPrefix": "+49",
        "nationalNumber": "93166398390",
        "nationalPrefix": "0931",
        "nationalBody": "66398390",
        "leadingZeros": 1
      }
    ]
  }
}
```