# PostgreSQL Configuration Workspace

Dieses Repository enthält die PostgreSQL-Konfigurationsdateien und SSL-Zertifikate für den Server `pg01.postgres.database.dimetrics.io`.

## Struktur

### PostgreSQL 14
- `14/main/pg_hba copy.conf` - Backup der Client-Authentifizierungskonfiguration

### PostgreSQL 16
- `16/main/environment` - Umgebungsvariablen für PostgreSQL-Prozesse
- `16/main/pg_ctl.conf` - Automatische pg_ctl-Konfiguration
- `16/main/pg_hba.conf` - Client-Authentifizierungskonfiguration (aktiv)
- `16/main/pg_hba copy.conf` - Backup der Client-Authentifizierungskonfiguration
- `16/main/pg_ident.conf` - PostgreSQL-Benutzername-Mapping
- `16/main/postgresql.conf` - Hauptkonfigurationsdatei
- `16/main/start.conf` - Automatische Startup-Konfiguration
- `16/main/conf.d/` - Verzeichnis für zusätzliche Konfigurationsdateien

### SSL-Zertifikate
- `ssl/pg01/` - SSL-Zertifikate für PostgreSQL
  - `server.crt` - Server-Zertifikat
  - `server.key` - Privater Schlüssel
  - `server-ca.crt` - CA-Zertifikat
  - `README.md` - Anleitung für SSL-Zertifikat-Setup

## Wichtige Konfigurationen

### SSL-Verschlüsselung
PostgreSQL ist für SSL-Verbindungen konfiguriert:
- `ssl = on`
- Let's Encrypt-Zertifikate werden verwendet
- Nur SSL-Verbindungen für externe Clients erlaubt

### Performance-Optimierungen
- `shared_buffers = 12GB`
- `work_mem = 128MB`
- `maintenance_work_mem = 1GB`
- `effective_cache_size = 32GB`
- `max_connections = 200`

### Sicherheit
- Authentifizierung über `scram-sha-256`
- Lokale Verbindungen über Unix-Socket mit `peer`-Authentifizierung
- SSL-Verschlüsselung für alle externen Verbindungen
- Zugriff für 172.*.*.* IP-Bereiche konfiguriert

### Logging
- Verbindungen und Disconnections werden geloggt
- Langsame Queries (>500ms) werden geloggt
- Checkpoint-Aktivitäten werden geloggt

## Wartung

### SSL-Zertifikat erneuern
Folgen Sie der Anleitung in `ssl/pg01/README.md` für die Erneuerung der Let's Encrypt-Zertifikate.

### Konfiguration laden
Nach Änderungen an den Konfigurationsdateien:
```bash
sudo systemctl reload postgresql
# oder
sudo -u postgres pg_ctl reload -D /var/lib/postgresql/16/main
```

### Backup
Dieses Repository dient als Backup der wichtigsten PostgreSQL-Konfigurationsdateien.

## Kontakt
Dimetrics PostgreSQL Server Configuration
Stand: November 2025