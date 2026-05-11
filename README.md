# Chromium Browser Forensics Playbook

Chromium-based browsers such as **Google Chrome**, **Microsoft Edge**, **Brave**, and **Opera** store valuable forensic artifacts including browsing history, downloads, cookies, credentials, sessions, extensions, and cache data.

This playbook provides a practical DFIR approach for collecting and analyzing Chromium browser artifacts during incident response, forensic investigations, CTFs, and security labs.

---

## 01 - Evidence Collection

### Windows

```bash
%LOCALAPPDATA%\Google\Chrome\User Data\
%LOCALAPPDATA%\Microsoft\Edge\User Data\
%LOCALAPPDATA%\BraveSoftware\Brave-Browser\User Data\
```

### Linux

```bash
~/.config/google-chrome/
~/.config/microsoft-edge/
~/.config/BraveSoftware/Brave-Browser/
```

### macOS

```bash
~/Library/Application Support/Google/Chrome/
~/Library/Application Support/Microsoft Edge/
~/Library/Application Support/BraveSoftware/Brave-Browser/
```

---

### 02 - Common Chromium Profile Files

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

### 03 - Browsing ``History`` Analysis

The ``History`` database helps investigators reconstruct user browsing activity, visited websites, searches, and downloaded files. It is useful for identifying phishing activity, malicious domains, attacker infrastructure, and user actions during an incident.

### Important Tables

| Table                | Description           |
| -------------------- | --------------------- |
| urls                 | Visited URLs          |
| visits               | Visit timestamps      |
| downloads            | Downloaded files      |
| keyword_search_terms | Search engine queries |

### Visited URLs Query

```bash
SELECT url, title, visit_count
FROM urls;
```

### Search Queries

```bash
SELECT term
FROM keyword_search_terms;
```

### Download History Query

```bash
SELECT target_path, tab_url
FROM downloads;
```

---

## 04 - Cookies & Sessions




