# Das erfolgsbasierte Abrechnungsmodell - Umsetzung

WIP - diese Dokumentation ist aktuell in Arbeit. Bei Fragen melden Sie sich bei ilja@endereco.de.

## Wieso?
Wir rechnen nur dann ab, wenn eine geprüfte Adresse genutzt wird, d.h der Kunde im Checkout oder bei der Registrierung weiter kommt.
Somit trägt seine zukünftige Bestellung den Preis der Adressprüfung.

Aus unserer Sicht ist das fair gegenüber dem Händler

## DSGVO
Die erfolgbasiete Abrechnung setzt ein Tracking voraus. Man müsste wissen, dass diese Serie der Anfragen, die an der API akommen, mit 
der späteren Bestellung/Registrierung verknüpft sind.

Da wir den Prinzip der äußersten und kompromislosen Datensparsamkeit ausüben, haben wir uns folgende Lösung überlegt.

## Technische Umsetzung

### Vorbereitung 
Während der Eingabe der Adresse erstellen wir dynamisch ein Paar unsichtbaren Feldern, die auf eine folgende Art und Weise benannt werden:

*prefix_session_id*

*prefix_session_counter*

Das "prefix" kann dabei mit einem Service-Kennzeichen oder irgendeinem anderen Text ersetzt werden. Wichtig ist die Endung.

Nachdem diese unsichtbare Felder erstellt wurden, bekommt das Feld mit Endung "_session_counter" einen Wert 0 und das Feld mit Endung "_session_id" einen zufälligen UUID v4.

Wenn während der Eingabe der Adresse eine Antwort von Endereco Services zurückkommt, wird der Counter um 1 erhöht.

Dabei unterteilen wir unsere Services in folgende Gruppen: Emailprüfung, Adressprüfung/Splitstreet/Eingabeassistent, Anredeprüfung, Telefonnummernormierung.

Jede Gruppe soll eine eigene einzigartige ID haben.

### Nach dem Absenden

Ist der Nutzer mit seiner Eingabe fertig und sendet die Form ab, lesen wir serverseitig den Inhalt der abgesendeten Daten und probieren darin Varaiblenpaare mit Endung "_session_id" bzw. "_session_counter" zu finden.

Falls gefunden, wird bei jedem Paar in "_session_counter" geprüft, ob der Wert größer 0 ist. Wenn es größer 0 ist, heißt das, dass im Frontend Endereco Service genutzt wurde und die Session ID somit abrechenbar wäre. In diesem Fall wird für jede abrechenbare Session ID ein doAccounting Anfrage erstellt.


Sessions mit Counter = 0 werden ignoriert.

Wichtig ist, dass diese Logik vor der serverseitigen Validation ausgeführt ist, da sonst die Session ID verloren geht.

### AJAX Formen

Falls man die Form mit AJAX absendet, sollen nach jedem Absenden die Session ID und Counter erneuert werden, als ob die Seite neugeladen wäre.


