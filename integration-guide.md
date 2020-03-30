# Integrationsanleitung

Dieser Guide beschreibt die von uns empfohlene Vorgehensweise bei der Integration der Endereco Service API. Die aktuelle Dokumentation zu einzelnen Funktionen der API findest du hier: [Dokumentation](https://github.com/Endereco/enderecoservice_api)

## Allgemeine Funktionsweise der API

Die  API implementiert das [JSON-RPC2](https://www.jsonrpc.org/specification) Protokoll. Es werden JSON Objekte erwartet und zurückgegeben. Meta-Daten ,darunter auch Zugriffsschlüssel (API-Key) werden im Header übergeben. Ein API-Key gilt zeitlich unbeschränkt und muss nicht erneuert werden. Ein Login ist nicht notwendig -- es reicht den API-Key im Header zu übergeben. 
Die Zuordnung der je API Key aktivieren Services sowie deren Konfiguration erfolgt seitens Endereco, serverseitig.

## Abrechnungsmodelle

Die Services von Endereco stehen kostenpflichtig zur Verfügung. Grundsätzlich gibt es aktuell drei Abrechnungsmodelle:

1. Abrechnung nach tatsächlicher Nutzung (API Requests)
2. Abrechnung nach abgeschloßenen Sessions (Erfolgsbasiert)
3. Pauschale Abrechnung mit Nutzungslimit (Credits)

### Abrechnung nach tatsächlicher Nutzung

Endereco zählt serverseitig wie oft welche Funktion mit welchem API-Key genutzt wird. Anhand dieser Statistik können wir am Ende des Abrechnungszeitraums (i.d.R ein Monat) ermitteln, wieviele Aufrufe eines Services ein Nutzer  (= API Key) hatte. Multipliziert man die Zahl der Aufruf mit den vereinbarten Einzelpreisen, erhält man einen Gesamtpreis.

### Abrechnung nach abgeschloßenen Sessions (erfolgsbasierte Abrechnung)

Mit diesem Modell ermöglichen wir unseren Service "fairer“ zu gestalten D.h. der Kunde wird nur dann abgerechnet, wenn unser Service tatsächlich Mehrwert bringt. Z.b. wenn ein User unseren Service nutzt und sich erfolgreich registriert. Oder im Checkout Prozess eines Webshops seine Daten aktualisiert.

Eine Session unterteilt sich in mehrere Typen oder -- bei uns intern -- auch Pakete genannt. Diese Pakete haben ihre Standardpreise. Beispiel: Das Paket "Adressprüfung+Eingabeassistent" wird mit ~0.10€ pro abgeschloßene Session berechnet.

Bei diesem Abrechnungsmodell muss eine Session vor der Nutzung gestartet werden und nach dem Erfolg (Registrierung, nächster Schritt im Checkout, Bestellung etc) mit einer speziellen Anfrage abgeschloßen werden.

Die Nutzung dieses Modells muss bei individueller Implementierung mit uns abgesprochen und abgenommen werden. Wenn man zum Beispiel eine Datei mit 10.000 Adressen innerhalb einer session prüfen möchte, eignet sich dieses Modell nicht.

### Pauschale Abrechnung mit Nutzungslimit (Credits)

Das einfachste Model. Man zahlt einen Betrag X und hat dafür Y Anfragen in einem bestimmten Zeitraum (i.d.R. ein Jahr) zur Verfügung. Die Anfragen zählen wir Serverseitig. Wenn das Limit erschöpft ist, schließen wir die API. Die Budgets werden individuell vereinbart und hinterlegt.

## Implementierung

Die Implementiert kann in nahezu jeder Programmiersprache erfolgen, die es erlaubt HTTP Anfragen zu formulieren. Formulierbar sollen sowohl das body der Anfrage, als auch die Headers sein sollen. In dieser Anleitung gebe ich Code-Beispiele für PHP7 und die Bibliothek [Guzzle](http://docs.guzzlephp.org/en/stable/index.html).

Grundsätzlich sieht das Vorgehen wie folgt aus: 

1. Man baut ein JSON auf
2. Man schickt es an die Endereco API
3. Man erhält ein JSON zurück

### Abrechnung nach tatsächlicher Nutzung

Um im Rahmen von diesem Modell unseren Service zu nutzen, müssen die Anfragen einfach an uns geschickt werden.

Zum Beispiel ist für den API-Key `123456789asdfgh1234567` ein postCodeAutocomplete Service Freigegeben. In der kommenden Anfrage wollen wir wissen welche Autocomplete Varianten wird für die Teil-Postleitzahl `972` bekommen.

Laut [Doku](https://github.com/Endereco/enderecoservice_api/blob/master/functions/postcode-autocomplete.md) muss die JSON der Anfrage so aussehen:

Body:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "postCodeAutocomplete",
  "params": {
    "country": "de",
    "language": "de",
    "postCode": "972"
  }
}
```

Headers
```
Content-Type: application/json
X-Auth-Key: 123456789asdfgh1234567
X-Transaction-Id: not_required
```

*X-Auth-Key* ist dabei der API-Key und *X-Transaction-Id* ist für die Session ID reserviert. Nutzt man keine so empfehlen wir an der Stelle "not_required" zu setzen, dadurch überspring unser Server einige sessionbezogene Schritte und gibt die Antwort schneller zurück.

Eine solche Anfrage könnte man in PHP7 so implementieren:

```php
use \GuzzleHttp\Client;
use \GuzzleHttp\Psr7\Request;

$message = array(
    'jsonrpc' => '2.0',
    'id' => 1,
    'method' => 'postCodeAutocomplete',
    'params' => array(
        'country' => 'de',
        'language' => 'de',
        'postCode' => '972',
    ),
);

$client = new Client(array('timeout' => 3.0));
$headers = array(
    'Content-Type' => 'application/json',
    'X-Auth-Key' => '123456789asdfgh1234567',
    'X-Transaction-Id' => 'not_required',
);

$request = new Request('POST', 'https://endereco-service.de/rpc/v1', $headers, json_encode($message));
$response = $client->send($request);
$response_array = json_decode($response->getBody()->getContents(), true);
```

### Abrechnung nach abgeschlossenen Sessions

Bei diesem Modell ist die Vorgehensweise ähnlich wie bei Abrechnung nach Nutzung, nur muss zuerst die Session gestartet werden. Dafür steht die Funktion [startSession](https://github.com/Endereco/enderecoservice_api/blob/master/functions/start-session.md) zur Verfügung.

Nach der Ausführung dieser Funktion wird bei uns intern eine Session registriert, deren ID wird zurückgeben. Nun muss diese ID mit dem header `X-Transaction-Id` bei jeder kommenden Anfrage angegeben werden. Dadurch können wir eingehende Anfragen der Session zuordnen.

Am Ende der Session soll sie abgeschlossen werden. Dafür steht die Funktion [doAccounting](https://github.com/Endereco/enderecoservice_api/blob/master/functions/do-accounting.md) zur Verfügung. Auch bei *“doAccounting“* muss die Session ID übergeben werden. Wenn wir eine doAccouting-Anfrage erhalten, markieren wir die entsprechende Session als abgeschlossen. Die ID darf ab jetzt auch lokal nicht mehr verwendet werden. Eine Session die seit einiger Zeit (zZ 6 Stunden) kein doAccounting bekommt, wird als "verlassen" markiert. Ist quasi abgeschloßen, wird aber nicht abgerechnet.

### Pauschale Abrechnung mit Nutzungslimit

Die Implementierung ist identisch zu **Abrechnung nach Tatsächlicher Nutzung**. Wenn der Limit erschöpft ist, gibt unsere API eine Fehlermeldung "Ihr Konto ist nicht mehr aktiv" aus.
