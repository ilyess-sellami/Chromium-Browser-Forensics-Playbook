# Chromium Browser Forensics Playbook

Chromium-based browsers such as **Google Chrome**, **Microsoft Edge**, **Brave**, and **Opera** store valuable forensic artifacts including browsing history, downloads, cookies, credentials, sessions, extensions, and cache data.

This playbook provides a practical DFIR approach for collecting and analyzing Chromium browser artifacts during incident response, forensic investigations, CTFs, and security labs.

---

## 00 - Evidence Collection

#### Windows

```bash
%LOCALAPPDATA%\Google\Chrome\User Data\Default\
%LOCALAPPDATA%\Microsoft\Edge\User Data\Default\
%LOCALAPPDATA%\BraveSoftware\Brave-Browser\User Data\Default\
```

#### Linux

```bash
~/.config/google-chrome/
~/.config/microsoft-edge/
~/.config/BraveSoftware/Brave-Browser/
```

#### MacOS

```bash
~/Library/Application Support/Google/Chrome/
~/Library/Application Support/Microsoft Edge/
~/Library/Application Support/BraveSoftware/Brave-Browser/
```

#### Common Chromium Profile Files

| File            | Purpose                      | Default Path                       |
| --------------- | ---------------------------- | ---------------------------------- |
| History         | Browsing history & downloads | `Default/History`                  |
| Cookies         | Stored cookies               | `Default/Network/Cookies`          |
| Login Data      | Saved credentials            | `Default/Login Data`               |
| Web Data        | Autofill & form data         | `Default/Web Data`                 |
| Favicons        | Website icons                | `Default/Favicons`                 |
| Bookmarks       | Saved bookmarks              | `Default/Bookmarks`                |
| Preferences     | Browser settings             | `Default/Preferences`              |
| Extensions      | Installed extensions         | `Default/Extensions/`              |
| Top Sites       | Frequently visited sites     | `Default/Top Sites`                |
| Current Session | Active tabs & sessions       | `Default/Sessions/Current Session` |

---

## 01 - ``History`` Analysis

The ``History`` database helps investigators reconstruct user browsing activity, visited websites, searches, and downloaded files. It is useful for identifying phishing activity, malicious domains, attacker infrastructure, and user actions during an incident.

#### Important Tables

| Table                | Description           |
| -------------------- | --------------------- |
| urls                 | Visited URLs          |
| visits               | Visit timestamps      |
| downloads            | Downloaded files      |
| keyword_search_terms | Search engine queries |

#### Visited URLs Query

```sql
SELECT url, title, visit_count
FROM urls;
```

#### Search Query

```sql
SELECT term
FROM keyword_search_terms;
```

#### Download History Query

```sql
SELECT target_path, tab_url
FROM downloads;
```

---

## 02 - ``Cookies`` Analysis

The ``Cookies`` database stores authentication sessions, tracking data, and login states. It is useful for identifying active sessions, account compromise, session hijacking, and tracking user access to web services.

```sql
SELECT host_key, name, value, is_persistent, last_access_utc, expires_utc
FROM cookies;
```

---

## 03 - ``Login Data`` Analysis

The ``Login Data`` database stores saved usernames and encrypted passwords. It is useful for detecting credential theft, reused accounts, and compromised services.

```sql
SELECT origin_url, username_value, password_value
FROM logins;
```

---

## 04 - ``Web Data`` Analysis

The ``Web Data`` database stores autofill information such as emails, phone numbers, addresses, and payment details. It is useful for identifying exposed personal data and identity theft risks.

```sql
SELECT name, value
FROM autofill;
```

---

## 05 - ``Favicons`` Analysis

The ``Favicons`` database stores website icons associated with visited pages. It is useful for correlating browsing activity and recovering traces of visited websites.

```sql
SELECT icon_url
FROM favicons;
```

---

## 06 - ``Bookmarks`` Analysis

The ``Bookmarks`` file stores saved favorite websites and bookmarked pages. It is useful for identifying important services, frequently used platforms, and suspicious saved links.

```sql
SELECT url
FROM bookmarks;
```

---

## 07 - ``Preferences`` Analysis

