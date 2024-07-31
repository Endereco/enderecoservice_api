# E-Mail-Prüfung

- [Struktur der Schnittstelle](#struktur-der-schnittstelle)
  - [Methode](#methode)
  - [Eingabeparameter](#eingabeparameter)
  - [Ausgabefelder](#ausgabefelder)
  - [Statuscodes](#statuscodes)
    - [Gernelle Statuscodes](#generelle-statuscodes)
    - [Erweiterte Statuscodes](#erweiterte-statuscodes)
    - [Veraltete Statuscodes](#veraltete-statuscodes)
- [Beispiele](#beispiele)
  - [Gültige E-Mail-Adresse](#gültige-e-mail-adresse)

# Struktur der Schnittstelle

Die Struktur der API ist [hier](./../structure-api.md) beschrieben

## Methode

Für die E-Mail-Prüfung muss ```emailCheck``` als Methode angegeben werden

## Eingabeparameter

| Feld  | Erwarteter Wert | Pflicht | Bedeutung                   |
|-------|-----------------|---------|-----------------------------|
| email | String          | Ja      | Enthält die E-Mail-Adresse. |

## Ausgabefelder

| Feld   | Erwarteter Wert | Pflicht | Bedeutung                                                                                                                 |
|--------|-----------------|---------|---------------------------------------------------------------------------------------------------------------------------|
| status | Array           | Ja      | Enthält eine Liste mit Statuscodes, die den geprüften Datensatz beschreiben. Siehe [Liste der Statuscodes](#statuscodes). |

## Statuscodes

### Generelle Statuscodes

| Code               | Bedeutung                                                                |
|--------------------|--------------------------------------------------------------------------|
| email_correct      | Die E-Mail-Adresse ist korrekt.                                          |
| email_not_correct  | Die E-Mail-Adresse ist nicht korrekt.                                    |
| email_syntax_error | die Schreibweise der E-Mail-Adresse ist falsch.                          |
| email_no_mx        | Es gibt keinen E-Mail-Server hinter der E-Mail-Domain.                   |
| email_unknown_tld  | Top-Level-Domain der E-Mail-Adresse ist nicht bekannt/existiert nicht.   |

### Erweiterte Statuscodes

| Code                                   | Bedeutung                                                                                             |
|----------------------------------------|-------------------------------------------------------------------------------------------------------|
| email_deliverable                      | E-Mails können zugestellt werden                                                                      |
| email_undeliverable                    | E-Mails können nicht zugestellt werden                                                                |
| email_risky                            | E-Mail-Adresse ist riskant. Es konnte nicht sichergestellt werden, ob E-Mails empfangen werden können |
| email_disposable                       | Es handelt sich um eine Wegwerf-E-Mail-Adresse, die nur für kurze Zeit existiert                      |
| email_at_sign_not_found                | Die E-Mail-Adresse enthält kein @-Zeichen                                                             |
| email_dns_error                        | Der DNS-Server ist nicht erreichbar                                                                   |
| email_dns_timeout                      | Der Verbindungsaufbau zum DNS-Server hat zu lange gedauert                                            |
| email_domain_does_not_exist            | Die Domain der E-Mail-Adresse existiert nicht                                                         |
| email_domain_is_misconfigured          | Die Domain der E-Mail-Adresse ist falsch konfiguriert                                                 |
| email_domain_part_compliance_failure   | Der Domain-Teil der E-Mail-Adresse ist nicht vollständig                                              |
| email_double_dot_sequence              | In der E-Mail-Adresse wurden zwei Punkte direkt hintereinander gefunden                               |
| email_invalid_address_length           | Die Länge der E-Mail-Adresse ist nicht zulässig                                                       |
| email_invalid_character_sequence       | In der E-Mail-Adresse befindet sich ein ungültiges Zeichen                                            |
| email_local_endpoint_rejected          | Die Prüfung der E-Mail-Adresse wurde abgelehnt                                                        |
| email_local_sender_address_rejected    | Die Prüfung der Absender-E-Mail-Adresse wurde abgelehnt                                               |
| email_mailbox_does_not_exist           | Für diese E-Mail-Adresse existiert kein Postfach                                                      |
| email_mailbox_full                     | Der Postfachspeicher ist voll                                                                         |
| email_mailbox_temporarily_unavailable  | Das Postfach ist temporär nicht erreichbar                                                            |
| email_spam_trap_detected               | Der Provider hat eine Spam-Falle erkannt                                                              |
| email_server_is_catch_all              | Diese Domain akzeptiert alle E-Mail-Adressen, unabhängig vom Text vor dem @-Zeichen                   |

### Veraltete Statuscodes

!! **DEPRECATED** !!

Diese Codes werden bald mit sprechenden Statuscodes ersetzt. Bitte nicht bei der Eigenentwicklung einsetzen.

| Code   | Bedeutung                                                                                            |
|--------|------------------------------------------------------------------------------------------------------|
| A1000	 | Alles OK. Die E-Mail Adresse soll zustellbar sein und es wurden keine Tricks auf dem Server erkannt. |
| A1100	 | Die E-Mail Adresse werden zu jeder E-Mail Adresse akzeptiert.                                        |
| A1110	 | Alle E-Mails zu einer bestimmten Domain werden akzeptiert.                                           |
| A1400	 | Die E-Mails werden akzeptiert, es handelt sich jedoch um eine "disposable" E-Mail Adresse.           |
| A4000	 | Fehler.                                                                                              |
| A4100	 | SMTP Fehler.                                                                                         |
| A4110	 | Man könnte sich zwar mit SMTP verbinden, aber danach bricht die Verbindung ab.                       |
| A4200	 | Falsche Syntax.                                                                                      |
| A4300	 | Server kennt die E-Mail Adresse nicht.                                                               |
| A4400	 | E-Mail Zustellung fehlgeschlagen.                                                                    |
| A4500	 | Relay Fehler.                                                                                        |
| A4600	 | Antispam blockiert die eMail.                                                                        |
| A4700	 | E-Mail Konto existiert nicht, ist inaktiv oder kann keine eMail empfangen.                           |
| A4800	 | Kein E-Mail Server auf der Domain gefunden.                                                          |
| A4810	 | E-Mail Server ist down.                                                                              |
| A4900	 | Span traps erkannt.                                                                                  |
| A5000	 | Unbekannter Fehler.                                                                                  |

# Beispiele

## Gültige E-Mail-Adresse

### Anfrage

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "emailCheck",
  "params": {
    "email": "info@endereco.de"
  }
}
```

### Antwort

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "status": [
      "A1000",
      "email_correct"
    ]
  }
}
```
