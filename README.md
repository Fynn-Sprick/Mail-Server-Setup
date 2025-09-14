# 📧 Mailserver Setup

Dieses Repository dokumentiert mein selbst gehostetes Mailserver-Setup.  
Ziel: Ein sicherer, zuverlässiger und moderner Mailserver mit Unterstützung für IMAP, SMTP, TLS, DKIM, SPF und DMARC.  

---

## ⚙️ Architektur

- **Mailserver-Software:** [Mailcow: dockerized](https://mailcow.email/)  
- **Containerisierung:** Docker & Docker Compose  
- **DNS / Records:**  
  - SPF, DKIM, DMARC  
  - MX-Records  
  - Reverse DNS  
- **Sicherheit:**  
  - TLS-Zertifikate (Let's Encrypt)  
  - Spamfilter & Virenscanner (Rspamd, ClamAV)  
  - Fail2Ban für Schutz vor Brute-Force-Angriffen  

---

## 🗂️ Komponenten

| Dienst        | Aufgabe                                                                 |
|---------------|------------------------------------------------------------------------|
| **Postfix**   | SMTP-Server für ausgehende und eingehende Mails                        |
| **Dovecot**   | IMAP/POP3-Server für Postfächer                                       |
| **Rspamd**    | Spam- und Virenfilter                                                  |
| **ClamAV**    | Virenscanner                                                           |
| **Redis**     | Caching für Rspamd                                                     |
| **MariaDB**   | Datenbank für Mailcow                                                  |
| **SOGo**      | Webmail, Kalender und Kontakte                                         |
| **ACME**      | Let's Encrypt Integration für TLS-Zertifikate                          |

---

## 🚀 Installation

### 1. Repository klonen
```bash
git clone https://github.com/<dein-user>/<dein-repo>.git
cd <dein-repo>
```

### 2. Mailcow herunterladen
```bash
git clone https://github.com/mailcow/mailcow-dockerized
cd mailcow-dockerized
```

### 3. Konfiguration erstellen
```bash
./generate_config.sh
```

- Domain eingeben (z. B. `mail.deinedomain.de`)  
- IP-Adresse des Servers angeben  

### 4. Mailcow starten
```bash
docker compose pull
docker compose up -d
```

---

## 🔑 DNS-Konfiguration

Beispiel für `example.com`:  

| Record | Typ  | Wert                                          |
|--------|------|-----------------------------------------------|
| MX     | 10   | `mail.example.com`                           |
| A      |      | `123.45.67.89`                               |
| TXT    | SPF  | `"v=spf1 mx ~all"`                           |
| TXT    | DKIM | Schlüssel von Mailcow                         |
| TXT    | DMARC| `"v=DMARC1; p=quarantine; rua=mailto:dmarc@example.com"` |

---

## 🛡️ Sicherheit & Best Practices

- Immer **TLS erzwingen** (SMTP, IMAP, POP3)  
- **Fail2Ban aktivieren** für Schutz gegen Brute-Force  
- Regelmäßig Updates einspielen (`docker compose pull && docker compose up -d`)  
- Backups für `/var/lib/docker/volumes/mailcowdockerized_*` einrichten  

---

## 📬 Testen

- MX- und SPF-Records prüfen: [MXToolbox](https://mxtoolbox.com/)  
- DKIM testen: `dig TXT default._domainkey.example.com`  
- Mail-Server testen: [Mail Tester](https://www.mail-tester.com/)  

---

## 📖 Lizenz

Dieses Setup basiert auf [Mailcow](https://mailcow.email/).  
Eigene Anpassungen unter MIT-Lizenz.  
