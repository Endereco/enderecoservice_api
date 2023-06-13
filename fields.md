# Liste aller Felder und ihre Interpretation

## Inhaltsverzeichnis

- [Grundaufbau der JSON](#grundaufbau-der-json)
- [Eingabeparameter](#eingabeparameter)
- [Ausgabenfelder](#ausgabenfelder)

## Grundaufbau der JSON

Der grundsätzliche Aufbau der JSON folgt aus der [JSON-RPC 2.0 Spezifikation](https://www.jsonrpc.org/specification).

Die Spezifikation ist jedoch nicht vollständig umgesetzt. Unter anderem gibt es aktuell keine Möglichkeit mehrere
Datensätze zu übermitteln (Bulk-Processing) und die Error-Codes sind rudimentär und sollen in einer Integration nicht
verarbeitet werden.

| Feld | Erwarteter Wert | Bedeutung |
| --- | --- | --- |
| jsonrpc | "2.0" als String | Mit diesem Feld wird die Version des Protokolls angegeben. Der Wert ist immer gleich. |
| id | Zahl größer als 0 | Mit diesem Feld wird die ID der Anfrage übergeben. Falls man mehrere Anfragen asynchron versendet, kann über die ID die Sequenz wiederhergestellt werden. |
| method | String | Name [einer der Funktionen](./readme.md#verzeichnis-der-methoden-und-use-cases) der Endereco Services API, z.B. "addressCheck" |
| params | Hash-Array / Object | Eine Sammlung der methodenspezifischen Parameter |
| result | Hash-Array / Object | Container für die Antwort bei erfolgreichem Verlauf der Anfrage. |
| error | Hash-Array / Object | Container für die Fehlermeldung beim fehlerhaften Verlauf der Anfrage. |
| code | Zahl | Nummer des Fehlers. Aktuell rudimentär umgesetzt und soll ignoriert werden. |
| message | String | Fehlermeldung in menschenlesbarer Form. |

## Eingabeparameter

# addressCheck

| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String | Ja | Enthält die Postleitzahl. |
| cityName | String | Ja | Enthält den Ortsnamen. |
| streetFull | String | Ja, wenn street und houseNumber nicht gesetzt ist | Enthält die Straße mit Hausnummer und ggf. Adresszusatz. |
| street | String | Ja, wenn streetFull nicht gesetzt ist | Enthält nur den Straßennamen.  |
| houseNumber | String | Ja, wenn streetFull nicht gesetzt ist | Enthält die Hausnummer. |
| additionalInfo | String | Nein | Enthält Adresszusatz. |

# postCodeAutocomplete

| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String | Ja | Enthält die Postleitzahl. |

# cityNameAutocomplete
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| cityName | String | Ja | Enthält den Ortsnamen. |

# streetAutocomplete
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String | Ja | Enthält die Postleitzahl. |
| cityName | String | Ja | Enthält den Ortsnamen. |
| street | String | Ja | Enthält nur den Straßennamen.  |

# emailCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| email | String | Ja | Enthält die E-Mail Adresse. |

# nameCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| title | String | Nein | Enthält einen Titel. z.B. Dr.
| salutation | [m,f,d,x,s,o] | Nein | Enthält den Anrede-Code. Siehe [Tabelle der Anrede-Codes](#tabelle-der-anrede-codes). |
| name | String| Ja, wenn name nicht gesetzt ist | Enthält den Vornamen. Veraltet und sollte nicht mehr verwendet werden |
| firstName | String| Ja, wenn name nicht gesetzt ist | Enthält den Vornamen. |
| lastName | String | Ja, wenn name nicht gesetzt ist | Enthält den Nachnamen. |
| countryCode | String | Nein | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| language | String | Nein | Sprachkürzel für die Ausgabe. Aktuell wird nur die Ausgabe vom nameCheck beeinflusst, zukünftig wird das mit weitere Services möglich sein. Standardwert ist DE [Liste der Sprachcodes](https://de.wikipedia.org/wiki/Liste_der_ISO-639-1-Codes)|

# phoneCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| phone | String | Ja | Enthält die Rufnummer. |
| format | String | Nein | Enthält den Namen des Formats. Wird aktuell nur bei Rufnummernprüfung verwendet. Siehe [Liste der Rufnummernformate](#liste-der-rufnummernformate). |

# ibanCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| iban | String | Ja | Enthält die IBAN. |

# vatIdCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| vatId | String | Ja | Enthält die Umsatzsteuer ID Nummer. |
| requesterVatID | String | Nein | Enthält die eigene Umsatzstuer ID Nummer. |
| companyName | String | Nein | Enthält den Firmenname der abgefragten Umsatzsteuer ID Nummer |
| companyPostalCode | String | Nein | Enthält die Postleitzahl der abgefragten Firma |
| companyLocality | String | Nein | Enthält den Ortsnamen der abgefragten Firma |
| companyStreetFull | String | Nein | Enthält den Straßenname ink. Hausnummer der abgefragten Firma |

### Tabelle der Anrede-Codes

| Code | Bedeutung |
| --- | --- |
| m | männlich |
| f | weiblich |
| d | [divers](https://de.wikipedia.org/wiki/Divers) |
| x | unbekannt (kann sowohl männlich, als auch weiblich sein) |
| s | Familie |
| o | Organisation |

### Liste der Rufnummernformate

| Format | Bedeutung |
| --- | --- |
| E164 | Siehe, [E.164 Format](https://de.wikipedia.org/wiki/E.164) |
| INTERNATIONAL | Internationale Schreibweise der Nummer. |
| NATIONAL | Nationale Schreibweise der Nummer. |
| RFC3966 | Schreibweise der Nummer, die in *href*-Attribut verwendet werden kann (tel: Link). |

## Ausgabenfelder

| Feld | Erwarteter Wert | Bedeutung |
| --- | --- | --- |
| predictions | Array | Enthält JSON-Objekte mit Korrekturvorschlägen. |
| status | Array | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |
| original | Array | Enthält die ursprüngliche Eingabe. |
| score | Float | Enthält eine Zahl zwischen 0.0 und 1.0. Repräsentiert die Wahrscheinlichkeit oder Qualität der Aussage. |
| --Eingabefelder| mixed | Alle Felder aus ein Eingabe können in der Ausgabe vorkommen. Ihr Wert wurde ggf. berichtigt. Siehe [Eingabeparameter](#eingabeparameter). |
| streetName | String | Neuere Schreibweise für das Feld "street". Enthält das gleiche Wert wie "street". Soll zukünftig das Feld "street" ersetzen. |
| buildingNumber | String | Neuere Schreibweise für das Feld "houseNumber". Enthält das gleiche Wert wie "houseNumber". Soll zukünftig das Feld "houseNumber" ersetzen. |
| region | String | Erkannte Region bei Rufnummernprüfung. |
| carrier | String | Der ursprüngliche Netzanbieter bei Rufnummernprüfung. |
| countryPrefix | String | Landesvorauswahl bei einer Rufnummer. |
| phone | String | Enthält die Rufnummer. |
| nationalNumber | String | Der nationale Teil einer Rufnummer. |
| formatNational| String | Enthält die national formatierte Telefonnummer. |
| formatInternational | String | Enthält die international formatierte Telefonnummer. |
| formatE164 | String | Enthält die nach [E.164](https://de.wikipedia.org/wiki/E.164) formatierte Telefonnummer. |
| formatRFC3966 | String | Enthält die nach [RFC3966](https://datatracker.ietf.org/doc/html/rfc3966) formatierte Telefonnummer. |
| nationalPrefix | String | Enthält die Ortsvorwahl. |
| nationalBody | String | Enthält einen Teil der Telefonnummer ohne Landes- und Ortsvorwahl. |
| leadingZeros |  Zahl | Wie viele "0" sollen einer nationalen Rufnummer vorangestellt werden. |
| checksum | Zahl | Enthält die Prüfsumme. |
| bankCode | String | Enthält eine Bankleitzahl. |
| bankName | String | Enthält den Namen der Bank. |
| bankPostalCode | String | Enthält die PLZ der Bank. |
| bankLocality | String | Enthält den Ort der Bank. |
| bankShortName | String | Enthält die verkürzte Schreibweise der Bank. |
| bic | String | Enthält die BIC. |
| accountNumber | String | Enthält die Kontonummer. |
| countryCentralBankName | String | ggf. enthält den Namen der Zentralbank. |
| countryCentralBankUrl | String | ggf. enthält die URL auf die Website der Zentralbank. |
| companyName | String | Enthält den Firmennamen. |

