# Google Dork Cheat Sheet

A quick-reference collection of the most useful Google dork patterns for authorized penetration testing and bug bounty reconnaissance. Replace `target.com` with your authorized target scope.

> ⚠️ **Use only on systems you own or are authorized to test.** Unauthorized scanning is a crime in most jurisdictions.

## Search Operators Refresher

| Operator | What it does |
| -------- | ------------ |
| `site:` | Restrict results to a domain |
| `intitle:` | Search page title |
| `inurl:` | Search the URL |
| `intext:` | Search page body text |
| `filetype:` | Match file extension |
| `ext:` | Same as filetype |
| `cache:` | Show Google's cached version |
| `link:` | Pages linking to a URL |
| `-` | Exclude a term |
| `OR` / `|` | Either term |
| `*` | Wildcard |
| `"..."` | Exact phrase |
| `before:` / `after:` | Date range filter |

## Directory Listing

```
site:target.com intitle:"index of"
site:target.com intitle:"index of" "parent directory"
site:target.com intitle:"index of" (wp-content OR uploads OR backup)
```

## Exposed Database Files

```
site:target.com filetype:sql
site:target.com ext:sql "INSERT INTO"
site:target.com ext:db "SQLite format"
site:target.com ext:dump (password OR passwd)
```

## Configuration Files

```
site:target.com ext:env
site:target.com (ext:yml OR ext:yaml) (password OR secret OR key)
site:target.com filetype:config (password OR passwd)
site:target.com intitle:"index of" .git
site:target.com filetype:xml "connectionStrings"
```

## Credentials & Secrets

```
site:target.com intext:"api_key" OR intext:"apikey"
site:target.com inurl:api (intext:sk_live OR intext:api_key)
site:target.com filetype:log (password OR passwd OR pwd)
site:target.com intext:"BEGIN RSA PRIVATE KEY"
site:target.com filetype:pem
site:target.com intext:"aws_access_key_id"
```

## Login & Admin Panels

```
site:target.com inurl:admin
site:target.com inurl:login
site:target.com inurl:wp-login
site:target.com inurl:phpmyadmin
site:target.com intitle:"dashboard" intext:"login"
```

## Error Messages (Info Disclosure)

```
site:target.com "SQL syntax" OR "mysql_fetch_array"
site:target.com "ORA-01756"
site:target.com "Warning: mysql_connect()"
site:target.com intext:"stack trace" (exception OR error)
site:target.com filetype:php intext:"Undefined index"
```

## Backup & Old Files

```
site:target.com (ext:bak OR ext:backup OR ext:old OR ext:swp)
site:target.com ext:zip (backup OR db OR site)
site:target.com filetype:tar "backup"
site:target.com intitle:"index of" backup
```

## Interesting Server Info

```
site:target.com intitle:"phpinfo()" "PHP Version"
site:target.com inurl:server-status "Apache Status"
site:target.com inurl:server-info "Apache Server Information"
site:target.com filetype:txt "Disallow:" inurl:robots
```

## Juicy Documents

```
site:target.com (filetype:pdf OR filetype:doc OR filetype:xls) (confidential OR internal)
site:target.com filetype:xlsx (password OR credentials)
site:target.com filetype:ppt (roadmap OR strategy)
```

## Combining Dorks

Combine operators to narrow results and cut noise:

```
site:target.com intitle:"index of" -inurl:https
site:target.com (ext:log OR ext:txt) intext:password after:2024-01-01
site:sub.target.com inurl:api intext:"token"
```

## Defensive Self-Check

Run these against your **own** domain monthly:

```
site:yourdomain.com ext:sql
site:yourdomain.com ext:env
site:yourdomain.com intitle:"index of"
site:yourdomain.com intext:"BEGIN RSA PRIVATE KEY"
```

Found something indexed that shouldn't be? See [Writeup 8](8.Defensive%20Security%20How%20to%20Prevent%20Google%20Indexing%20and%20Protect%20Against%20Google%20Dorking.md) for removal and prevention steps.

## Further Reading

- [Google Hacking Database](https://www.exploit-db.com/google-hacking-database) — thousands of categorized, real-world dorks
- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) — the methodology behind the recon
