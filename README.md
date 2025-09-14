# 📧 Mailserver Setup (Portfolio)

Dieses Repository dokumentiert mein eigenes Mailserver-Setup für E-Mail-Kommunikation.  
Es dient als Referenzprojekt für mein Portfolio im Bereich **Systemadministration, IT-Security und Self-Hosting**.  

---

## ⚙️ Architekturübersicht

- **E-Mail-Gateway:** Proxmox Mail Gateway (PMG)  
- **Mailserver:** Mailcow (dockerized)  
- **Virtualisierung:** Proxmox VE  
- **Netzwerk:** Dedizierter Server mit Reverse Proxy + Firewall  

### Aufbau

```
Internet
   │
   ▼
[ Proxmox Mail Gateway ]
   │   (Spam- und Virenfilter, Greylisting, RBLs)
   ▼
[ Mailcow ]
   │   (Postfix + Dovecot, SOGo, Rspamd, ClamAV, ACME)
   ▼
Clients (IMAP/SMTP, Webmail)
```

---

## 🛠️ Eingesetzte Komponenten

### 🔹 Proxmox Mail Gateway (PMG)
- Läuft als **Reverse Proxy / SMTP-Relay** vor Mailcow  
- Filtert Spam & Malware mit:
  - RBLs (Realtime Blackhole Lists)  
  - Greylisting  
  - ClamAV  
  - SpamAssassin  
- Quarantäne-Portal für Benutzer  

### 🔹 Mailcow (dockerized)
- **Postfix** → SMTP für ausgehende/ eingehende Mails  
- **Dovecot** → IMAP/POP3 für Mailzugriff  
- **Rspamd + ClamAV** → Spam- und Virenprüfung  
- **SOGo** → Webmail mit Kalender & Kontakte  
- **ACME/LetsEncrypt** → Automatische TLS-Zertifikate  
- **Fail2Ban** → Schutz vor Brute-Force  

---

## 🔑 DNS-Konfiguration

Für meine Domain sind folgende Records gesetzt:

| Record | Typ  | Wert                                          |
|--------|------|-----------------------------------------------|
| MX     | 10   | `mail.meinedomain.de`                        |
| A      |      | `123.45.67.89`                               |
| TXT    | SPF  | `"v=spf1 mx -all"`                           |
| TXT    | DKIM | Schlüssel von Mailcow                         |
| TXT    | DMARC| `"v=DMARC1; p=quarantine; rua=mailto:dmarc@meinedomain.de"` |

---

## 🛡️ Sicherheitsfeatures

- **TLS-Zwang** für SMTP/IMAP  
- **DKIM, SPF, DMARC** für Absenderauthentifizierung  
- **Fail2Ban & Firewall** gegen Angriffe  
- **Spam- und Virenfilterung** über PMG & Rspamd  
- **Getrennte Rollen**: PMG als vorgeschaltetes Gateway, Mailcow als internes Mailsystem  

---

## 📬 Zugriffsmöglichkeiten

- **Webmail (SOGo):** `https://mail.meinedomain.de`  
- **SMTP/IMAP:** TLS-verschlüsselt auf Port 587/993  
- **PMG-Portal:** `https://pmg.meinedomain.de` (nur für Admins & Quarantäne-User)  

---

## 📖 Zweck

Dieses Setup demonstriert meine Fähigkeiten in den Bereichen:  

- Linux-Serveradministration  
- Virtualisierung mit Proxmox  
- Mailserver-Sicherheit (TLS, SPF/DKIM/DMARC, Spamfilter)  
- Automatisierung mit Docker & Compose  
- Netzwerkarchitektur für produktive Dienste  

---

## 📝 Lizenz

Diese Dokumentation dient ausschließlich als Referenzprojekt (Portfolio).  
