# Struktur der Schnittstelle

Die Grundstruktur der JSON folgt der [JSON-RPC 2.0 Spezifikation](https://www.jsonrpc.org/specification).

Es muss im JSON-Format formuliert werden. Die Antwort entspricht ebenfalls dem JSON-Dateiformat. Die JSON wird im Body der HTTP Anfrage übermittelt.

Für die Übermittlung ist HTTPS (Port 443) zu setzen.

Aus Datenschutzgründen erlauben wir nur eine Server-to-Server-Kommunikation. Direkte Anfragen sind nicht erlaubt. So
vermeiden wir es, personenbezogene Daten des Endnutzers zu erhalten, die wir nicht brauchen.

Jedoch ist die Spezifikation nicht vollständig umgesetzt. Unter anderem gibt es aktuell keine Möglichkeit, mehrere
Datensätze (Bulk-Processing) zu übermitteln, und die Fehlercodes sind rudimentär und sollten in einer Integration nicht
verarbeitet werden.

| Feld    | Erwarteter Wert     | Bedeutung                                                                                                                                                    |
|---------|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| jsonrpc | "2.0" als String    | Mit diesem Feld wird die Version des Protokolls angegeben. Der Wert ist immer gleich.                                                                        |
| id      | Zahl größer als 0   | Mit diesem Feld wird die ID der Anfrage übergeben. Falls mehrere Anfragen asynchron versendet werden, kann über die ID die Sequenz wiederhergestellt werden. |
| method  | String              | Name [einer der Funktionen](./readme.md#übersicht-der-funktionen) der Endereco Services API, z.B. "addressCheck"                                             |
| params  | Hash-Array / Object | Eine Sammlung der methodenspezifischen Parameter                                                                                                             |
| result  | Hash-Array / Object | Container für die Antwort bei erfolgreichem Verlauf der Anfrage.                                                                                             |
| error   | Hash-Array / Object | Container für die Fehlermeldung beim fehlerhaften Verlauf der Anfrage.                                                                                       |
| code    | Zahl                | Nummer des Fehlers. Aktuell rudimentär umgesetzt und soll ignoriert werden.                                                                                  |
| message | String              | Fehlermeldung in menschenlesbarer Form.                                                                                                                      |


## Url

```
POST https://endereco-service.de/rpc/v1
```

## Headers

| Feld                  | Bedeutung                                                                                        |
|-----------------------|--------------------------------------------------------------------------------------------------|
| Content-Type          | application/json                                                                                 |
| X-Transaction-Id      | not_required, siehe [Generierung der Session ID's](./guidlines/sessions-guideline.md)            |
| X-Agent               | MyClient v1.0.0, siehe [Client ID Guideline](./guidlines/client-id-guideline.md)                 |
| X-Transaction-Referer | www.example.de/register, siehe [Referrer übergeben](./guidlines/providing-referrer-guidlines.md) |
| X-Auth-Key            | siehe [Authentifizierung](./readme.md#authentifizierung)                                         |
