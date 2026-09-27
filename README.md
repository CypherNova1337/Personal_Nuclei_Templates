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

### CVEs
| Template | Name | Severity |
|---|---|---|
| `cves/CVE-2025-29927.yaml` | Next.js Middleware Bypass | critical |

### Exposures
| Template | Name | Severity |
|---|---|---|
| `exposures/env-file-exposure.yaml` | Environment (.env) File Exposure | high |
| `exposures/aws-keys-disclosure.yaml` | AWS Access/Secret Key Disclosure | high |
| `exposures/git-config-exposure.yaml` | Git Config / Repository Exposure | medium |
| `exposures/credentials-disclosure.yaml` | Credentials & Secrets Disclosure (Body Regex) | medium |
| `exposures/compressed-backup-files.yaml` | Compressed Backup File Detection | medium |
| `exposures/php-backup-files.yaml` | PHP Source Backup File Disclosure | medium |
| `exposures/wp-setup-config-exposed.yaml` | WordPress setup-config.php Exposed | critical |
| `exposures/phpinfo-exposure.yaml` | PHPInfo Page Exposure | low |
| `exposures/apache-server-status.yaml` | Apache mod_status Exposure | low |
| `exposures/ds-store-exposure.yaml` | Apple .DS_Store File Exposure | low |

### Misconfiguration
| Template | Name | Severity |
|---|---|---|
| `misconfiguration/crlf-injection.yaml` | CRLF Injection Detection | high |
| `misconfiguration/put-method-enabled.yaml` | PUT Method Enabled | high |
| `misconfiguration/cors-misconfiguration.yaml` | CORS Misconfiguration (Credentialed Arbitrary Origin) | medium |
| `misconfiguration/x-forwarded-host-injection.yaml` | X-Forwarded-Host Header Reflection | medium |
| `misconfiguration/spring-boot-actuator-exposure.yaml` | Spring Boot Actuator Exposure | medium |
| `misconfiguration/directory-listing.yaml` | Directory Listing Enabled | low |
| `misconfiguration/iis-shortname-enumeration.yaml` | IIS Short Name (8.3) Enumeration | low |
| `misconfiguration/cloudflare-rocketloader-htmli.yaml` | Cloudflare Rocket Loader HTML Injection | low |

### Vulnerabilities
| Template | Name | Severity |
|---|---|---|
| `vulnerabilities/error-based-sqli.yaml` | Error-Based SQL Injection Detection | high |
| `vulnerabilities/linux-lfi-comprehensive.yaml` | Comprehensive Linux LFI Scanner | high |
| `vulnerabilities/response-based-ssrf.yaml` | Full Response SSRF Detection | high |
| `vulnerabilities/nextjs-cache-poisoning.yaml` | Next.js Cache Poisoning | high |
| `vulnerabilities/open-redirect.yaml` | Open Redirect Detection | medium |
| `vulnerabilities/graphql-introspection-enabled.yaml` | GraphQL Introspection Enabled | info |

### Takeovers
| Template | Name | Severity |
|---|---|---|
| `takeovers/subdomain-takeover-detect.yaml` | Subdomain Takeover Detection | high |
| `takeovers/wordpress-takeover.yaml` | WordPress Takeover Detection | high |

### Technologies
| Template | Name | Severity |
|---|---|---|
| `technologies/s3-bucket-detect.yaml` | Amazon S3 Bucket Detection | info |
| `technologies/api-endpoints.yaml` | Common API Endpoints | info |
| `technologies/graphql-endpoints.yaml` | GraphQL Endpoint Discovery | info |
| `technologies/swagger-ui-detect.yaml` | Swagger UI Config URL Injection | low |

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
