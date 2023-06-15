# Liste aller Felder und ihre Interpretation

## Inhaltsverzeichnis

- [Grundaufbau der JSON](#grundaufbau-der-json)
- [Eingabeparameter](#eingabeparameter)
  - [Eingabeparameter addressCheck](#eingabeparameter-addresscheck)
- [Ausgabenfelder](#ausgabenfelder)
  - [Ausgabenfelder addressCheck](#ausgabenfelder-addresscheck)

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

### Eingabeparameter addressCheck

| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String | Ja | Enthält die Postleitzahl. |
| cityName | String | Ja | Enthält den Ortsnamen. |
| streetFull | String | Ja, wenn street und houseNumber nicht gesetzt ist | Enthält die Straße mit Hausnummer und ggf. Adresszusatz. |
| street | String | Ja, wenn streetFull nicht gesetzt ist | Enthält nur den Straßennamen.  |
| houseNumber | String | Ja, wenn streetFull nicht gesetzt ist | Enthält die Hausnummer. |
| additionalInfo | String | Nein | Enthält Adresszusatz. |
| subdivisionCode | String | Nein | Enthält den Bundesland/Regions code. Format: Ländercode + - + Bundeslandkürzel

### postCodeAutocomplete

| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String | Ja | Enthält die Postleitzahl. |

### cityNameAutocomplete
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| cityName | String | Ja | Enthält den Ortsnamen. |

### streetAutocomplete
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| country | String | Ja | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String | Ja | Enthält die Postleitzahl. |
| cityName | String | Ja | Enthält den Ortsnamen. |
| street | String | Ja | Enthält nur den Straßennamen.  |

### emailCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| email | String | Ja | Enthält die E-Mail Adresse. |

### nameCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| title | String | Nein | Enthält einen Titel. z.B. Dr.
| salutation | [m,f,d,x,s,o] | Ja | Enthält den Anrede-Code. Siehe [Tabelle der Anrede-Codes](#tabelle-der-anrede-codes). |
| name | String| Ja, wenn name nicht gesetzt ist | Enthält den Vornamen. Veraltet und sollte nicht mehr verwendet werden |
| firstName | String| Ja, wenn name nicht gesetzt ist | Enthält den Vornamen. |
| lastName | String | Ja, wenn name nicht gesetzt ist | Enthält den Nachnamen. |
| countryCode | String | Nein | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| language | String | Nein | Sprachkürzel für die Ausgabe. Aktuell wird nur die Ausgabe vom nameCheck beeinflusst, zukünftig wird das mit weitere Services möglich sein. Standardwert ist DE [Liste der Sprachcodes](https://de.wikipedia.org/wiki/Liste_der_ISO-639-1-Codes)|

### phoneCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| phone | String | Ja | Enthält die Rufnummer. |
| format | String | Nein | Enthält den Namen des Formats. Wird aktuell nur bei Rufnummernprüfung verwendet. Siehe [Liste der Rufnummernformate](#liste-der-rufnummernformate). |

### ibanCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| iban | String | Ja | Enthält die IBAN. |

### vatIdCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| vatId | String | Ja | Enthält die zu prüfende Umsatzsteuer ID |
| requesterVatID | String | Nein | Enthält die eigene Umsatzsteuer ID |
| companyName | String | Nein | Enthält den Firmenname der zu prüfenden Umsatzsteuer ID |
| companyPostalCode | String | Nein | Enthält die Postleitzahl der zu prüfendenn Firma |
| companyLocality | String | Nein | Enthält den Ortsnamen der zu prüfenden Firma |
| companyStreetFull | String | Nein | Enthält den Straßenname ink. Hausnummer der zu prüfenden Firma |

## Ausgabenfelder

### Ausgabenfelder addressCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| predictions | Array | Ja | Enthält JSON-Objekte mit Korrekturvorschlägen |
| predictions.country | String | Ja | Enthällt den Ländercode zu diesem Vorschlag, siehe [Liste der Ländercodes](./country-codes.md) |
| predictions.postCode | String | Ja | Enthält die Postleitzahl zu diesem Vorschlag. |
| predictions.cityName | String | Ja | Enthält den Ortsnamen zu diesem Vorschlag. |
| predictions.street | String | Ja | Enthält nur den Straßennamen zu diesem Vorschlag.  |
| predictions.houseNumber | String | Ja | Enthält die Hausnummer zu diesem Vorschlag. |
| predictions.additionalInfo | String | Nein | Enthält den Adresszusatz zu diesem Vorschlag. |
| predictions.subdivisionCode | String | Nein | Enthält den Bundesland/Regions-code zu diesem Vorschlag. Format: Ländercode + - + Bundeslandkürzel |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### postCodeAutocomplete
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| predictions | Array | Ja | Enthält JSON-Objekte mit Korrekturvorschlägen |
| predictions.postCode | String | Ja | Enthält die Postleitzahl zu diesem Vorschlag. |
| predictions.cityName | String | Ja | Enthält den Ortsnamen zu diesem Vorschlag. |
| predictions.subdivisionCode | String | Nein | Enthält den Bundesland/Regions-code zu diesem Vorschlag. Format: Ländercode + - + Bundeslandkürzel |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### cityNameAutocomplete
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| predictions | Array | Ja | Enthält JSON-Objekte mit Korrekturvorschlägen |
| predictions.postCode | String | Ja | Enthält die Postleitzahl zu diesem Vorschlag. |
| predictions.cityName | String | Ja | Enthält den Ortsnamen zu diesem Vorschlag. |
| predictions.subdivisionCode | String | Nein | Enthält den Bundesland/Regions-code zu diesem Vorschlag. Format: Ländercode + - + Bundeslandkürzel |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### streetAutocomplete
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| predictions | Array | Ja | Enthält JSON-Objekte mit Korrekturvorschlägen |
| predictions.street | String | Ja | Enthält den Straßennamen zu diesem Vorschlag. Veraltet, das Feld "streetName" sollte verwendet werden |
| predictions.streetName | String | Ja | Enthält den Straßennamen zu diesem Vorschlag. Identisch mit dem Feld "street" |
| predictions.buildingNumber | String | Ja | Enthällt die Hausnummer zu diesem Vorschlag. |
| predictions.additionalInfo | String | Ja | Enthält Adresszusatz zu diesem Vorschlag. |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### emailCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### nameCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| predictions | Array | Enthält JSON-Objekte mit Korrekturvorschlägen. |
| predictions.title | String | Nein | Enthält einen Titel zu diesem Vorschlag |
| predictions.salutation | [m,f,d,x,s,o] | Ja | Enthält den Anrede-Code zu diesem Vorschlag. Siehe [Tabelle der Anrede-Codes](#tabelle-der-anrede-codes). |
| predictions.firstName | String | Ja | Enthält den Vornamen zu diesem Vorschlag |
| predictions.lastName | String | Ja | Enthält den Nachnamen zu diesem Vorschlag |
| score | Float | Ja | Enthält eine Zahl zwischen 0.0 und 1.0. Repräsentiert die Wahrscheinlichkeit oder Qualität der Aussage. |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### phoneCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| original | Json | Ja | Identisch mit der Eingabe |
| original.phone | String | Ja | Identisch mit der Eingabe |
| predictions | Array | Nein | Enthält JSON-Objekte mit Korrekturvorschlägen. |
| predictions.phone | String | Ja, wenn predictions da sind | Enthält die Orginaleingabe zu diesem Vorschlag |
| predictions.formatNational | String | Ja, wenn predictions da sind | Enthält die Nummer im Nationalen format zu diesem Vorschlag |
| predictions.formatInternational | String | Ja, wenn predictions da sind | Enthält die Nummer im Internationelen format zu diesem Vorschlag |
| predictions.formatE164 | String | Ja, wenn predictions da sind | Enthält die Nummer im E164 format zu diesem Vorschlag [E.164](https://de.wikipedia.org/wiki/E.164) |
| predictions.formatRFC3966 | String | Ja, wenn predictions da sind | Enthält die Nummer im RFC3966 Format zu diesem Vorschlag [RFC3966](https://datatracker.ietf.org/doc/html/rfc3966) |
| predictions.region | String | Ja, wenn predictions da sind | Enthält die Region zu diesem Vorschlag |
| predictions.carrier | String | Ja, wenn predictions da sind | Enthält den Netzanbieter zu diesem Vorschlag |
| predictions.countryPrefix | String | Ja, wenn predictions da sind | Enthält die Landesvorauswahl zu diesem Vorschlag |
| predictions.nationalNumber | String | Ja, wenn predictions da sind | Enthält den nationalen Teil zu diesem Vorschlag |
| predictions.nationalPrefix | String | Ja, wenn predictions da sind | Enthält die Ortsvorwahl zu diesem Vorschlag |
| predictions.nationalBody | String | Ja, wenn predictions da sind | Enthält einen Teil der Telefonnummer ohne Landes- und Ortsvorwahl zu diesem Vorschlag |
| predictions.leadingZeros | Int | Ja, wenn predictions da sind | Enthält die Anzahl der '0' die vor der nationalen Rufnummer stehen zu diesem Vorschlag |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### ibanCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| original | Json | Ja | Identisch mit der Eingabe |
| original.iban | String | Ja | Identisch mit der Eingabe |
| predictions | Array | Nein | Enthält JSON-Objekte mit Korrekturvorschlägen. |
| predictions.iban | String | Ja, wenn predictions da sind | Enthält die formatierte iBan zu diesem Vorschlag |
| predictions.checksum | String | Ja, wenn predictions da sind | Enthält die Prüfziffer der iBan zu diesem Vorschlag |
| predictions.bankCode | String | Ja, wenn predictions da sind | Enthält eine Bankleitzahl zu diesem Vorschlag |
| predictions.bankName | String | Ja, wenn predictions da sind | Enthält den Namen der Bank zu diesem Vorschlag |
| predictions.bankPostalCode | String | Ja, wenn predictions da sind | Enthält die PLZ der Bank zu diesem Vorschlag |
| predictions.bankLocality | String | Ja, wenn predictions da sind | Enthält den Ort der Bank zu diesem Vorschlags |
| predictions.bankShortName | String | Ja, wenn predictions da sind | Enthält die verkürzte Schreibweise der Bank zu diesem Vorschlag |
| predictions.bic | String | Ja, wenn predictions da sind | Enthält die PIC zu diesem Vorschlag |
| predictions.accountNumber | String | Ja, wenn predictions da sind | Enthält die Kontonummer zu diesem Vorschlag |
| predictions.countryCentralBankName | String | Ja, wenn predictions da sind | Enthält ggf. den Namen der Zentralbank zu diesem Vorschlag |
| predictions.countryCentralBankUrl | String | Ja, wenn predictions da sind | Enthält ggf. die URL auf die Website der Zentralban zu diesem Vorschlag |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### vatIdCheck
| Feld | Erwarteter Wert | Pflicht | Bedeutung |
| --- | --- | --- | --- |
| predictions | Array | Ja | Enthält JSON-Objekte mit Korrekturvorschlägen. |
| predictions.vatId | String | Ja | Enthält die formatierte Umsatzsteuer ID zu prüfenden Firma |
| predictions.companyName | String | Nein | Enthält den Firmennamen zu diesem Vorschlag. |
| predictions.companyAddress | String | Nein | Enthält die Firmenadresse zu diesem Vorschlag. |
| cerification | Json | Nein | Enthält Informationen zu den Zertifikat |
| cerification.timestamp | Json | Nein | Enthält den Zeitpunkt der Abfrage |
| cerification.source | Json | Nein | Enthält die Datenquelle |
| status | Array | Ja | Enthält eine Liste aus Statuscodes, die das geprüfte Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

## Weitere Bedeutungen

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
