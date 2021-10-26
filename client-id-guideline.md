# Client ID Guideline

Neben sonstigen Metadaten, soll auch die Kennung des Clients bei jeder Anfrage übermittelt werden.

Der Name ist wie folgt aufzubauen:

`[NAME DES CLIENTS] [VERSION]`

Der NAME soll die Integration eindeutig beschreiben, aber möglichst kurz bleiben.

Die VERSION soll in [Semver Syntax](https://semver.org/) geschrieben werden.

Zum Beispiel:
- `WaWi-Intergration v.1.0.1`
- `Endereco Shopware5 Client v5.0.0`
- `StorageApp(Android) 0.1.0-rc.1`

Wir nutzen die Kennung des Clients bei Problemdiagnosen.

## Soll auch die Version des Systems oder Themes angegeben werden?

Nein. Für Systemname und -version, sowie für das verwendete Theme, wird es zukünftig andere Möglichkeiten geben.
