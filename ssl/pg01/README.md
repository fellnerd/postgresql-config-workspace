# SSL-Zertifikat für PostgreSQL mit Let's Encrypt

Diese Anleitung beschreibt, wie das Let's Encrypt-Zertifikat für den PostgreSQL-Server bereitgestellt und eingebunden wird.

## Pfade

- Let's Encrypt Verzeichnis:  
  `/opt/psa/var/modules/letsencrypt/etc/live/pg01.postgres.database.dimetrics.io/`

- PostgreSQL SSL-Verzeichnis:  
  `/etc/postgresql/ssl/pg01/`

## Benötigte Zertifikatsdateien

PostgreSQL erwartet folgende Dateien:

| PostgreSQL-Konfiguration | Beschreibung                | Quelle bei Let's Encrypt                         |
|--------------------------|-----------------------------|--------------------------------------------------|
| `server.crt`             | Server-Zertifikat inkl. Chain | `fullchain.pem`                            |
| `server.key`             | Privater Schlüssel           | `privkey.pem`                                   |
| `server-ca.crt`          | CA-Zertifikate (optional)    | `chain.pem`                                     |

## Schritte zur Bereitstellung

### 1. Zertifikate kopieren

```bash
cp /opt/psa/var/modules/letsencrypt/etc/live/pg01.postgres.database.dimetrics.io/fullchain.pem /etc/postgresql/ssl/pg01/server.crt
cp /opt/psa/var/modules/letsencrypt/etc/live/pg01.postgres.database.dimetrics.io/privkey.pem    /etc/postgresql/ssl/pg01/server.key
cp /opt/psa/var/modules/letsencrypt/etc/live/pg01.postgres.database.dimetrics.io/chain.pem      /etc/postgresql/ssl/pg01/server-ca.crt
```

### 2. Dateiberechtigungen setzen

```bash
chown postgres:postgres /etc/postgresql/ssl/pg01/server.*
chmod 600 /etc/postgresql/ssl/pg01/server.key
chmod 644 /etc/postgresql/ssl/pg01/server.crt /etc/postgresql/ssl/pg01/server-ca.crt
```

### 3. PostgreSQL neu starten

```bash
systemctl restart postgresql
```

### 4. Zertifikat überprüfen (optional)

```bash
openssl x509 -in /etc/postgresql/ssl/pg01/server.crt -noout -enddate
```

Oder Verbindung testen:

```bash
openssl s_client -connect localhost:5432 -starttls postgres
```

## Automatische Erneuerung

Falls Let's Encrypt-Zertifikate automatisch erneuert werden (z. B. per Plesk oder Cron), muss dieser Kopiervorgang ebenfalls automatisiert werden. Ein Cronjob nach erfolgreicher Erneuerung kann dies sicherstellen.
