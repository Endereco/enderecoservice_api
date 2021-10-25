# Liste aller Felder und ihre Interpretation

## Inhaltsverzeichnis

- [Grundaufbau der JSON](#grundaufbau-der-json)
- [Eingabeparameter](#eingabeparameter)
- [Ausgabenfelder](#ausgabenfelder)

## Grundaufbau der JSON

Der grundsätzliche Aufbau der JSON folgt aus der [JSON-RPC 2.0 Spezifikation](https://www.jsonrpc.org/specification).
Die Spezifikation ist jedoch nicht vollständig umgesetzt. Unter anderem gibt es aktuell keine Möglichkeit 
mehrere Datensätze zu übermitteln (Bulk-Processing) und die Error-Codes sind rudimentär und können

| Feld | Erwarteter Wert | Bedeutung |
| --- | --- | --- |
| jsonrpc | "2.0" als String | Mit diesem Feld wird die Version des Protokolls angegeben. Der Wert ist immer gleich. |
| id | Zahl größer als 0 | Mit diesem Feld wird die ID der Anfrage übergeben. Falls man mehrere Anfragen asynchron versendet, kann über die ID die Sequenz wiederhergestellt werden. |
| method | String | Name [einer der Funktionen](./readme.md#verzeichnis-der-methoden-und-use-cases) der Endereco Services API, z.B. "addressCheck" |
| params | Hash-Array / Object | Eine Sammlung der methodenspezifischen Parameter |
| result | Hash-Array / Object | Container für die Antwort bei erfolgreichen Verlauf der Anfrage. |
| error | Hash-Array / Object | Container frü die Fehlermeldung beim fehlerhaften Verlauf der Anfrage. |
| code | Zahl | Nummer des Fehlers. Aktuell rudimentär umgesetzt und soll ignoriert werden. |
| message | String | Fehlermeldung in menschenlesbarer Form. |

## Eingabeparameter

| Feld | Erwarteter Wert | Bedeutung |
| --- | --- | --- |
| | | |

## Ausgabenfelder

| Feld | Erwarteter Wert | Bedeutung |
| --- | --- | --- |
| | | |
