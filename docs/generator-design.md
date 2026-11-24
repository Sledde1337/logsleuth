# Generator design

## Injected attacks

- SSH brute force: IP 203.0.113.50 tries the user `root` with 60 failed
  logins between 02:10 and 02:15 on one night. No successful login.
  Expected detection: 1 finding, 60 failures.
- Password spraying: IP 198.51.100.23 tries 20 different usernames
  (admin, test, guest, ubuntu, ...) with 2 failed logins each between
  03:00 and 03:30. Expected detection: 1 finding, 20 users, 40 failures.
- Web scanning: IP 192.0.2.77 requests 80 paths that don't exist
  (/admin, /.env, /wp-login.php, ...) between 04:00 and 04:05 and gets
  a 404 each time. Expected detection: 1 finding, 80 requests, 80 404s.
- Unusual user agent: IP 198.51.100.99 makes 15 requests with the user
  agent `sqlmap/1.7` between 05:00 and 05:10, while all normal traffic
  uses browser user agents. Expected detection: 1 finding, 15 requests.

## Log line formats

auth.log example (ISO 8601 timestamps, as written by rsyslog on newer
Debian/Ubuntu; includes year and timezone):

    2026-11-08T03:14:07.512340+00:00 web-1 sshd[21873]: Failed password for root from 203.0.113.50 port 51822 ssh2

nginx access.log example (combined format):

    192.0.2.10 - - [08/Nov/2026:03:14:07 +0000] "GET /index.html HTTP/1.1" 200 1024 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0 Safari/537.36"

## Decisions

All attacks happen on 2026-11-08 (UTC). Times in the attack list above are UTC.