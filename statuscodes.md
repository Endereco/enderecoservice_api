# Liste aller Statuscode und ihre Interpretation 

## Inhaltsverzeichnis

| Service | Statuscode Gruppe | 
|---|---|
| [Adresseingabeassistent](#adresseingabeassistent) | [Generelle Statuscodes](#generelle-statuscodes) |
| [Adressprüfung](#adressprüfung) | [Generelle Statuscodes](#generelle-statuscodes-1) <br> [Erweiterte Statuscodes](#erweiterte-statuscodes) <br> [Experimentelle Statuscodes](#experimentelle-statuscodes) <br> [Veraltete Statuscodes](#veraltete-statuscodes) |
| [Namensprüfung](#namensprüfung) | [Generelle Statuscodes](#generelle-statuscodes-2) <br> [zus. Echtheitsprüfung](#mit-aktivem-zusatzservice-echtheitsprüfung) |
| [Rufnummernprüfung](#namensprüfung) | [Statuscodes](#statuscodes) |
| [IBAN Prüfung](#iban-prüfung) | [Generelle Statuscodes](#generelle-statuscodes-3) <br> [zus. Kontonummernprüfung](#mit-dem-zusatzservice-kontonummernprüfung) |
| [E-Mail Adresse Prüfung](#e-mail-adresse-prüfung) | [Generelle Statuscodes](#generelle-statuscodes-4) <br> [zus. Erweiterte SMTP Prüfung](#mit-aktivem-zusatzservice-erweiterte-smtp-prüfung) |
| [Umsatzsteuer ID Prüfung](#umsatzsteuer-id-prüfung) | [Statuscodes](#statuscodes-1) |

## Adresseingabeassistent

### Generelle Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| A1000 | Es gibt nur ein Element in der Vorschlagsliste. |
| A2000 | Es gibt mehrere Elemente in der Vorschlagsliste. |
| A3000 | Es wurden keine Elemente für die Vorschlagsliste gefunden. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

## Adressprüfung

### Generelle Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code                             | Bedeutung |
|----------------------------------| --------- |
| address_correct                  | Die eingegebene Adresse war korrekt und brauchte keine Korrektur. |
| address_needs_correction         | Die eingegebene Adresse ist nicht korrekt, im “predictions” liegt ein Korrekturvorschlag. |
| address_multiple_variants        | Für die Eingabe gab es mehrere Varianten. Der Nutzer soll eine auswählen. |
| address_not_found                | Die Adresse ist nicht bekannt.  |
| address_of_not_supported_type    | Bei der Adresse geht es um einen Typ der von uns nicht unterstützt wird, z.B. eine Postfiliale, eine Packstation. |
| address_selected_by_customer     | Die Adresse wurde von einem Nutzer manuell bestätigt. |
| country_code_needs_correction    | Ländercode ist falsch und muss korrigiert werden. |
| country_code_correct             | Ländercode ist korrekt. |
| postal_code_needs_correction     | Postleitzahl ist falsch und muss korrigiert werden. |
| postal_code_correct              | Postleitzahl ist korrekt. |
| locality_needs_correction        | Ortsname ist falsch und muss korrigiert werden. |
| locality_correct                 | Ortsname ist korrekt. |
| street_name_needs_correction     | Straßenname ist falsch und muss korrigiert werden. |
| street_name_correct              | Straßenname ist korrekt. |
| street_full_needs_correction     | Volle Straße (inkl. Hausnummer) ist falsch und muss korrigiert werden. |
| street_full_correct              | Volle Straße ist korrekt. |
| building_number_needs_correction | Hausnummer ist falsch und muss korrigiert werden. |
| building_number_correct          | Hausnummer ist korrekt. |
| building_number_is_missing       | Hausnummer wurde nicht angegeben. |
| building_number_not_found        | (optional) Hausnummer wurde auf der Straße und in dem PLZ-Bereich nicht gefunden. |
| additional_info_needs_correction | Zusatzinfos sind falsch und müssen korrigiert werden. |
| additional_info_correct          | Zusatzinfos sind korrekt. |
| address_is_packstation           | Bei der Adresse handelt es sich um eine Packstation. |
| address_is_postoffice            | Bei der Adresse handelt es sich um eine Postfiliale. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

### Erweiterte Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

Um den manuellen Aufwand bei der Bearbeitung von Adressprüfungen zu minimieren, kann zusätzlich zu Ergebnissen der
Adressprüfung eine serverseitige Klassifizierung die Änderungen bewerten.
So kann leichter entschieden werden, ob eine vorgeschlagene Korrektur automatisch übernommen werden darf oder bestätigt werden muss.

Die Statuscodes werden dabei nach einem einheitlichen Muster gebaut: [TEIL]_[minor|major]_correction

| Code | Bedeutung |
| ---- | --------- |
| address_minor_correction | Es handelt sich um eine "kleine" Korrektur, die automatisch übernommen werden kann. |
| address_major_correction | Es handelt sich um eine "kritische" Korrektur, bei der eine automatische Korrekturübernahme unmöglich ist. |
| country_code_minor_correction | Kleine Landkorrektur. Wird aktuell nie ausgegeben. |
| country_code_major_correction | Wichtige Landeskorrektur. |
| postal_code_minor_correction | Kleine Korrektur der PLZ. |
| postal_code_major_correction | Wichtige Korrektur der PLZ. |
| locality_minor_correction | Kleine Korrektur des Ortes. |
| locality_major_correction | Wichtige Korrektur des Ortes. |
| street_name_minor_correction | Kleine Korrektur der Straße. |
| street_name_major_correction | Wichtige Korrektur der Straße. |
| building_number_minor_correction | Kleine Korrektur der Hausnummer. |
| building_number_major_correction | Wichtige Korrektur der Hausnummer. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

### Experimentelle Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

Je nach Server Konfiguration und dem verwendeten Prüfalgorithmen können noch folgende Statuscodes ausgegeben werden:

| Code | Bedeutung |
| ---- | --------- |
| address_contains_unverified_info | In der Adresse gibt es einen Teil, der nicht geprüft werden kann, der jedoch auch nicht entfernt wird. |
| building_number_contains_unverified_info | In der Hausnummer wurde ein Element übermittelt, das nicht geprüft werden kann und in der Prüfung ignoriert wurde . z.B. ein Zustellhinweis. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

### Veraltete Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

!! **DEPRECATED** !!

Falls du noch die alten Statuscodes mit Zahlen nutzest, empfehlen wir diese mit neuen Statuscodes zu 
ersetzen, da die alten Statuscodes Mitte/Ende 2022 nicht mehr unterstützt werden.

Das Mapping ist 1-zu-1, bis auf den Statuscode `A1100`. Bei ihm entspricht die Kombination `A1000` + `A1100` dem 
Statuscode `address_needs_correction`. Das heißt bei einer korrekturbedürftigen Adresse kommt der 
Statuscode `address_correct` nicht vor.

| Veralteter Code | Neuer Code | Bedeutung |
| ---- | --- | --------- |
| A1000 | address_correct |Adresse korrekt. |
| A1100 | address_needs_correction | Adresse korrekt, aber korrigiert. Gibt es, wenn es nur eine Variante der Adresse gibt, diese sich jedoch einwenig unterscheidet. Kommt mit A1000 zusammen.|
| A2000 | address_multiple_variants | Mehrere Varianten. |
| A3000 | address_not_found | Keine Varianten. Die Adresse existiert nicht in unseren Datenbanken. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

## Namensprüfung

### Generelle Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| name_is_natural_person | Bei dem übermittelten Namen handelt es sich um eine natürliche Person. z.B. "Mario Fischer" |
| name_is_family | Bei dem Namen handelt es sich um eine Familie, z.B. "Familie Müller |
| name_is_organisation | Bei dem Namen handelt es sich um eine Organisation, z.B. "Endereco GmbH" |
| name_is_group | Bei dem Namen handelt es sich um eine Gruppe. |
| name_multiple_variants | Bei dem Namen wurden mehrere Personen entdeckt, z.B. "Maria und Fritz Fleischmann" |
| name_correct | Der Name ist bekannt und korrekt geschrieben. |
| name_needs_correction | Die bekannte Schreibweise des Namens unterscheidet sich von der übermittelten. |
| salutation_needs_correction | Anrede soll korrigiert werden. |
| first_name_needs_correction | Vorname soll korrigiert werden. | 
| last_name_needs_correction | Nachname soll korrigiert werden. |
| name_transpositioned | Vorname und Nachname wurden vertauscht und stehen in falschen Feldern. |
| name_has_duplicates | Teile des Namens sind in mehreren Feldern gleichzeitig anwesend, z.B. "Ilja Weber" im Vornamenfeld, "Weber" Nachnamenfeld |
| name_not_found | Die Schreibweise des Namens ist unbekannt. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

### Mit aktivem Zusatzservice "Echtheitsprüfung"

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| name_is_real | Der Name ist 100% echt. |
| name_is_probably_real | Der Name ist mit Wahrscheinlichkeit von 75% oder mehr echt. |
| name_is_probably_fake | Der Name ist mit Wahrscheinlichkeit von 50% oder mehr echt. |
| name_is_fake | Der Name ist definitiv fake. |
| name_has_placeholder | Der Name ist ein Platzhalter, z.B. "Max Musterman" |
| name_has_random_typing | Der Name ist eine Zufallskette an Zeichen, z.B. "Asdasdasd fAsfasf" |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

## Rufnummernprüfung

### Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| phone_correct | Die Telefonnummer ist korrekt und richtig formatiert. |
| phone_invalid | Die Telefonnummer ist falsch geschrieben. Es gibt keine Korrekturvorschläge. |
| phone_needs_correction | Die Telefonnummer ist nicht ganz korrekt, kann aber korrigiert werden. |
| phone_format_needs_correction | Die Telefonnummer ist falsch formatiert. Voraussetzung ist, dass ein Format in den Eingabeparametern spezifiziert wurde. |
| phone_format_e164 | Die übermittelte Telefonnummer ist im [Format E.164](https://www.bundesnetzagentur.de/SharedDocs/Downloads/DE/Sachgebiete/Telekommunikation/Unternehmen_Institutionen/Nummerierung/Rufnummern/NP_Nummernraum.pdf?__blob=publicationFile&v=3) |
| phone_format_international | Die übermittelte Telefonnummer ist international formatiert. |
| phone_format_national | Die übermittelte Telefonnummer ist national formatiert. |
| phone_format_rfc3966 | Die übermittelte Telefonnummer ist ein tel: Link. |
| phone_format_unknown | Das Format konnte nicht erkannt werden. |
| phone_is_mobile | Die Telefonnummer ist eine Mobilfunknetznummer. |
| phone_is_fixed_line | Die Telefonnummer ist eine Festnetznummer. |
| phone_is_unknown_type | Der Typ der Telefonnummer konnte nicht eindeutig erkannt werden. |
| phone_hlr_lookup_succeeded | Die Telefonnummer wurde über HLR-Lookup geprüft und ist aktiv. |
| phone_hlr_lookup_failed | Die Telefonnummer wurde über HLR-Lookup geprüft und ist inaktiv. |
| phone_hlr_lookup_not_possible | HLR-Lookup ist für die Telefonnummer nicht möglich (z.B. weil es eine Festnetznummer ist). |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

## IBAN Prüfung

### Generelle Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| iban_correct | Die übermittelte IBAN ist korrekt. |
| iban_needs_correction | Die übermittelte IBAN soll korrigiert werden. |
| iban_invalid | Die übermittelte IBAN ist falsch. |
| iban_checksum_correct | Die Prüfsumme der IBAN ist richtig. |
| iban_checksum_invalid | Die Prüfsumme der IBAN ist falsch. |
| iban_length_correct | Die Länge der IBAN ist richtig. |
| iban_length_invalid | Die Länge der IBAN ist falsch. |
| iban_is_sepa | Die IBAN ist SEPA fähig. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

### Mit dem Zusatzservice "Kontonummernprüfung"

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| iban_account_correct | Die Kontonummer ist korrekt. |
| iban_account_invalid | Die Kontonummer ist falsch. |
| iban_account_checkmethod_X | Die Kontonummer wurde mit der Prüfzimmermethode X geprüft. Anstatt von X steht die Nummer der Prüfzimmermethode, z.B. "10" |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)


## E-Mail Adresse Prüfung

### Generelle Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| email_correct | Die E-Mail Adresse ist korrekt. |
| email_not_correct | Die E-Mail Adresse ist nicht korrekt. |
| email_syntax_error | die Schreibweise der E-Mail Adresse ist falsch. |
| email_no_mx | Es gibt keinen E-Mail Server hinter der E-Mail Domain. |
| email_unknown_tld | Top-Level-Domain der E-Mail Adresse ist nicht bekannt / existiert nicht. |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

### Mit aktivem Zusatzservice "Erweiterte SMTP Prüfung"

siehe [Veraltete Codes](#veraltete-codes)

### Veraltete Codes

**!!DEPRECATED!!**

Diese Codes werden bald mit sprechenden Statuscodes ersetzt. Bitte nicht bei der Eigenentwicklung einsetzen.

| Code | Bedeutung |
| ---- | --------- |
| A1000	| Alles OK. Die E-Mail Adresse soll zustellbar sein und es wurden keine Tricks auf dem Server erkannt. |
| A1100	| Die E-Mail Adresse werden zu jeder E-Mail Adresse akzeptiert. |
| A1110	| Alle E-Mails zu einer bestimmten Domain werden akzeptiert. |
| A1400	| Die E-Mails werden akzeptiert, es handelt sich jedoch um eine "disposable" E-Mail Adresse. |
| A4000	| Fehler. |
| A4100	| SMTP Fehler. |
| A4110	| Man könnte sich zwar mit SMTP verbinden, aber danach bricht die Verbindung ab. |
| A4200	| Falsche Syntax. |
| A4300	| Server kennt die E-Mail Adresse nicht. |
| A4400	| E-Mail Zustellung fehlgeschlagen. |
| A4500	| Relay Fehler. |
| A4600	| Antispam blockiert die eMail. |
| A4700	| E-Mail Konto existiert nicht, ist inaktiv oder kann keine eMail empfangen. |
| A4800	| Kein E-Mail Server auf der Domain gefunden. |
| A4810	| E-Mail Server ist down. |
| A4900	| Span traps erkannt. |
| A5000	| Unbekannter Fehler. |

## Umsatzsteuer ID Prüfung

### Statuscodes

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)

| Code | Bedeutung |
| ---- | --------- |
| vat_valid | Umsatzsteuer ID ist korrekt. |
| vat_needs_correction | Schreibweise der Umsatzsteuer ID ist korrekturbedürftig. |
| vat_unknown_country_code | Landeskürzel ist falsch oder unbekannt. | 
| vat_wrong_format | Format entspricht nicht der [Spezifikation](http://ec.europa.eu/taxation_customs/vies/faq.html?locale=de#item_11). | 
| vat_has_company_name | Für die Umsatzsteuer ID konnte der Firmenname ermittelt werden. Der Firmenname ist in der Ausgabe vorhanden. | 
| vat_invalid | Die Umsatzsteuer ID ist ungültig.  |

[zurück zum Inhaltsverzeichnis ↑](#liste-aller-statuscode-und-ihre-interpretation)
