# Referrer übermitteln

Für uns ist es relevant zu wissen, woher die Anfragen kommen. Das betrifft sowohl die Anfragen aus dem Browser als
auch serverseitige Anfragen oder Anfragen aus einer App.

Wir können damit herausfinden, ob irgendjemand unberechtigt den API-Key nutzt. Bei einem Softwarefehler können wir
unsere Abrechnung anpassen, um Fehler in der Nutzungserfassung zu korrigieren.

Es ist für uns essentiell wichtig, dass jede Implementierung uns den Referrer mitteilt.

Dabei sind folgende Punkte zu beachten:

- Der Referrer muss anonym sein und darf keinesfalls persönliche Daten enthalten.
- Der Referrer soll nicht zu lang sein. Die Länge des Textes im Referrer soll ausreichend sein, um die Erfassungsstelle
  in der Website/App zu identifizieren.
- GET-Parameter sollen auf jedem Fall entfernt werden. Falls über GET-Parameter geroutet wird, sollen sie vor der
  Übermittlung in den Text des Referrers eingefügt werden, aber nicht als GET-Parameter.
- Es soll nicht möglich sein, die Anfragen zu unterschiedlichen Datensätzen über Referrer zusammenzuführen.

## Der Referrer muss anonym sein und darf keinesfalls persönliche Daten enthalten.

Manche Systeme können IDs von Kunden oder Kundenadressen enthalten.

``` 
https://www.example.com/myapp/users/1/addresses/3
```

Wenn wir eine URL wie im Beispiel bei jeder Anfrage erhalten würden, könnten wir die Anfragen zu der Person mit Id 1
zuordnen. Das wäre ein Verstoß gegen unsere Datenschutzbestimmungen und eine unrechtmäßige Verarbeitung der
personenbezogenen Daten.

Deswegen soll die Domain vor dem Absenden zur endereco ServicesAPI in folgendes umgewandelt werden:

``` 
https://www.example.com/myapp/users/x/addresses/x
```

## Der Referrer soll nicht zu lang sein.

Wir speichern die Anfragen dauerhaft, um später abrechnugsrelevante Statistiken zu erstellen. Zu lange Texte verursachen
einen großen Aufwand bei der Datenbankanfragen, wenn man mit Mengen von über 300 Mio. Einträgen arbeitet.

Beispiel:

``` 
https://www.example.com/loremipsum-this-and-taht/usemenegementsystemsetc/users/1314/addresses/3312/?utm_md=123123123&acf_d=15415512351c123
```

Mit einem solchen Referrer hat man zu viel Information.

Kürzere Variante:

``` 
https://www.example.com/loremipsum-this-and-taht/usemenegementsystemsetc/users/x/addresses/x/
```

- GET-Parameter entfernt
- IDs mit "x" ersetzt

## GET-Parameter sollen auf jedem Fall entfernt werden

GET Parameter enthalten öfters Tracking IDs oder sonstige Daten, die für die endereco Services-API nicht relevant und aus
Datenschutzsicht kritisch zu betrachten sind.

``` 
https://www.example.com/myapp/users/x/addresses/x?utm_id=k1dk212d8
```

Sie sollen auf jeden Fall entfernt werden.

``` 
https://www.example.com/myapp/users/x/addresses
```

Manche Applikationen nutzen GET-Parameter für Routing.

``` 
https://www.example.com/index.php?cl=my-account
```

In diesem Fall ist "my-account" für die enderecoServices-API interessant, jedoch soll der GET-Parameter trotzdem entfernt
werden. Den Namen des Controllers kann man in diesem Fall behalten.

``` 
https://www.example.com/index.php/my-account
```

## Es soll nicht möglich sein, die Anfragen zu unterschiedlichen Datensätzen über Referrer zusammenzuführen

Die endereco Service-API verarbeitet Namen, Rufnummern, E-Mail Adressen und Lieferadressen getrennt voneinander. Es soll
technisch nicht möglich sein, sie zusammenzuführen. Der Referrer ist dabei eine Schwachstelle.

``` 
https://www.example.com/order/7023x/address
```

Hier lassen sich über die Order-ID (auch wenn sie teilweise anonymisiert erscheint) die Anfragen zusammenführen.

Gewünscht wäre folgendes:

``` 
https://www.example.com/order/x/address
```
