# Endereco API

Endereco API stellt einen elektronischen und automatisierbaren Zugang zu den [Leistungen der Firma Endereco UG (haftungsbeschränkt)](https://www.endereco.de/services/) her. 
Darunter fällt unter anderem die Prüfung der Adressen, E-Mail Adressen und Telefonnummern.

## Über 200 Unternehmen prüfen ihre Stammdaten mit Endereco

Egal ob Onlineshop, Warenwirtschaftssystem oder eigene Entwicklung, Endereco kümmert sich um die Qualität der Daten und spart ihren Kunden den Mehraufwand durch schlechte Stammdaten. 

Unsere Referenzen:

|  |  |  |  |
|---|---|---|---|
| ![](./imgs/bahn-805cdfd6.png "") | ![](./imgs/bodymed-3fa122cb.png "") | ![](./imgs/esmeyer-9ea34a21.png "") | ![](./imgs/leuchtstark-5bc7e2f1.png "") |
| ![](./imgs/lodenfrey-504abfa7.png "") | ![](./imgs/paltinum-9f33b7f8.png "") | ![](./imgs/session-a3b56a76.png "") | ![](./imgs/sheego-b19f87e2.png "") |

## Vorbereitung

Wenn du die API nutzen willst, dann kannst du so vorgehen:

1. Frage [über diese Form](https://share.hsforms.com/1Hez4RJSYQt2EFH_tdnCjsw3e78w) oder über E-Mail Kontakt info@endereco.de oder support@endereco.de nach einem API-Key. Für einen API-Key wirst du mit Endereco einen AV-Vertrag abschließen müssen: nur so dürfen wir die Daten prüfen.
2. Probiere unsere [Postman-Collection](https://todo.com) aus. Dort sind alle gängige Anfragen schon vorformuliert. Die [Import-Datei](https://todo.com) wurde für die aktuelle Version von [Postman](https://www.postman.com/) erstellt.
3. Falls du noch Fragen hast, suche dir den Ansprechspartner aus der Liste unten.

### Ansprechspartner
| Art der Frage | zuständige Person | Kontakt |
|---|---|---|
| geschäftliche/finanzielle Frage | Robert Rieser  | robert@endereco.de oder info@endereco.de |
| technische Frage | Ilja Weber | ilja@endereco.de oder suppoert@endereco.de  |
| Frage zu Datenschutz | Lena Schmitt | lena@endereco.de oder datenschutz@endereco.de |

## Übersicht der Funktionen

| Adresse | E-Mail Adresse | Person | Telefonnummer | IBAN | Umsatzsteuer ID | technische Methoden |
|---|---|---|---|---|---|---|
| Adressprüfung | E-Mail Prüfung | Namensprüfung | Rufnummerprüfung | IBAN-Prüfung | Umsatzsteuer ID Prüfung | Abschluss einer Session |
| PLZ Vorschläge | | | | | | Abschluss eine Konversion |
| Ortsvorschläge | | | | | |
| Straßenvorschläge | | | | | |

## Übersicht der Statuscodes

In der Antwort der API wird i.d.R. eine Liste von Statuscodes zurückgegeben. Diese Statuscodes beschreiben den geprüften Datensatz und diktieren weiter folgende Reaktion. Die [vollständige Liste der Statuscodes](./statuscodes.md) ist hilfreich für Entwickler, die die Prüfungen von Endereco tief in ihre Geschäftsprozesse integrieren wollen.. 

Vorgesehene Reaktion auf bestimmte Statuscodes ist in [Frontend Umsetzung](./ui-requirements.md) beschrieben. Diese ist für UI 

## Request oder Session

Die Leistungen von Endereco sind kostenpflichtig. Die einfachste Art der Abrechnung ist wenn man pro Anfrage einen gewissen Betrag zahlt.

Endereco versucht jedoch dem Nutzer entgegen zu kommen und einen Betrag erst dann in rechnung zu stellen ,wenn dieser auch tatsächlich was genutzt hat. Wir nennen es "erfolgsbasierte Abrechnung". Erst wenn ein bestimmter logischer Datensatz (z.B. eine Lieferadresse) nach der Verarbeitung durch Endereco gespeichert wird, rechnen wir ab.

Um die erfolgsbasierte Abrechnung zu nutzen, müssen die sog. Sessions umgesetzt werden. Dafür haben wir [eine Anleitung](./generating-sessions.md) erstellt. Wenn du dazu noch Fragen hast, melde dich bei unseren [Ansprechspartnern](#Ansprechspartner).

## Methoden der API

Grundstäzlicher Aufbau des Protokolls ist in der [JSON-RPC 2.0 Spezifikation](https://www.jsonrpc.org/specification) beschrieben. Es muss eine JSON Datei formuliert werden und man bekommt eine JSON Datei als Antwort. Die JSON wird im Body der HTTP Anfrage übermittelt. 
Weitere Steuerinformationen wie API-Key werden in Headers der HTTP Anfrage übermittelt. Es wird nur Server-to-Server Kommunikation erlaubt, Anfragen aus dem Browser sind nicht zugelassen, wegen [Datenschutz](./data-protection.md).

### Prüfung einer Adresse mit getrennten Straße/Hausnummer

```
POST https://endereco-service.de/rpc/v1
```
#### Request Headers
|  |  |
|---|---|
| Content-Type| application/json  |
| X-Transaction-Id | not_required, siehe [Generierung der Session ID's](./generating-sessions.md) |
| X-Transaction-Referer | MyClient v1.0.0, siehe [Referrer übergeben](./providing-referrer.md) |
| X-Auth-Key | siehe [API-Key Anfragen](./request-apikey.md) |

#### Body raw (JSON)

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

#### Antwort Basis

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

#### Antwort Basis + Zusatzfunktion "Automatische Korrekturübernahme"
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
      "street_name_minor_correction",
      "building_number_correct",
      "address_minor_correction"
    ]
  }
}
```

#### Antwort Basis + Zusatzfunktion "Hausnummerexistenzprüfung"
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "predictions": [],
    "status": [
      "address_needs_correction",
      "A1100",
      "country_code_correct",
      "postal_code_correct",
      "locality_correct",
      "street_name_needs_correction",
      "building_number_needs_correction",
      "building_number_not_found"
    ]
  }
}
```
