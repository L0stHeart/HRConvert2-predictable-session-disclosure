# HRConvert2: predictable session id leaks other users' files

https://github.com/zelon88/HRConvert2/security/advisories/GHSA-qj74-5h4j-f368

Affects HRConvert2 3.5 and earlier, on installs that still use the default salts shipped in the config. The advisory names no fixed release. No CVE assigned.

High. CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N (7.5).
CWE-330, CWE-200, CWE-548.

The session value is a function of the date and those salts, so every visitor on a given day shares it. A log written from that value is served from the web root, and the log names other sessions' files. Those files are under the web root as well. No login is required. I only verified this with the default salts left as shipped. Replacing the salts stops the guess. The logs should not be web-accessible either way.

Reported 2026-07-31 through GitHub private vulnerability reporting. The vendor published the advisory on 2026-08-03.

L0stHeart
