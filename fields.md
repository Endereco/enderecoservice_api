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

| Feld | Erwarteter Wert | Bedeutung |
| --- | --- | --- |
| country | String | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| language | String | Sprachkürzel für die Ausgabe. Spielt aktuell keine Rolle, zukünftig kann damit die Sprache der Ausgabe beeinflusst werden. |
| postCode | String | Enthält die Postleitzahl. |
| cityName | String | Enthält den Ortsnamen. |
| streetFull | String | Enthält die Straße mit Hausnummer und ggf. Adresszusatz. |
| street | String | Enthält nur den Straßennamen.  |
| houseNumber | String | Enthält die Hausnummer. |
| additionalInfo | String | Enthält Adresszusatz. |
| email | String | Enthält die E-Mail Adresse. |
| salutation | [m,f,d,x,s,o] | Enthält den Anrede-Code. Siehe [Tabelle der Anrede-Codes](#tabelle-der-anrede-codes) |
| firstName | String| Enthält den Vornamen. |
| lastName | String | Enthält den Nachnamen. |
| phone | String | Enthält die Rufnummer. |
| format | String | Enthält den Namen des Format. Wird aktuell nur bei Rufnummernprüfung verwendet. Siehe [Liste der Rufnummernformate](#liste-der-rufnummernformate). |
| iban | String | Enthält die IBAN. |
| vatId | String | Enthält die Umsatzsteuer ID Nummer. |
| sessionId | String | Enthält die Session ID. Siehe [Doku für Sessions](./sessions-guideline.md). |

### Tabelle der Anrede-Codes

| Code | Bedeutung |
| --- | --- |
| m | männlich |
| f | weiblich |
| d | divers |
| x | unbekannt |
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
| nationalNumber | String | Der nationale Teil einer Rufnummer. |
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

