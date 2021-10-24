# Liste aller Statuscode und ihre Interpretation

|[Adresseingabe-Assistent](#Adresseingabeassistent)|
|[Generelle Statuscodes](#generelle_statuscodes)|

|Adressprüfung|
|  Generelle Statuscodes|
|  Zusatz Erweitere Statuscodes|
|  Experimentelle Statuscodes|
|  Veraltete Statuscodes|
  
|Namensprüfung|
|  Generelle Statuscodes|
|  Zusatz Echtheitsprüfung|
  
|Telefnonnummernprüfung|
|  Generelle Statuscodes|
  
|IBAN Prüfung|
|  Generelle Statuscodes|
|  Zusatz Kontonummerprüfung|
  

## Adresseingabeassistent

### Generelle Statuscodes

| Code | Bedeutung |
| ---- | --------- |
| A1000 | Es gibt nur ein Element in der Vorschlagsliste. |
| A2000 | Es gibt mehrere Elemente in der Vorschlagsliste. |
| A3000 | Es wurden keine Elemente für die Vorschlagsliste gefunden. |

## Adressprüfung

### Generelle Statuscodes:

| Code | Bedeutung |
| ---- | --------- |
| address_correct | Die eingegebene Adresse war korrekt und brauchte keine Korrektur. |
| address_needs_correction | Die eingegebene Adresse ist nicht korrekt, im “predictions” liegt ein Korrekturvorschlag. |
| address_multiple_variants | Für die Eingabe gab es mehrere Varianten. Der Nutzer soll eine Auswählen. |
| address_not_found | Die Adresse wurde gar nicht gefunden.  |
| address_of_not_supported_type | Bei der Adressat geht es um einen Typ der von uns nicht unterstützt wird, z.B. eine Postfiliale, eine Packstation. |
| address_selected_by_customer | Die Adresse wurde von einem Nutzer manuell bestätigt. |
| country_code_needs_correction | Ländercode ist falsch und muss korrigiert werden. |
| country_code_correct | Ländercode ist korrekt. |
| postal_code_needs_correction | Postleitzahl ist falsch und muss korrigiert werden. |
| postal_code_correct | Postleitzahl ist korrekt. |
| locality_needs_correction | Ortsname ist falsch und muss korrigiert werden. |
| locality_correct | Ortsname ist korrekt. |
| street_name_needs_correction | Straßenname ist falsch und muss korrigiert werden. |
| street_name_correct | Straßenname ist korrekt. |
| street_full_needs_correction | Volle Straße (inkl Hausnummer) ist falsch und muss korrigiert werden. |
| street_full_correct | Volle Straße ist korrekt. |
| building_number_needs_correction | Hausnummer ist falsch und muss korrigiert werden. |
| building_number_correct | Hausnummer ist korrekt. |
| building_number_is_missing | Hausnummer wurde nicht angegeben. |
| building_number_not_found | (optional) Hausnummer wurde auf der Straße und in der PLZ nicht gefunden. |
| additional_info_needs_correction | Zusatzinfos sind falsch und müssen korrigiert werden. |
| additional_info_correct | Zusatzinfos sind korrekt. |
| address_is_packstation | Bei dem Adress handelt es sich um eine Packstation. |
| address_is_postfiliale | Bei der Adresse handelt es sich um eine Postfiliale. |
| address_is_postfach | Bei der Adresse handelt es sich um ein Postfach. |
| address_is_paketshop | Bei der Adresse handelt es sich um ein Paketshop. |


### Erweiterte Statuscodes:

Um den manuellen Aufwand bei Bearbeitung von Adressprüfungen zu minimieren, kann zusätzlich zu Ergebnissen der
Adressprüfung eine serverseitig klassifizierung die Änderungen bewerten.
So kann leichter entschieden werden ob eine vorgeschlagene Korrektur automatisch übernommen werden darf, oder bestätigt werden muss.

Die Statuscodes werden dabei nach einem einheitlichen Muster gebaut: [ADRESSTEIL]_[minor|major]_correction

| Code | Bedeutung |
| ---- | --------- |
| address_minor_correction | Es handelt sich um eine "kleine" Korrektur, die automatisch übernommen werden kann. |
| address_major_correction | Es handelt sich um eine "kritische" Korrektur, bei der eine automatische Korrekturübernahme unmöglich ist. |
| country_code_minor_correction | Kleine Landkorrektur. Wird akuell nie ausgegeben. |
| country_code_major_correction | Wichtige Landeskorrektur. |
| postal_code_minor_correction | Kleine Korrektur der PLZ. |
| postal_code_major_correction | Wichtige Korrektur der PLZ. |
| locality_minor_correction | Kleine Korrektur des Ortes. |
| locality_major_correction | Wichtige Korrektur des Ortes. |
| street_name_minor_correction | Kleine Korrektur der Straße. |
| street_name_major_correction | Wichtige Korrektur der Straße. |
| building_number_minor_correction | Kleine Korrektur der Hausnummer. |
| building_number_major_correction | Wichtige Korrektur der Hausnummer. |

### Experimentelle Statuscodes:

Je nach Server Konfiguration und dem verwendeten Prüfalgorithmen können noch folgende Statuscodes ausgegeben werden:

| Code | Bedeutung |
| ---- | --------- |
| address_contains_unverified_info | In der Adresse gibt es einen Teil, der nicht geprüft werden kann, der jedoch auch nicht entfernt wird. |
| building_number_contains_unverified_info | In der Hausnummer wurde ein Element übermittelt, das nicht geprüft werden kann und in der Prüfung ignoriert wurde . z.B. ein Zustellhinweis. |

### <span style="color:red;font-weight: bold;">Veraltete Statuscodes:</span>

!! **DEPRECATED** !!

| Code | Bedeutung |
| ---- | --------- |
| A1000 | Adresse korrekt. |
| A1100 | Adresse korrekt, aber korrigiert. Gibt es, wenn es nur eine Variante der Adresse gibt, diese sich jedoch einwenig unterscheidet. Kommt mit A1000 zusammen.|
| A2000 | Mehrere Varianten. |
| A3000 | Keine Varianten. Die Adresse existiert nicht in unseren Datenbanken. |

## Namensprüfung

### Statuscodes

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

### Mit aktivem Zusatzservice "Echtheitsprüfung"

| Code | Bedeutung |
| ---- | --------- |
| name_is_real | Der Name ist 100% echt. |
| name_is_probably_real | Der Name ist mit Wahrscheinlichkeit von 75% oder mehr echt. |
| name_is_probably_fake | Der Name ist mit Wahrscheinlichkeit von 50% oder mehr echt. |
| name_is_fake | Der Name ist definitiv fake. |
| name_has_placeholder | Der Name ist ein Platzhalter, z.B. "Max Musterman" |
| name_has_random_typing | Der Name ist eine Zufallskette an Zeichen, z.B. "Asdasdasd fAsfasf" |

## Telefonnummernprüfung 

### Statuscodes

| Code | Bedeutung |
| ---- | --------- |
| phone_correct | Die Telefonnummer ist korrekt und richtig formatiert. |
| phone_invalid | Die Telefonnummer ist falsch geschrieben. Es gibt keine Korrekturvorschläge. |
| phone_needs_correction | Die Telefonnummer ist nicht ganz korrekt, kann aber korrigiert werden. |
| phone_format_e164 | Die Telefonnummer ist im [Format E.164](https://www.bundesnetzagentur.de/SharedDocs/Downloads/DE/Sachgebiete/Telekommunikation/Unternehmen_Institutionen/Nummerierung/Rufnummern/NP_Nummernraum.pdf?__blob=publicationFile&v=3) |
| phone_format_international | Die Telefonnummer ist international formatiert. |
| phone_format_national | Die Telefonnummer ist national formatiert. |
| phone_format_rfc3966 | Die Telefonnummer ist ein tel: Link. |
| phone_format_undefined | Format konnte nicht erkannt werden. |
| phone_is_mobile | Die Telefonnummer ist eine Mobilfunknetznummer. |
| phone_is_fixed_line | Die Telefonnummer ist eine Festnetznummer. |
| phone_is_unknown_type | Der Typ der Telefonnummer konnte nicht eindeutig erkannt werden. |
| phone_carrier_X | Die Telefonnummer gehört oder gehörte ursprünglich dem X - Anbieter. Anstatt von X steht der Kürzel des Anbieters, z.B. "o2" |

## IBAN Prüfung

### Statuscodes

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

### Mit dem Zusatzservice "Kontonummerprüfung"

| Code | Bedeutung |
| ---- | --------- |
| iban_account_correct | Die Kontonummer ist korrekt. |
| iban_account_invalid | Die Kontonummer ist falsch. |
| iban_account_checkmethod_X | Die Kontonummer wurde mit der Prüfzimmermethode X geprüft. Anstatt von X steht die Nummer der Prüfzimmermethode, z.B. "10" |
