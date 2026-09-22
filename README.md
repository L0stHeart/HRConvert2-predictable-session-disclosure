# HRConvert2: predictable session id leaks other users' files

https://github.com/zelon88/HRConvert2/security/advisories/GHSA-qj74-5h4j-f368

HRConvert2 is a self-hosted PHP file conversion and sharing server (zelon88/HRConvert2). I reviewed v3.5, commit `f27083b`. No authentication anywhere in the app. I reported this on 2026-07-31. The vendor published the advisory on 2026-08-03.

There is no fixed version on the advisory, and no CVE.

Each visitor's uploads and conversions live under a per-session directory. With the salts left at the values shipped in the default config, that session value is a function of the date and those salts. Every visitor on a given day shares it. The log filename is derived from the same inputs, and the log is served out of the web root. The log lines name other sessions' files and the paths those files were written to. Those paths are web-accessible too. No login is required.

I only verified the default-salt case. The config file ships working defaults, and nothing forces an administrator to change them, but a deployment that already replaced the salts is outside what I demonstrated. Replacing the salts stops the guess. The logs and the data directory should not be served directly either way. Both have to be true before this is gone. Changing the salts alone leaves the files on a URL if the directory name leaks some other way.

| | |
|---|---|
| Affected | 3.5 and earlier, default salts |
| Fixed | Not stated on the advisory |
| Severity | High, 7.5 |
| Vector | CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N |
| CWE | CWE-330, CWE-200, CWE-548 |
| Privileges | None |
| CVE | Not assigned |

I read another user's file end to end in a local lab: one session uploaded and downloaded it, a second session fetched it with no credentials. The vendor published that report as the advisory.

v3.6.6's note about hardening the core does not mention session identifiers or the log directory. I have not retested any release after 3.5 for this bug, so there is no fix version I will stand behind.

I asked for a CVE on 2026-08-07, in the same note as the other HRConvert2 advisories from this review. The maintainer closed it the same day. As of 2026-09-22 this advisory has no CVE id.

Local instance of v3.5 only. No third-party deployment.

L0stHeart
https://github.com/L0stHeart
