# HRConvert2 session ids are shared for a whole day

https://github.com/zelon88/HRConvert2/security/advisories/GHSA-qj74-5h4j-f368

Severity: high (CVSS 7.5, `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`). CWE-330, CWE-200, CWE-548. No CVE. The advisory names no fixed release.

HRConvert2 3.5 and earlier is affected when the salts are still the defaults shipped in the config. I reviewed 3.5, commit f27083b. There is no authentication.

Each visitor's uploads live under a per-session directory. With those default salts, the session value is a function of the date and the salts, so every visitor on a given day shares it. The log name comes from the same inputs, and the log is served from the web root. The log names other sessions' files and the paths they were written to. Those paths are web-accessible too. No login is required.

I only demonstrated the default-salt case. The config ships working defaults and nothing forces a change, but a deployment that already replaced the salts is outside what I showed. Replacing the salts stops the guess. The logs and the data directory should not be served directly either way. Changing the salts alone leaves the files on a URL if the directory name leaks some other way.

In the local lab one session uploaded a file and downloaded it, and a second session with no credentials fetched that file.

The 3.6.6 note about hardening the core does not mention session identifiers or the log directory. I have not retested any release after 3.5, so there is no fix version I will stand behind.

I asked for a CVE on 7 August 2026, in the same note as the other HRConvert2 advisories from this review. The maintainer closed it the same day. As of 22 September 2026 this advisory has no CVE id.

Local instance of 3.5 only.

Reported privately on 31 July 2026. The vendor published the advisory on 3 August 2026.

L0stHeart
