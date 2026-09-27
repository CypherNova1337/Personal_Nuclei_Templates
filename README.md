# Personal Nuclei Templates

A curated, hand-tuned collection of [Nuclei](https://github.com/projectdiscovery/nuclei)
templates I use for authorized security testing and bug-bounty reconnaissance.

Every template here is written and maintained by **CypherNova1337**. The focus is
**signal over noise**: each template is built to fire only when there is strong,
corroborated evidence of the issue, so results are worth triaging. Where a template
is based on public research or a CVE, the original research is credited in the
template's `reference:` list.

> ⚠️ **Authorized use only.** These templates are for testing systems you own or
> are explicitly permitted to test. You are responsible for how you use them.

---

## Layout

```
cves/              # Templates for specific CVEs
exposures/         # Sensitive files, secrets and information disclosure
misconfiguration/  # Insecure server / application configuration
vulnerabilities/   # Active vulnerability checks (SQLi, LFI, SSRF, redirects, ...)
takeovers/         # Subdomain / service takeover detection
technologies/      # Technology & endpoint discovery
fuzzing/           # DAST / fuzzing templates
```

## Usage

Install Nuclei (see the [official docs](https://github.com/projectdiscovery/nuclei#install-nuclei)), then:

```bash
# Run the whole collection against a single target
nuclei -u https://target.example.com -t /path/to/Personal_Nuclei_Templates/

# Run one category
nuclei -u https://target.example.com -t /path/to/Personal_Nuclei_Templates/exposures/

# Run a single template
nuclei -u https://target.example.com -t /path/to/Personal_Nuclei_Templates/exposures/git-config-exposure.yaml

# Scan a list of hosts
nuclei -l hosts.txt -t /path/to/Personal_Nuclei_Templates/

# DAST / fuzzing templates (SQLi, SSRF, LFI) require fuzzing mode + real URLs
nuclei -l urls.txt -t /path/to/Personal_Nuclei_Templates/vulnerabilities/ -dast
```

Some templates depend on Nuclei features:

- **Interactsh** (out-of-band) is used by `response-based-ssrf.yaml`. It is enabled
  by default; use `-no-interactsh` to disable.
- **Fuzzing / DAST** (`-dast`) is required for the query-parameter fuzzing templates
  (`error-based-sqli`, `linux-lfi-comprehensive`, `response-based-ssrf`).

Validate everything before use:

```bash
nuclei -validate -t /path/to/Personal_Nuclei_Templates/
```

---

## Template index

_56 templates across 7 categories._

### CVEs
| Template | Name | Severity |
|---|---|---|
| `cves/CVE-2025-29927.yaml` | Next.js Middleware Bypass | critical |

### Exposures
| Template | Name | Severity |
|---|---|---|
| `exposures/wp-setup-config-exposed.yaml` | WordPress setup-config.php Exposed (Installation Available) | critical |
| `exposures/aws-credentials-file-exposure.yaml` | AWS Credentials File Exposure | high |
| `exposures/aws-keys-disclosure.yaml` | AWS Access/Secret Key Disclosure | high |
| `exposures/env-file-exposure.yaml` | Environment (.env) File Exposure | high |
| `exposures/kubeconfig-exposure.yaml` | Kubernetes kubeconfig Exposure | high |
| `exposures/npmrc-authtoken-exposure.yaml` | NPM .npmrc Auth Token Exposure | high |
| `exposures/sql-dump-exposure.yaml` | SQL Database Dump Exposure | high |
| `exposures/ssh-private-key-exposure.yaml` | SSH Private Key Exposure | high |
| `exposures/terraform-state-exposure.yaml` | Terraform State File Exposure | high |
| `exposures/compressed-backup-files.yaml` | Compressed Backup File - Detect | medium |
| `exposures/credentials-disclosure.yaml` | Credentials & Secrets Disclosure (Body Regex) | medium |
| `exposures/docker-compose-exposure.yaml` | Docker Compose File Exposure | medium |
| `exposures/git-config-exposure.yaml` | Git Config / Repository Exposure | medium |
| `exposures/git-head-exposure.yaml` | Git HEAD Exposure | medium |
| `exposures/laravel-log-exposure.yaml` | Laravel Log File Exposure | medium |
| `exposures/php-backup-files.yaml` | PHP Source - Backup File Information Disclosure | medium |
| `exposures/subversion-wcdb-exposure.yaml` | Subversion (.svn) Working Copy Exposure | medium |
| `exposures/wordpress-debug-log-exposure.yaml` | WordPress debug.log Exposure | medium |
| `exposures/apache-server-status.yaml` | Apache mod_status - server-status Exposure | low |
| `exposures/dockerfile-exposure.yaml` | Dockerfile Exposure | low |
| `exposures/ds-store-exposure.yaml` | Apple .DS_Store File Exposure | low |
| `exposures/phpinfo-exposure.yaml` | PHPInfo Page Exposure | low |
| `exposures/source-map-exposure.yaml` | JavaScript Source Map Exposure | low |

### Misconfiguration
| Template | Name | Severity |
|---|---|---|
| `misconfiguration/crlf-injection.yaml` | CRLF Injection Detection | high |
| `misconfiguration/put-method-enabled.yaml` | PUT Method Enabled | high |
| `misconfiguration/werkzeug-debugger-exposure.yaml` | Werkzeug / Flask Interactive Debugger Exposure | high |
| `misconfiguration/cors-misconfiguration.yaml` | CORS Misconfiguration - Credentialed Arbitrary Origin Reflection | medium |
| `misconfiguration/cors-null-origin.yaml` | CORS Misconfiguration - Null Origin Trusted | medium |
| `misconfiguration/django-debug-mode.yaml` | Django DEBUG Mode Enabled | medium |
| `misconfiguration/laravel-debug-mode.yaml` | Laravel Debug Mode Enabled (Whoops / Ignition) | medium |
| `misconfiguration/spring-boot-actuator-exposure.yaml` | Spring Boot Actuator - Sensitive Endpoint Exposure | medium |
| `misconfiguration/symfony-profiler-exposure.yaml` | Symfony Web Profiler Exposure | medium |
| `misconfiguration/x-forwarded-host-injection.yaml` | X-Forwarded-Host Header Reflection | medium |
| `misconfiguration/cloudflare-rocketloader-htmli.yaml` | Cloudflare Rocket Loader - HTML Injection | low |
| `misconfiguration/directory-listing.yaml` | Directory Listing Enabled | low |
| `misconfiguration/http-trace-method-enabled.yaml` | HTTP TRACE Method Enabled (Cross-Site Tracing) | low |
| `misconfiguration/iis-shortname-enumeration.yaml` | IIS Short Name (8.3) Enumeration | low |
| `misconfiguration/insecure-cookie-flags.yaml` | Session Cookie Missing HttpOnly / Secure Flags | info |
| `misconfiguration/missing-security-headers.yaml` | Missing HTTP Security Headers (No Hardening) | info |

### Vulnerabilities
| Template | Name | Severity |
|---|---|---|
| `vulnerabilities/error-based-sqli.yaml` | Error-Based SQL Injection Detection | high |
| `vulnerabilities/linux-lfi-comprehensive.yaml` | Comprehensive Linux Local File Inclusion (LFI) Scanner - v2 | high |
| `vulnerabilities/nextjs-cache-poisoning.yaml` | Next.js - Cache Poisoning | high |
| `vulnerabilities/response-based-ssrf.yaml` | Full Response SSRF Detection | high |
| `vulnerabilities/open-redirect.yaml` | Open Redirect Detection | medium |
| `vulnerabilities/graphql-field-suggestion.yaml` | GraphQL Field Suggestion Enabled | info |
| `vulnerabilities/graphql-introspection-enabled.yaml` | GraphQL Introspection Enabled | info |

### Fuzzing
| Template | Name | Severity |
|---|---|---|
| `fuzzing/ssti-reflected.yaml` | Server-Side Template Injection (Reflected, Arithmetic Probe) | high |

### Takeovers
| Template | Name | Severity |
|---|---|---|
| `takeovers/subdomain-takeover-detect.yaml` | Subdomain Takeover Detection | high |
| `takeovers/wordpress-takeover.yaml` | WordPress takeover detection | high |

### Technologies
| Template | Name | Severity |
|---|---|---|
| `technologies/swagger-ui-detect.yaml` | Swagger UI Config URL Injection | low |
| `technologies/wordpress-user-enumeration.yaml` | WordPress User Enumeration via REST API | low |
| `technologies/wordpress-xmlrpc-enabled.yaml` | WordPress XML-RPC Interface Enabled | low |
| `technologies/api-endpoints.yaml` | Common API Endpoints | info |
| `technologies/graphql-endpoints.yaml` | GraphQL Endpoint Discovery | info |
| `technologies/s3-bucket-detect.yaml` | Amazon S3 Bucket Detection | info |

---

## Design principles

- **Low false positives.** Findings require multiple corroborating signals
  (e.g. content structure + status + content-type), unique random markers instead
  of literal `evil.com`-style strings, and negative matchers to rule out HTML
  fallback pages. CORS, for example, only fires on an arbitrary *external* origin
  reflected together with `Access-Control-Allow-Credentials: true` and a baseline
  differential — not on plain origin reflection.
- **Modern Nuclei v3 syntax.** All templates use the `http:` protocol block and
  pass `nuclei -validate`.
- **Actionable metadata.** Each template carries a clear description, severity,
  CWE/CVSS classification where relevant, references and remediation guidance.

## Contributing / contact

This is a personal collection. Suggestions and bug reports are welcome via issues.

**Author:** CypherNova1337 · cyphernova7331@proton.me

## License

Provided as-is for lawful, authorized security testing. No warranty of any kind.
