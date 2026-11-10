# Notes

## 2026-11-09 — PHP2Shell interim mitigation (CVE-2026-109618 / CVE-2026-109619)

Unauthenticated RCE in PHP; Apache mod_php (used here) is highest risk.
Advisory: https://www.netspi.com/blog/executive-blog/critical-vulnerability/php2shell-unauthenticated-rce-on-php-cve-2026-109618-cve-2026-109619/
Fixed in PHP 8.5.12, 8.4.27, 8.3.36, 8.2.35. No official fix for 8.1 and older.

- Running: `jkozik/nwrtlsdr.com:v1`, PHP 8.2.34, vulnerable.
- Interim fix (covers CVE-2026-109619 only): ConfigMap `php-cve-2026-109619`
  (`php-cve-2026-109619-cm.yml`) sets `session.upload_progress.enabled = 0`,
  mounted into `nwrtlsdr` at `/usr/local/etc/php/conf.d/zz-cve-2026-109619.ini`.
  Verified Off in all pods.
- Real fix: rebuild the image on `php:8.4-apache` (= 8.4.27 as of 2026-11-06) with
  `docker build --pull`, push a new tag, update the image in `nwrtlsdr-deploy.yml`,
  and confirm with `kubectl exec deploy/nwrtlsdr -- php -v`.
- Also on rebuild: cap Apache MaxRequestWorkers at about 10-15 (memory limit is 384Mi).
