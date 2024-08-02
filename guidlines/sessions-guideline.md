# Sessions Guideline

## Unterschied zwischen Abrechnung pro Anfrage und Abrechnung pro Datensatz

Die einfachste Art, die endereco Services-API zu nutzen, ist, die Anfragen zu senden und pro Anfrage abgerechnet zu werden.
Jede Anfrage wird einzeln erfasst und getrennt von anderen gezählt.

Beim diesen Verfahren werden alle Anfrage zusammengezählt, unabhängig davon, ob der Nutzer die beabsichtigte Anmeldung bzw. den Kauf abschließt oder nicht.  
So kann jemand zwar eine Adresse eingeben, aber im letzten Moment seine Registrierung abbrechen und die Webseite
verlassen.

Als Alternative bietet endereco eine sog. "erfolgsbasierte Abrechnung". Technisch formuliert sieht das wie folgend aus:

Die Abrechnung erfolgt nicht pro gesendeter Anfrage, sondern pro gespeichertem Datensatz. Ein Datensatz kann eine Rechnungs- bzw. Lieferadresse, eine
Telefonnummer, ein Name etc. sein.

Um ein Datensatz zu erfassen, ist in manchen Fällen die Nutzung von mehreren Funktionen der API notwendig. Um zum Beispiel eine
Adresse zu erfassen, werden zuerst PLZ- und Ortseingabehilfe verwendet, danach Straßeneingabehilfe und abschließend die
Adressprüfung für die gesamte Adresse. Je nach Eingabe und Korrekturvorschlägen können danach noch weitere
Adressprüfungen folgen.

Wird der Datensatz danach im Zielsystem gespeichert, wird er einmalig abgerechnet.

Damit die endereco Services-API diesen Vorgang korrekt registrieren kann, sind einige weitere Metainformationen notwendig.
Diese müssen bei individueller Entwicklung selbst umgesetzt werden.

Dafür stellen wir zuerst zwei Konzepte vor:

- Session
- Abschluss einer Session

## Session

Eine Session ist der Prozess der Datensatzerfassung.

Jede Session hat eine eindeutige ID. Die ID ist nur dann als eindeutig zu sehen, wenn sie zufällig generiert wird und innerhalb von einem Monat nur
einmal vorkommt.

Jede Anfrage innerhalb der Session muss die Session ID im Header `x-transaction-id` übermitteln.

![](../imgs/sessionid.png "")

Dadurch kann die endereco Services-API serverseitig die Anfragen zusammenfassen.

**Wichtig!** Jeder Datensatz und jede neue Datenerfassung soll eine eigene Session-ID besitzen.

### Best Practice von endereco: UUID v4 generieren.

Wir setzen [UUID v4](https://en.wikipedia.org/wiki/Universally_unique_identifier#Version_4_(random)) als Session-ID ein.

Es gibt zahlreiche freie Bibliotheken, die diese ID in diversen Programmiersprachen implementieren.

Zum Beispiel PHP:

```shell
 composer require ramsey/uuid
```

```php
// datei.php

require './vendor/autoload.php';

use Ramsey\Uuid\Uuid;

$uuid = Uuid::uuid4();

echo $uuid;

// output: c07968ba-ec38-4cf2-8819-5b9c70b78a72
```

Beispiel JavaScript:

```shell
npm i uuidv4
```

```javascript
import {uuid} from 'uuidv4';

console.log(uuid());
// output: fe82c91a-7b02-44bd-93fc-1badb42666c7
```

## Abschluss der Session

Wenn im Laufe der Datenerfassung der Datensatz gespeichert wird, egal ob in der Datei, Datenbank, Session-Variable,
Cookie oder localStorage, erwartet die endereco Services-API eine Meldung darüber.

Die Meldung ist eine spezielle Anfrage, die, wie jede andere Anfrage, an die endereco Services-API gesendet werden muss.

Die Anfrage wird so formuliert:

```
POST https://endereco-service.de/rpc/v1
```

#### Request Headers

|                       |                                                                                        |
|-----------------------|----------------------------------------------------------------------------------------|
| Content-Type          | application/json                                                                       |
| X-Transaction-Id      | not_required, siehe [Generierung der Session ID's](sessions-guideline.md)              |
| X-Agent               | MyClient v1.0.0, siehe [Client ID Guideline](client-id-guideline.md)                   |
| X-Transaction-Referer | www.example.de/register, siehe [Referrer übergeben](./providing-referrer-guidlines.md) |
| X-Auth-Key            | siehe [Authentifizierung](./../readme.md#authentifizierung)                            |

#### Body raw (JSON)

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "doAccounting",
  "params": {
    "sessionsId": "c07968ba-ec38-4cf2-8819-5b9c70b78a72"
  }
}
```

Die Session-ID kommt hierbei doppelt vor: einmal im Header und einmal
als "sessionsId"-Parameter. Der Grund dafür ist, dass wir zukünftig den Abschluss mehrerer Sessions über einen Aufruf
ermöglichen wollen. Aktueller Stand ist die Übergangsphase.

#### Antwort

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "status": [
      "A1000"
    ]
  }
}
```

## Nutzung der JS-SDK

Falls in der Implementierung die [JS-SDK](https://github.com/Endereco/js-sdk) genutzt wird, kann die Generierung
der Session-ID übersprungen werden. Diese Aufgabe wird von [JS-SDK](https://github.com/Endereco/js-sdk) erledigt.

Die Session-IDs werden dabei als hidden input hinterlegt und beim Absenden der Form mit übermittelt.

Die Aufgabe des Entwicklers besteht darin, die so übermittelten Session IDs serverseitig zu empfangen und mittels "
doAccounting" and die endereco Services-API zu übermitteln.

## Abnahme der Eigenimplementierung durch Endereco

Wenn die Implementierung der erfolgsbasierten Abrechnung beabsichtigt wird, muss die Implementation erst durch endereco abgenommen und freigegeben
werden. Vereinbare hierfür bitte einen Termin mit uns via support@endereco.de

Vor der Abnahme ist nur die anfragenbasierte Abrechnung möglich.

## Hilfe und Schulung

Falls diese Guideline das Thema nicht ausreichen erklärt hat bzw. noch weitere Fragen bestehen, stehen
unsere [Ansprechspartner](../readme.md#ansprechpartner) zur Verfügung.

## FAQ

### Was ist, wenn sich der Endkunde nach Adressprüfung für seine ursprüngliche Eingabe entscheidet? Soll ich trotzdem "doAccounting" senden?

Ja. Unabhängig davon, welche Adresse der Kunde am Ende auswählt - wenn endereco die Adresse geprüft hat und die Adresse
danach gespeichert wird, soll ein "doAccounting" gesendet werden.

### Was heißt "gespeichert"?

Als gespeichert gilt eine Adresse dann, wenn sie nach dem Neustart der Applikation oder dem erneuten Laden der Seite weiterhin besteht.

Dabei spielt es keine Rolle, wo genau sie gespeichert ist: ob Cookie, Session-Cookie auf dem Server, Datei,
localStorage im Browser, Datenbank oder sonstiges.

### Wann soll die Session-ID generiert werden?

Die Session-ID soll am Anfang der Eingabe generiert werden, damit alle folgenden Anfragen diese im Header übermitteln können.

### Kann die Session-ID für mehrere Adressen verwendet werden?

Nein. Eine Session-ID soll nur für eine Adresse bzw. für einen Prozess der Eingabe der Adresse verwendet werden.
Im Erfolgsfall soll die Session mit "doAccounting" abgeschlossen werden. Danach soll sie nicht mehr genutzt werden.

### Was passiert mit Sessions, die nicht abgeschlossen wurden?

Solche Sessions werden nach einer Stunde automatisch als "verlassen" markiert. Das erfolgt auf unseren Servern und muss
nicht in einer Integration umgesetzt werden.

Eine Session, die "verlassen" ist, wird nicht in Rechnung gestellt. Wir
vergleichen jedoch die Summen der "abgeschlossenen" und "verlassenen" Sessions, um Anomalien zu erkennen.
