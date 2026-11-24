# Mimer RadioServer 14 - Security Advisories
 
Proofs of Concept (PoCs) for 3 vulnerabilities identified in **Mimer RadioServer (version 14)** during a penetration test.<br>

## Vulnerabilities
 
| CVE | Type | CWE | Affected component |
|-----|------|-----|--------------------|
| [CVE-2026-39137](#cve-2026-39137--reflected-cross-site-scripting-xss) | Reflected XSS | CWE-79 | `generate_radio_table.php` |
| [CVE-2026-39138](#cve-2026-39138--cross-site-request-forgery-csrf) | CSRF | CWE-352 | All functionalities |
| [CVE-2026-39139](#cve-2026-39139--php-code-injection--remote-code-execution) | PHP Code Injection | CWE-94 | `update_password.php`, `credentials.php` |
 
## Affected product
 
- **Product:** Mimer RadioServer - Admin Control Panel
- **Version:** 14
- **Vendor:** LS Elektronik AB (https://lse.se/radioserver)
---
 
## CVE-2026-39137 — Reflected Cross-Site Scripting (XSS)

A reflected cross-site scripting vulnerability exists in `generate_radio_table.php` in Mimer RadioServer Admin Control Panel 14. Values derived from the `fileString` parameter are inserted into HTML attribute context (`<input value="...">`) without output encoding, allowing an attacker to inject and execute arbitrary JavaScript in the context of a victim's session.
 
- Proof of concept: [`cve-2026-39137.html`](./cve-2026-39137.html)
---
 
## CVE-2026-39138 — Cross-Site Request Forgery (CSRF)

The Mimer RadioServer Admin Control Panel 14 does not implement anti-CSRF protections on its state-changing endpoints. A remote attacker can forge requests that an authenticated administrator's browser will submit automatically. Because this includes the password-update function, the CSRF can be chained with CVE-2026-39139 to reach remote code execution without the attacker possessing valid credentials.
 
- Proof of concept: [`cve-2026-39138.html`](./cve-2026-39138.html)
---
 
## CVE-2026-39139 — Authenticated Remote Code Execution
 
The password-update routine in Mimer RadioServer Admin Control Panel 14 writes the supplied password value into `credentials.php` without sanitization or encoding. The written value is subsequently interpreted as PHP source, so an attacker can inject arbitrary PHP code that executes on later requests. Because the injected code resides in a file included by `login.php` (which does not require authentication), the resulting code execution is reachable pre-authentication once injection has occurred.
 
- Proof of concept: [`cve-2026-39139.py`](./cve-2026-39139.py)
---

## Disclaimer 

These PoCs have been created only for research purposes. Using them against systems without explicit authorization is illegal.
