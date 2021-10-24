# Das erfolgsbasierte Abrechnungsmodell - Umsetzung

WIP - diese Dokumentation ist aktuell in Arbeit. Bei Fragen melden Sie sich unter support@endereco.de.

## Wieso?
Wir rechnen eine Prüfung optional nur dann ab, wenn z.B. eine geprüfte Adresse genutzt werden kann, d.h z.B: der Kunde im Checkout oder bei der Registrierung weiter kommt oder eine Adresse gespeichert wurde. Diese Abrechnungsweise ist transparten und an einen Erfolg gekoppelt und damit leichter kalkulierbar.

## DSGVO
Die erfolgbasiete Abrechnung setzt ein Tracking der Anfragen voraus. Wir müssen erkennnen, dass eine Serie der Anfragen, die an die API übermittelt werden, zusammengehörten und zur gleichen Transaktion gehören. 

Da wir den Prinzip der äußersten und kompromislosen Datensparsamkeit ausüben, haben wir uns folgende Lösung überlegt.

## Technische Umsetzung

### Vorbereitung 
Während der Eingabe der Adresse auf einer Seite erstellen wir dynamisch unsichtbare Felder, die wie folgt benannt werden:

*prefix_session_id*
*prefix_session_counter*

Das "prefix" kann dabei mit einem Service-Kennzeichen oder anderen Text ersetzt werden. Wichtig ist die Endung.

Nachdem diese unsichtbare Felder erstellt wurden, wird dem Feld mit Endung "_session_counter" der Wert 0 und dem Feld mit Endung "_session_id" eine zufällige UUID v4 zugewiesen.

Wenn während der Eingabe der Adresse eine Antwort von Endereco Services zurückkommt, wird der Counter um 1 erhöht.

Dabei unterteilen wir unsere Services in folgende Gruppen: 

Emailprüfung, 
Adressprüfung/
Splitstreet/
Eingabeassistent, 
Anredeprüfung, 
Telefonnummernormierung.

Jede Gruppe muss eine eindeutige ID erhalten.

### Nach dem Absenden

Ist der Nutzer mit seiner Eingabe fertig und sendet das Formular ab, lesen wir serverseitig den Inhalt der abgesendeten Daten aus und ermitteln darin enthaltene Varaiblenpaare mit den Endungen "_session_id" bzw. "_session_counter".

Für jedem Paar wird im Feld "_session_counter" geprüft, ob der Wert größer 0 ist. Wenn der Wert > 0 ist, wurden im Frontend die Endendereco Service genutzt.
Somit können wir diese Session ID als abrechenbar kennzeichnen. In diesem Fall wird für jede abrechenbare Session ID ein doAccounting Anfrage erstellt.


Sessions mit Counter = 0 werden ignoriert.

Wichtig ist, dass diese Logik vor der serverseitigen Validation ausgeführt ist, da sonst die Session ID verloren geht.

### AJAX Formen

Falls man das Formular per AJAX absendet, sollen nach jedem Absenden Session ID und Counter erneuert werden, wie bei einem neuladen der Seite.


