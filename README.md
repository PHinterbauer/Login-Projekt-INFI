# Login-Projekt

[![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.x-000000?logo=flask&logoColor=white)](https://flask.palletsprojects.com/)
[![PHP](https://img.shields.io/badge/PHP-8.x-777BB4?logo=php&logoColor=white)](https://www.php.net/)
[![SQLite](https://img.shields.io/badge/SQLite-3-003B57?logo=sqlite&logoColor=white)](https://www.sqlite.org/)
[![License](https://img.shields.io/badge/License-Educational-lightgrey)](#lizenz-und-projektstatus)

Eine einfache, aber vollständige Login-Anwendung mit **Flask**, **PHP** und **SQLite**.
Das Projekt enthält zwei eigenständige Versionen (Python und PHP), die Registrierung,
Login, Passwort-Reset und drei wählbare Sicherheitsstufen bieten.

---

## ✨ Funktionen

| Stufe | Anmeldung |
| --- | --- |
| 1 | E-Mail-Adresse und Passwort |
| 2 | Zusätzlich ein Code per E-Mail |
| 3 | Zusätzlich ein TOTP-Code aus einer Authenticator-App |

Weitere Funktionen:

- ⏱️ Sitzungen laufen nach **15 Minuten** ab.
- 📧 E-Mail-Codes sind **5 Minuten** gültig.
- 🔐 TOTP-Codes sind **30 Sekunden** gültig.
- 🔗 Passwort-Reset-Links sind **15 Minuten** gültig.
- 🎨 Die Login-Seite bietet mehrere Designs (u. a. **Bier** und **Käse**).
- 🗑️ Alle gespeicherten Login-Daten können im Dashboard gelöscht werden.

---

## 🧰 Voraussetzungen

**Python-Version**

- Windows mit Python 3.11 oder neuer
- Terminal (Eingabeaufforderung oder PowerShell)
- Internetverbindung für die einmalige Installation der Pakete

**PHP-Version**

- XAMPP mit Apache
- PHP mit den Erweiterungen `pdo_sqlite` und `sqlite3`

---

## 📁 Projektstruktur

```
Gruber_Login/
├── app.py                  # Flask-Anwendung
├── requirements.txt        # Python-Abhängigkeiten
├── config.example.ini      # Vorlage für die Konfiguration
├── static/
│   └── style.css           # Designs der Python-Anwendung
├── php/                    # PHP-Anwendung (XAMPP)
│   ├── config.php
│   ├── index.php
│   ├── register.php
│   ├── dashboard.php
│   ├── email-code.php
│   ├── otp.php
│   ├── reset.php
│   ├── reset-password.php
│   ├── logout.php
│   └── .htaccess
└── README.md
```

> `config.ini`, `login.sqlite3` und der Ordner `.venv/` werden nicht versioniert.

---

## 🐍 Python-Version starten

### 1. Projektordner öffnen

```cmd
cd "C:\Pfad\zum\Gruber_Login"
```

### 2. Virtuelle Umgebung erstellen

```cmd
py -m venv .venv
```

### 3. Umgebung aktivieren

Eingabeaufforderung:

```cmd
.venv\Scripts\activate
```

PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### 4. Abhängigkeiten installieren

```cmd
python -m pip install -r requirements.txt
```

### 5. Anwendung starten

```cmd
python app.py
```

Der lokale Webserver läuft anschließend auf Port `5000`. Öffne im Browser:

<http://127.0.0.1:5000>

Zum Beenden drücke `Strg+C`. Die virtuelle Umgebung verlässt du mit:

```cmd
deactivate
```

> Falls PowerShell das Aktivieren blockiert, starte die App direkt mit dem Interpreter der virtuellen Umgebung:
>
> ```powershell
> .venv\Scripts\python.exe app.py
> ```

---

## ⚙️ Konfiguration

Die Datei `config.ini` enthält lokale Einstellungen wie den Flask-Schlüssel und die
SMTP-Zugangsdaten. Sie wird nicht in Git gespeichert. Erstelle sie einmalig aus der Vorlage:

```cmd
copy config.example.ini config.ini
```

Passe anschließend mindestens diese Werte an:

```ini
[app]
flask_secret = ein-langes-zufaelliges-geheimnis
port = 5000

[mail]
host = smtp.example.com
port = 587
security = starttls
user = deine-adresse@example.com
password = dein-app-passwort
from = deine-adresse@example.com
```

Für Sicherheitsstufe 2 und Passwort-Reset werden gültige SMTP-Daten benötigt.
Bei vielen Mail-Anbietern ist dafür ein App-Passwort erforderlich.

**Wichtig:** `config.ini`, SMTP-Passwörter und geheime Schlüssel dürfen nicht
veröffentlicht oder in Git eingecheckt werden.

Die Anwendung startet standardmäßig unter `http://127.0.0.1:5000`. Der Port
kann im Abschnitt `[app]` geändert werden. Für lokale Tests kann
`wipe_database_on_start = true` verwendet werden; dadurch wird die Python-
Datenbank bei jedem Start gelöscht. Diese Einstellung darf im normalen
Betrieb nicht aktiviert sein.

---

## 🐘 PHP-Version mit XAMPP

1. Kopiere das Projekt nach `C:\xampp\htdocs\login`.
2. Starte Apache im XAMPP Control Panel.
3. Aktiviere in `php.ini` die Erweiterungen `pdo_sqlite` und `sqlite3`.
4. Starte Apache nach einer Änderung an `php.ini` neu.
5. Konfiguriere den Mail-Versand über XAMPP/Sendmail.
6. Öffne <http://127.0.0.1/login/php/>.

Die Python- und PHP-Version verwenden getrennte SQLite-Datenbanken. Ein Konto
aus der Python-Version ist daher nicht automatisch in der PHP-Version vorhanden.

---

## ✅ Empfohlener Testablauf

1. Starte die Python-Version mit Sicherheitsstufe 1.
2. Lege ein Konto an und teste Login und Logout.
3. Stelle im Dashboard Sicherheitsstufe 2 ein und prüfe den E-Mail-Code.
4. Teste die Passwort-Zurücksetzung mit einem gültigen SMTP-Konto.
5. Verwende für Sicherheitsstufe 3 eine Authenticator-App und teste den angezeigten TOTP-Schlüssel.
6. Prüfe, dass eine Sitzung nach Ablauf des Zeitlimits nicht mehr gültig ist.

---

## 🔒 Sicherheit

Die Anwendung speichert keine Klartextpasswörter. Passwörter werden vor der
Speicherung gehasht. E-Mail-Codes, TOTP-Codes und Reset-Links sind zeitlich
begrenzt. Für den produktiven Einsatz sollten zusätzlich HTTPS, Rate-Limiting,
sichere Cookie-Einstellungen und eine geschützte Serverkonfiguration verwendet
werden.

---

## 📄 Lizenz und Projektstatus

Dieses Projekt ist eine Schul-/Lernanwendung. Vor einem produktiven Einsatz
sollten Konfiguration, Sicherheit und Fehlerbehandlung nochmals geprüft werden.