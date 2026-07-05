# Tinder Bug Bounty Recon - 2026-07-05

## Summary
- **Target**: tinder.com
- **Subdomains Enumerated**: 79 (via subfinder)
- **Date**: 2026-07-05
- **Primary Domain**: tinder.com

## Live Hosts
| Host | Status | Notes |
|------|--------|-------|
| www.tinder.com | 301 -> tinder.com | Redirects to root |

All other subdomains (api, app, help, support, blog, etc.) return connection failures.

## CORS Testing
- www.tinder.com redirects to tinder.com
- No CORS headers visible in redirect response

## Exposed Files
| Path | Status | Size | Notes |
|------|--------|------|-------|
| /robots.txt | 200 | 111 bytes | Small, standard |
| /.env | 200 | 560 KB | SPA catch-all (same page) |
| /.git/config | 200 | 560 KB | SPA catch-all |

Note: All paths return HTTP 200 with identical page content (React SPA catch-all behavior).

## Directory Fuzzing (ffuf)
- `/sitemap.xml` returns HTTP 200 (42 KB)
- `/robots.txt` returns HTTP 200 (111 bytes)
- All other paths return HTTP 301 redirects

## Security Findings
- **Severity**: Low
- Minimal attack surface - only the root domain is publicly accessible
- SPA catch-all routes make all paths appear "found" (200 OK)
- No CORS issues detected
- Subdomain enumeration found 79 subdomains (invite.tinder.com, etc.)
