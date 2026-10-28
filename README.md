# Nextcloud MCP Server

Lokaler MCP-Server für den Zugriff auf Nextcloud aus Codex oder einem anderen
MCP-Client.

## Voraussetzungen

- Podman mit Compose-Unterstützung oder Docker Compose
- eine laufende Nextcloud-Instanz
- ein Nextcloud-App-Passwort

## Einrichtung

Vorlage kopieren und die eigenen Werte eintragen:

```bash
cp .env.example .env
$EDITOR .env
```

Benötigte Variablen:

```dotenv
NEXTCLOUD_HOST=https://cloud.example.tld
NEXTCLOUD_USERNAME=benutzername
NEXTCLOUD_PASSWORD=app-passwort
```

`.env` enthält vertrauliche Daten und darf nicht in Git eingecheckt werden.

## Starten und verbinden

```bash
podman compose up -d
podman compose ps
```

Der lokale MCP-Endpunkt ist anschließend unter `http://127.0.0.1:8000/mcp`
erreichbar. Mit Docker lautet der Präfix entsprechend `docker compose`.

```bash
codex mcp add nextcloud --url http://127.0.0.1:8000/mcp
```

Der Port ist standardmäßig nur lokal erreichbar. Bei einer Änderung muss der
Zugriff zusätzlich durch Firewall oder Reverse Proxy geschützt werden.

## Betrieb und Fehlerdiagnose

```bash
podman compose logs --tail=100 nextcloud-mcp
podman compose down
podman compose pull
podman compose up -d
```

Bei `401 Unauthorized` Host, Benutzername und App-Passwort prüfen. Bei
Verbindungsfehlern zuerst den Containerstatus und die Erreichbarkeit von
`NEXTCLOUD_HOST` prüfen.

Das verwendete Container-Image stammt aus dem Projekt
[cbcoutinho/nextcloud-mcp-server](https://github.com/cbcoutinho/nextcloud-mcp-server).

Weitere Projekte findest du auf [pixeledi.eu](https://pixeledi.eu).
