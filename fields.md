# Liste aller Felder und ihre Interpretation

## Inhaltsverzeichnis

- [Grundstruktur der JSON](#grundstruktur-der-json)
- [addressCheck](#addresscheck)  
- [postCodeAutocomplete](#postCodeAutocomplete)
- [cityNameAutocomplete](#cityNameAutocomplete)
- [streetAutocomplete](#streetAutocomplete)
- [emailCheck](#emailCheck)    
- [nameCheck](#nameCheck)
- [phoneCheck](#phoneCheck)    
- [ibanCheck](#ibanCheck)    
- [vatIdCheck](#vatIdCheck)
  - [Einfache Prüfung](#vatidcheck-einfache-prüfung)
  - [Qualifizierte Prüfung](#vatidcheck-qualifizierte-prüfung)
- [lucidCheck](#lucidCheck)
- [Firmensuche](#companyAutocomplete)

## Grundstruktur der JSON

Die Grundstruktur der JSON folgt der [JSON-RPC 2.0 Spezifikation](https://www.jsonrpc.org/specification).

Jedoch ist die Spezifikation nicht vollständig umgesetzt. Unter anderem gibt es aktuell keine Möglichkeit, mehrere
Datensätze (Bulk-Processing) zu übermitteln, und die Fehlercodes sind rudimentär und sollten in einer Integration nicht
verarbeitet werden.

| Feld    | Erwarteter Wert     | Bedeutung                                                                                                                                                    |
|---------|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| jsonrpc | "2.0" als String    | Mit diesem Feld wird die Version des Protokolls angegeben. Der Wert ist immer gleich.                                                                        |
| id      | Zahl größer als 0   | Mit diesem Feld wird die ID der Anfrage übergeben. Falls mehrere Anfragen asynchron versendet werden, kann über die ID die Sequenz wiederhergestellt werden. |
| method  | String              | Name [einer der Funktionen](./readme.md#verzeichnis-der-methoden-und-use-cases) der Endereco Services API, z.B. "addressCheck"                               |
| params  | Hash-Array / Object | Eine Sammlung der methodenspezifischen Parameter                                                                                                             |
| result  | Hash-Array / Object | Container für die Antwort bei erfolgreichem Verlauf der Anfrage.                                                                                             |
| error   | Hash-Array / Object | Container für die Fehlermeldung beim fehlerhaften Verlauf der Anfrage.                                                                                       |
| code    | Zahl                | Nummer des Fehlers. Aktuell rudimentär umgesetzt und soll ignoriert werden.                                                                                  |
| message | String              | Fehlermeldung in menschenlesbarer Form. |

## AddressCheck

### Eingabeparameter

| Feld            | Erwarteter Wert | Pflicht                                            | Bedeutung                                                                             |
|-----------------|-----------------|----------------------------------------------------|---------------------------------------------------------------------------------------|
| country         | String          | Ja                                                 | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode        | String          | Ja                                                 | Enthält die Postleitzahl.                                                             |
| cityName        | String          | Ja                                                 | Enthält den Ortsnamen.                                                                |
| streetFull      | String          | Ja, wenn street und houseNumber nicht gesetzt sind | Enthält die Straße mit Hausnummer und ggf. Adresszusatz.                              |
| street          | String          | Ja, wenn streetFull nicht gesetzt ist              | Enthält nur den Straßennamen.                                                         |
| houseNumber     | String          | Ja, wenn streetFull nicht gesetzt ist              | Enthält die Hausnummer.                                                               |
| additionalInfo  | String          | Nein                                               | Enthält Adresszusatz.                                                                 |
| subdivisionCode | String          | Nein                                               | Enthält den Bundesland-/Regionscode. Format: Ländercode + "-" + Bundeslandkürzel      |

### Ausgabefelder

| Feld                        | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                     |
|-----------------------------|-----------------|---------|-------------------------------------------------------------------------------------------------------------------------------|
| predictions                 | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                |
| predictions.country         | String          | Ja      | Enthält den Ländercode zu diesem Vorschlag, siehe [Liste der Ländercodes](./country-codes.md)                                 |
| predictions.postCode        | String          | Ja      | Enthält die Postleitzahl zu diesem Vorschlag.                                                                                 |
| predictions.cityName        | String          | Ja      | Enthält den Ortsnamen zu diesem Vorschlag.                                                                                    |
| predictions.street          | String          | Ja      | Enthält nur den Straßennamen zu diesem Vorschlag.                                                                             |
| predictions.houseNumber     | String          | Ja      | Enthält die Hausnummer zu diesem Vorschlag.                                                                                   |
| predictions.additionalInfo  | String          | Nein    | Enthält den Adresszusatz zu diesem Vorschlag.                                                                                 |
| predictions.subdivisionCode | String          | Nein    | Enthält den Bundesland-/Regionscode zu diesem Vorschlag. Format: Ländercode + "-" + Bundeslandkürzel                          |
| status                      | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

## postCodeAutocomplete

### Eingabeparameter

| Feld     | Erwarteter Wert | Pflicht | Bedeutung                                                                             |
|----------|-----------------|---------|---------------------------------------------------------------------------------------|
| country  | String          | Ja      | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String          | Ja      | Enthält die Postleitzahl.                                                             |

### Ausgabefelder

| Feld                        | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                     |
|-----------------------------|-----------------|---------|-------------------------------------------------------------------------------------------------------------------------------|
| predictions                 | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                |
| predictions.postCode        | String          | Ja      | Enthält die Postleitzahl zu diesem Vorschlag.                                                                                 |
| predictions.cityName        | String          | Ja      | Enthält den Ortsnamen zu diesem Vorschlag.                                                                                    |
| predictions.subdivisionCode | String          | Nein    | Enthält den Bundesland-/Regionscode zu diesem Vorschlag. Format: Ländercode + "-" + Bundeslandkürzel                          |
| status                      | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

## cityNameAutocomplete

### Eingabeparameter

| Feld     | Erwarteter Wert | Pflicht | Bedeutung                                                                             |
|----------|-----------------|---------|---------------------------------------------------------------------------------------|
| country  | String          | Ja      | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| cityName | String          | Ja      | Enthält den Ortsnamen.                                                                |

### Ausgabefelder

| Feld                        | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                     |
|-----------------------------|-----------------|---------|-------------------------------------------------------------------------------------------------------------------------------|
| predictions                 | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                |
| predictions.postCode        | String          | Ja      | Enthält die Postleitzahl zu diesem Vorschlag.                                                                                 |
| predictions.cityName        | String          | Ja      | Enthält den Ortsnamen zu diesem Vorschlag.                                                                                    |
| predictions.subdivisionCode | String          | Nein    | Enthält den Bundesland-/Regionscode zu diesem Vorschlag. Format: Ländercode + "-" + Bundeslandkürzel                          |
| status                      | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

## streetAutocomplete

### Eingabeparameter

| Feld     | Erwarteter Wert | Pflicht | Bedeutung                                                                             |
|----------|-----------------|---------|---------------------------------------------------------------------------------------|
| country  | String          | Ja      | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| postCode | String          | Ja      | Enthält die Postleitzahl.                                                             |
| cityName | String          | Ja      | Enthält den Ortsnamen.                                                                |
| street   | String          | Ja      | Enthält nur den Straßennamen.                                                         |

### Ausgabefelder

| Feld                       | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                     |
|----------------------------|-----------------|---------|-------------------------------------------------------------------------------------------------------------------------------|
| predictions                | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                |
| predictions.street         | String          | Ja      | Enthält den Straßennamen zu diesem Vorschlag. Veraltet, das Feld "streetName" sollte verwendet werden.                        |
| predictions.streetName     | String          | Ja      | Enthält den Straßennamen zu diesem Vorschlag. Identisch mit dem Feld "street".                                                |
| predictions.buildingNumber | String          | Ja      | Enthält die Hausnummer zu diesem Vorschlag.                                                                                   |
| predictions.additionalInfo | String          | Ja      | Enthält den Adresszusatz zu diesem Vorschlag.                                                                                 |
| status                     | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

## emailCheck

### Eingabeparameter

| Feld  | Erwarteter Wert | Pflicht | Bedeutung                   |
|-------|-----------------|---------|-----------------------------|
| email | String          | Ja      | Enthält die E-Mail-Adresse. |

### Ausgabefelder

| Feld   | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                     |
|--------|-----------------|---------|-------------------------------------------------------------------------------------------------------------------------------|
| status | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

## nameCheck

### Eingabeparameter

| Feld        | Erwarteter Wert | Pflicht                              | Bedeutung                                                                                                                                                                                                                                          |
|-------------|-----------------|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| title       | String          | Nein                                 | Enthält einen Titel, z.B. Dr.                                                                                                                                                                                                                      |
| salutation  | [m,f,d,x,s,o]   | Ja                                   | Enthält den Anrede-Code. Siehe [Tabelle der Anrede-Codes](#tabelle-der-anrede-codes).                                                                                                                                                              |
| name        | String          | Ja, wenn firstName nicht gesetzt ist | Enthält den Nachnamen. Veraltet und sollte nicht mehr verwendet werden.                                                                                                                                                                            |
| firstName   | String          | Ja, wenn name nicht gesetzt ist      | Enthält den Vornamen.                                                                                                                                                                                                                              |
| lastName    | String          | Ja, wenn name nicht gesetzt ist      | Enthält den Nachnamen.                                                                                                                                                                                                                             |
| countryCode | String          | Nein                                 | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md)                                                                                                                                                              |
| language    | String          | Nein                                 | Sprachkürzel für die Ausgabe. Aktuell wird nur die Ausgabe vom nameCheck beeinflusst, zukünftig wird das bei weiteren Services möglich sein. Standardwert ist DE. [Liste der Sprachcodes](https://de.wikipedia.org/wiki/Liste_der_ISO-639-1-Codes) |

### Ausgabefelder

| Feld                   | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                     |
|------------------------|-----------------|---------|-------------------------------------------------------------------------------------------------------------------------------|
| predictions            | Array           | Nein    | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                |
| predictions.title      | String          | Nein    | Enthält einen Titel zu diesem Vorschlag.                                                                                      |
| predictions.salutation | [m,f,d,x,s,o]   | Ja      | Enthält den Anrede-Code zu diesem Vorschlag. Siehe [Tabelle der Anrede-Codes](#tabelle-der-anrede-codes).                     |
| predictions.firstName  | String          | Ja      | Enthält den Vornamen zu diesem Vorschlag.                                                                                     |
| predictions.lastName   | String          | Ja      | Enthält den Nachnamen zu diesem Vorschlag.                                                                                    |
| score                  | Float           | Ja      | Enthält eine Zahl zwischen 0.0 und 1.0. Repräsentiert die Wahrscheinlichkeit oder Qualität der Aussage.                       |
| status                 | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### Tabelle der Anrede-Codes

| Code | Bedeutung                                               |
|------|---------------------------------------------------------|
| m    | männlich                                                |
| f    | weiblich                                                |
| d    | [divers](https://de.wikipedia.org/wiki/Divers)          |
| x    | unbekannt (kann sowohl männlich als auch weiblich sein) |
| s    | Familie                                                 |
| o    | Organisation                                            |

## phoneCheck

### Eingabeparameter

| Feld   | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                               |
|--------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| phone  | String          | Ja      | Enthält die Rufnummer.                                                                                                                                  |
| format | String          | Nein    | Enthält den Namen des Formats. Wird aktuell nur bei der Rufnummernprüfung verwendet. Siehe [Liste der Rufnummernformate](#liste-der-rufnummernformate). |

### Ausgabefelder

| Feld                            | Erwarteter Wert | Pflicht                             | Bedeutung                                                                                                                     |
|---------------------------------|-----------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| original                        | JSON            | Ja                                  | Identisch mit der Eingabe                                                                                                     |
| original.phone                  | String          | Ja                                  | Identisch mit der Eingabe                                                                                                     |
| predictions                     | Array           | Nein                                | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                |
| predictions.phone               | String          | Ja, wenn predictions vorhanden sind | Enthält die Original-Eingabe zu diesem Vorschlag.                                                                             |
| predictions.formatNational      | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im nationalen Format zu diesem Vorschlag.                                                                  |
| predictions.formatInternational | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im internationalen Format zu diesem Vorschlag.                                                             |
| predictions.formatE164          | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im E164-Format zu diesem Vorschlag. [E.164](https://de.wikipedia.org/wiki/E.164)                           |
| predictions.formatRFC3966       | String          | Ja, wenn predictions vorhanden sind | Enthält die Nummer im RFC3966-Format zu diesem Vorschlag. [RFC3966](https://datatracker.ietf.org/doc/html/rfc3966)            |
| predictions.region              | String          | Ja, wenn predictions vorhanden sind | Enthält die Region zu diesem Vorschlag.                                                                                       |
| predictions.carrier             | String          | Ja, wenn predictions vorhanden sind | Enthält den Netzanbieter zu diesem Vorschlag.                                                                                 |
| predictions.countryPrefix       | String          | Ja, wenn predictions vorhanden sind | Enthält die Landesvorwahl zu diesem Vorschlag.                                                                                |
| predictions.nationalNumber      | String          | Ja, wenn predictions vorhanden sind | Enthält den nationalen Teil zu diesem Vorschlag.                                                                              |
| predictions.nationalPrefix      | String          | Ja, wenn predictions vorhanden sind | Enthält die Ortsvorwahl zu diesem Vorschlag.                                                                                  |
| predictions.nationalBody        | String          | Ja, wenn predictions vorhanden sind | Enthält einen Teil der Telefonnummer ohne Landes- und Ortsvorwahl zu diesem Vorschlag.                                        |
| predictions.leadingZeros        | Int             | Ja, wenn predictions vorhanden sind | Enthält die Anzahl der führenden '0' in der nationalen Rufnummer zu diesem Vorschlag.                                         |
| status                          | Array           | Ja                                  | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

### Liste der Rufnummernformate

| Format        | Bedeutung                                                                         |
|---------------|-----------------------------------------------------------------------------------|
| E164          | Siehe [E.164-Format](https://de.wikipedia.org/wiki/E.164)                         |
| INTERNATIONAL | Internationale Schreibweise der Nummer                                            |
| NATIONAL      | Nationale Schreibweise der Nummer                                                 |
| RFC3966       | Schreibweise der Nummer, die im *href*-Attribut verwendet werden kann (tel: Link) |

## ibanCheck

### Eingabeparameter

| Feld | Erwarteter Wert | Pflicht | Bedeutung         |
|------|-----------------|---------|-------------------|
| iban | String          | Ja      | Enthält die IBAN. |

### Ausgabefelder

| Feld                               | Erwarteter Wert | Pflicht                             | Bedeutung                                                                                                                     |
|------------------------------------|-----------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| original                           | JSON            | Ja                                  | Identisch mit der Eingabe                                                                                                     |
| original.iban                      | String          | Ja                                  | Identisch mit der Eingabe                                                                                                     |
| predictions                        | Array           | Nein                                | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                |
| predictions.iban                   | String          | Ja, wenn predictions vorhanden sind | Enthält die formatierte IBAN zu diesem Vorschlag.                                                                             |
| predictions.checksum               | String          | Ja, wenn predictions vorhanden sind | Enthält die Prüfziffer der IBAN zu diesem Vorschlag.                                                                          |
| predictions.bankCode               | String          | Ja, wenn predictions vorhanden sind | Enthält eine Bankleitzahl zu diesem Vorschlag.                                                                                |
| predictions.bankName               | String          | Ja, wenn predictions vorhanden sind | Enthält den Namen der Bank zu diesem Vorschlag.                                                                               |
| predictions.bankPostalCode         | String          | Ja, wenn predictions vorhanden sind | Enthält die Postleitzahl der Bank zu diesem Vorschlag.                                                                        |
| predictions.bankLocality           | String          | Ja, wenn predictions vorhanden sind | Enthält den Ort der Bank zu diesem Vorschlag.                                                                                 |
| predictions.bankShortName          | String          | Ja, wenn predictions vorhanden sind | Enthält die verkürzte Schreibweise der Bank zu diesem Vorschlag.                                                              |
| predictions.bic                    | String          | Ja, wenn predictions vorhanden sind | Enthält den BIC zu diesem Vorschlag.                                                                                          |
| predictions.accountNumber          | String          | Ja, wenn predictions vorhanden sind | Enthält die Kontonummer zu diesem Vorschlag.                                                                                  |
| predictions.countryCentralBankName | String          | Ja, wenn predictions vorhanden sind | Enthält ggf. den Namen der Zentralbank zu diesem Vorschlag.                                                                   |
| predictions.countryCentralBankUrl  | String          | Ja, wenn predictions vorhanden sind | Enthält ggf. die URL zur Website der Zentralbank zu diesem Vorschlag.                                                         |
| status                             | Array           | Ja                                  | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md). |

## vatIdCheck

### vatIdCheck (Einfache Prüfung)

### Eingabeparameter

| Feld              | Erwarteter Wert | Pflicht | Bedeutung                               |
|-------------------|-----------------|---------|-----------------------------------------|
| vatId             | String          | Ja      | Enthält die zu prüfende Umsatzsteuer-ID |

### Ausgabefelder

| Feld              | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                             |
|-------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------------------|
| predictions       | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                        |
| predictions.vatId | String          | Ja      | Enthält die formatierte Umsatzsteuer-ID der zu prüfenden Firma.                                                                       |
| status            | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md).         |

### vatIdCheck (Qualifizierte Prüfung)

### Eingabeparameter

| Feld              | Erwarteter Wert | Pflicht | Bedeutung                                                        |
|-------------------|-----------------|---------|------------------------------------------------------------------|
| vatId             | String          | Ja      | Enthält die zu prüfende Umsatzsteuer-ID                          |
| requesterVatID    | String          | Ja      | Enthält die eigene Umsatzsteuer-ID                               |
| companyName       | String          | Ja      | Enthält den Firmennamen der zu prüfenden Umsatzsteuer-ID         |
| companyPostalCode | String          | Ja      | Enthält die Postleitzahl der zu prüfenden Firma                  |
| companyLocality   | String          | Ja      | Enthält den Ortsnamen der zu prüfenden Firma                     |
| companyStreetFull | String          | Ja      | Enthält den Straßennamen inkl. Hausnummer der zu prüfenden Firma |

### Ausgabefelder

| Feld                                               | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|-----------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| predictions                                        | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                                                                                                                                                                                                                                                                                |
| predictions.vatId                                  | String          | Ja      | Enthält die formatierte Umsatzsteuer-ID der zu prüfenden Firma.                                                                                                                                                                                                                                                                                                                               |
| predictions.companyName                            | String          | Ja      | Enthält den Firmennamen zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                                                  |
| predictions.companyAddress                         | String          | Ja      | Enthält die unstrukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                                |
| predictions.companyAddressFormatted                | String          | Ja      | Enthält die strukturierte Firmenadresse zu diesem Vorschlag. Wird nur zurückgegeben, wenn die Adresse in strukturierter Form vorliegt                                                                                                                                                                                                                                                         |
| predictions.companyAddressFormatted.streetFull     | String          | Ja      | Enthält die Straße inkl. Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                      |
| predictions.companyAddressFormatted.street         | String          | Ja      | Enthält den Straßennamen der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.houseNumber    | String          | Ja      | Enthält die Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                      |
| predictions.companyAddressFormatted.additionalInfo | String          | Ja      | Enthält den Adresszusatz der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.postCode       | String          | Ja      | Enthält die Postleitzahl der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                 |
| predictions.companyAddressFormatted.cityName       | String          | Ja      | Enthält den Ortsname der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                     |
| predictions.companyAddressFormatted.country        | String          | Ja      | Enthält den Ländercode der strukturierte Firmenadresse zu diesem Vorschlag.                                                                                                                                                                                                                                                                                                                   |
| certification                                      | JSON            | Nein    | Enthält Informationen zum Prüfzertifikat. Wird nur zurückgegeben, wenn es sich laut §18e UStG und 18e.1 Abs. 2 Satz 4 UStAE um eine qualifizierte Prüfung handelt. Wenn Prüfzertifikat nicht ausgegeben wird, bedeutet es, dass die Prüfung mit Daten aus Cache verglichen wurde, da BZSt Server nicht verfügbar ist. Die erneute Prüfung von UID zum späteren Zeitpunkt kann notwendig sein. |
| status                                             | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md).                                                                                                                                                                                                                                                                 |

> Alle erfolgreichen Prüfanfragen werden auf Seiten von endereco gespeichert und archiviert. 

## lucidCheck

### Eingabeparameter lucidCheck

| Feld    | Erwarteter Wert | Pflicht | Bedeutung                                                                  |
|---------|-----------------|---------|----------------------------------------------------------------------------|
| lucidId | String          | Ja      | Enthält die zu prüfende Registrierungsnummer vom Verpackungsregister Lucid |

### Ausgabefelder lucidCheck

| Feld                              | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                                                                                          |
|-----------------------------------|-----------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| predictions                       | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                                                                                     |
| predictions.lucidId               | String          | Ja      | Enthält die formatierte Registrierungsnummer vom Verpackungsregister Lucid                                                                                                                         |
| predictions.companyName           | String          | Ja      | Enthält den Firmennamen zu diesem Vorschlag.                                                                                                                                                       |
| predictions.kindOfReferenceNumber | String          | Ja      | Enthält den Typ der Referenznummer <br> vat: Umsatzsteuer-Identifikationsnummer <br> tax: Steuernummer <br> unknown: Unbekannt Firmenadresse zu diesem Vorschlag.                                  |
| predictions.referenceNumber       | String          | Ja      | Enthält die Referenznummer                                                                                                                                                                         |
| predictions.packagingDetailCode   | Int             | Ja      | Angabe, ob es Verpackungen mit Systembeteiligungspflicht sind <br>1: Verpackungen mit Systembeteiligungspflicht <br>2: Verpackungen ohne Systembeteiligungspflicht <br>3: beides <br> 4: Unbekannt |
| status                            | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md).                                                                      |

## companyAutocomplete

### Eingabeparameter lucidCheck

| Feld        | Erwarteter Wert | Pflicht | Bedeutung                                                                             |
|-------------|-----------------|---------|---------------------------------------------------------------------------------------|
| countryCode | String          | Nein    | ISO 3166-1 Alpha-2 Code des Landes, siehe [Liste der Ländercodes](./country-codes.md) |
| companyName | String          | Ja      | Enthällt den Firmennamen nach dem gesucht werden soll                                 |

### Ausgabefelder

| Feld                                               | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                             |
|----------------------------------------------------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------------------|
| predictions                                        | Array           | Ja      | Enthält JSON-Objekte mit Korrekturvorschlägen.                                                                                        |
| predictions.vatId                                  | String          | Ja      | Enthält die formatierte Umsatzsteuer-ID der zu prüfenden Firma.                                                                       |
| predictions.companyStatus                          | String          | Ja      | Enthält den Status zu der Firma (active, inaktive, unknown)                                                                           |
| predictions.companyName                            | String          | Ja      | Enthält den Firmennamen zu diesem Vorschlag.                                                                                          |
| predictions.companyAddress                         | String          | Ja      | Enthält die unstrukturierte Firmenadresse zu diesem Vorschlag.                                                                        |
| predictions.companyAddressFormatted                | String          | Ja      | Enthält die strukturierte Firmenadresse zu diesem Vorschlag. Wird nur zurückgegeben, wenn die Adresse in strukturierter Form vorliegt |
| predictions.companyAddressFormatted.streetFull     | String          | Ja      | Enthält die Straße inkl. Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                              |
| predictions.companyAddressFormatted.street         | String          | Ja      | Enthält den Straßennamen der strukturierte Firmenadresse zu diesem Vorschlag.                                                         |
| predictions.companyAddressFormatted.houseNumber    | String          | Ja      | Enthält die Hausnummer der strukturierte Firmenadresse zu diesem Vorschlag.                                                           |
| predictions.companyAddressFormatted.additionalInfo | String          | Ja      | Enthält den Adresszusatz der strukturierte Firmenadresse zu diesem Vorschlag.                                                         |
| predictions.companyAddressFormatted.postCode       | String          | Ja      | Enthält die Postleitzahl der strukturierte Firmenadresse zu diesem Vorschlag.                                                         |
| predictions.companyAddressFormatted.cityName       | String          | Ja      | Enthält den Ortsname der strukturierte Firmenadresse zu diesem Vorschlag.                                                             |
| predictions.companyAddressFormatted.country        | String          | Ja      | Enthält den Ländercode der strukturierte Firmenadresse zu diesem Vorschlag.                                                           |
| status                                             | Array           | Ja      | Enthält eine Liste aus Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](./statuscodes.md).         |
