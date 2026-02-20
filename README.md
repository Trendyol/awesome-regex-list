[![Patterns](https://img.shields.io/badge/patterns-37+-blue)](https://github.com/trendyol/awesome-regex-list)
[![Categories](https://img.shields.io/badge/categories-15-green)](https://github.com/trendyol/awesome-regex-list)
[![License](https://img.shields.io/badge/license-Apache%202.0-orange)](LICENSE)
[![Format](https://img.shields.io/badge/format-YAML-yellow)](regexes.yml)

## TL;DR

A curated collection of **37+ regex patterns** for detecting **credentials**, **PII**, and **sensitive data** (optimized for Turkish data). Features **context-aware matching** to reduce false positives. Ready to use in security scanning, data validation, and compliance tools.

```yaml
# Example usage
- name: "Credit Card Information"
  regexes:
    - "(?:credit|card|kredi|kart)...(4\\d{3}[\\s-]?\\d{4}[\\s-]?\\d{4}[\\s-]?\\d{4})"
# card_id: 5555-5555-5555-4444  ✅ Detected
# user_id: 5555-5555-5555-4444  ❌ Not detected (no false positive)
```

---

# Awesome Regex List

This repository provides ready-to-use regex patterns for security scanning, data validation, and compliance tools. Many patterns in this collection go beyond simple format matching by using a technique we call **context-aware matching**.

### What is Context-Aware Matching?

![Context-Aware Regex](images/context_aware_regex.png)
Unlike traditional regex patterns that only match data formats, context-aware patterns use **keyword pre-matching** to reduce false positives. This approach first identifies context keywords, then captures the actual sensitive value.

**Example text:**

```
card_id: 5555-5555-5555-4444
user_id: 5555-5555-5555-4444
```

**Traditional approach** (format-only):

```regex
5\d{3}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}
```

❌ Matches both lines — `user_id` is a false positive!

**Our approach** (context-aware):

![Context-Aware Format](images/context_aware_format.png)

✅ Only matches the first line — keyword `card` required before the number.

This significantly reduces false positives while maintaining high recall for actual sensitive data.

---

## Quick Reference

| Category                                                     | Patterns | Use Case                                 |
| ------------------------------------------------------------ | -------- | ---------------------------------------- |
| [Access Credentials](#access-credentials)                       | 10       | API keys, tokens, authentication         |
| [Amazon Web Services](#amazon-web-services)                     | 5        | AWS keys, secrets, MWS tokens            |
| [Cloudflare](#cloudflare)                                       | 4        | Cloudflare API tokens                    |
| [Google Credentials](#google-credentials)                       | 6        | Google API keys, OAuth, service accounts |
| [Address Information](#address-information)                     | 1        | Physical addresses, location data        |
| [Credit Card Information](#credit-card-information)             | 2        | Card numbers, CVV codes                  |
| [Birthday Information](#birthday-information)                   | 1        | Date of birth, PII detection             |
| [Database Connection Strings](#database-connection-strings)     | 1        | DB URLs, connection strings              |
| [Email Addresses](#email-addresses)                             | 1        | Email detection                          |
| [Fax Numbers](#fax-numbers)                                     | 1        | Fax number detection                     |
| [Github Credentials](#github-credentials)                       | 1        | GitHub tokens                            |
| [IBAN Information](#iban-information)                           | 1        | Bank account numbers                     |
| [IP Addresses](#ip-addresses)                                   | 1        | IPv4 address detection                   |
| [Mac Addresses](#mac-addresses)                                 | 1        | Mac Address Detection                    |
| [OpenAI Credentials](#openai-credentials)                       | 1        | OpenAI API keys and tokens               |
| [Password](#password)                                           | 1        | Password detection in code/config        |
| [Phone Numbers](#phone-numbers)                                 | 7        | Turkish phone number formats             |
| [Private Keys and Certificates](#private-keys-and-certificates) | 2        | Private keys, SSL/TLS certificates       |
| [Slack Credentials](#slack-credentials)                         | 1        | Slack API tokens                         |
| [Slack Webhook](#slack-webhook)                                 | 1        | Slack webhook URLs                       |
| [Passport Number](#passport-number)                              | 1        | Passport numbers (e.g. Turkish U + 8 digits) |
| [Turkish Identity Numbers](#turkish-identity-numbers)           | 1        | Turkish national ID numbers (TCKN)       |
| [Turkish ID Serial Number](#turkish-id-serial-number)          | 1        | Turkish ID card serial (e.g. A12Z34567)  |
| [Turkish Tax Numbers](#turkish-tax-numbers)                     | 1        | Turkish tax identification numbers       |

## Repository Structure

The repository maintains regex patterns in YAML format, organized by categories:

- **`regexes.yml`**: YAML-encoded patterns organized by categories

Each category entry is structured with the following fields:

- `name`: Category name (e.g., "Access Credentials")
- `description`: Semantic description of the category's purpose
- `regexes`: Array of regular expression patterns in Python-compatible syntax

This category-based structure allows for better organization as the collection grows, making it easier to find patterns relevant to specific use cases.

---

## Pattern Specifications

### Access Credentials

**Category Description:** Patterns for detecting and validating various types of access credentials, API keys, authentication tokens, and secret keys commonly used in software development and cloud services.

**Patterns Included:**

1. **Basic Authentication Tokens**

   - `\bbasic\s+([a-zA-Z0-9_\-\.=]{20,})\b` - HTTP Basic Auth tokens
   - `(?<=:\/\/)[a-zA-Z0-9]+:[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z]+` - Credentials in URLs
2. **Bearer Tokens**

   - `\bbearer\s+([a-zA-Z0-9_\-\.=]{20,})\b` - OAuth Bearer tokens
   - `\beyJ[a-zA-Z0-9\-_]+\.[a-zA-Z0-9\-_]+\.[a-zA-Z0-9\-_]{43}\b` - JWT tokens
3. **Python Package Index**

   - `pypi-[a-zA-Z0-9_-]{32,128}` - PyPI API tokens
4. **Live/Test API Keys**

   - `sk_live_[0-9a-zA-Z]{24,99}` - Live API keys (commonly used pattern for production API keys)
   - `sk_test_[0-9a-zA-Z]{24,99}` - Test API keys (commonly used pattern for test/staging API keys)
5. **Generic API Keys**

   - `(?i)\b(?:api[\s_-]?key|x[\s_-]?api[\s_-]?key|access[\s_-]?token|auth[\s_-]?token|authorization|secret[\s_-]?key|client[\s_-]?secret)\b\s*(?:(?:[:=]\s*([A-Za-z0-9]{12,}))|(?:"([A-Za-z0-9]{12,})"))` - Generic API key patterns
6. **Generic Secret Keys**

   - `sk-[0-9a-zA-Z]{40,60}` - Generic secret keys

### Amazon Web Services

**Category Description:** Patterns for detecting Amazon Web Services credentials and access keys.

**Patterns Included:**

1. **AWS Access Key IDs**

   - `AKIA[0-9A-Z]{16}` - AWS Access Key IDs (standard format)
   - `ASIA[0-9A-Z]{16}` - AWS Temporary Access Key IDs (for temporary credentials)
   - `(?<![A-Za-z0-9_])(?-i:(?:A3T[A-Z0-9]|AKIA|AGPA|AIDA|AROA|AIPA|ANPA|ANVA|ASIA)[A-Z0-9]{16})(?![A-Za-z0-9_])` - Comprehensive AWS Access Key ID pattern covering all AWS credential prefixes (A3T, AKIA, AGPA, AIDA, AROA, AIPA, ANPA, ANVA, ASIA) with word boundary checks
2. **AWS Secret Access Keys**

   - `aws_secret_access_key\s*=\s*["'']?[0-9a-zA-Z/+]{40}["'']?` - AWS Secret Access Keys in configuration format
3. **Amazon Marketplace Web Service (MWS)**

   - `amzn\.mws\.[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}` - Amazon MWS access tokens in UUID format

### Cloudflare

**Category Description:** Patterns for detecting Cloudflare API tokens and credentials.

**Patterns Included:**

1. **Cloudflare API Tokens**
   - `dev_[a-zA-Z0-9]{40,60}` - Cloudflare development tokens
   - `exp_[a-zA-Z0-9]{40,60}` - Cloudflare expiration tokens
   - `cro_[a-zA-Z0-9]{40,60}` - Cloudflare read-only tokens
   - `crw_[a-zA-Z0-9]{40,60}` - Cloudflare read-write tokens

### Google Credentials

**Category Description:** Patterns for detecting Google API keys, OAuth credentials, and service account information.

**Patterns Included:**

1. **Google API Keys**

   - `AIza[0-9A-Za-z-_]{35}` - Google API keys (general format, used across multiple Google services including GCP, Drive, Gmail, YouTube)
   - `AIzaSy[a-zA-Z0-9-_]{33}` - Google Maps API keys
2. **Google OAuth Credentials**

   - `GOCSPX-[a-zA-Z0-9]{20,35}` - Google OAuth client secrets
   - `[0-9]+-[0-9A-Za-z_]{32}\.apps\.googleusercontent\.com` - Google OAuth client IDs (used for GCP, Drive, Gmail, YouTube OAuth applications)
   - `ya29\.[0-9A-Za-z\-_]+` - Google OAuth access tokens
3. **Google Service Account**

   - `"type":\s*"service_account"` - Google Cloud Platform service account JSON files

---

### Address Information

**Category Description:** Patterns for detecting physical addresses and location information including street addresses, postal codes, and geographic locations.

**Patterns Included:**

1. **Address Components Detection**
   - `(?:\b(?:no|kat|daire|apt|d|k)\b|(?:\d{1,4}(?:/\d{1,3})?)|(?:[A-ZÇĞİÖŞÜ]\s?){1,3})[\s,-]*\b\d{1,4}\b(?:\s\d{1,4})?|(?:\b(?:no)\s?\d{1,3})|(?:\b(?:apt|daire)\s?\d{1,3}(?:[a-z])?)` - Detects address components including building numbers, floor numbers (kat), apartment numbers (daire/apt), and street numbers with support for Turkish address formats

### Credit Card Information

**Category Description:** Patterns for detecting credit card information including card numbers, expiration dates, and CVV codes.

**Patterns Included:**

1. **Credit Card Numbers**

   - `(?:credit|card|kredi|kart|amex|visa|master|discover|american express|troy|kk|cc|cc_no|kartno|banka)[^\d]{0,20}?\b(4\d{3}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{1,4}|(?:5[1-5]\d{2}|2(?:2[2-9]\d|[3-6]\d{2}|7[01]\d|720))[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}|3[47]\d{2}[\s-]?\d{6}[\s-]?\d{5}|6(?:011|5\d{2})[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}|(?:9792|65\d{2}|2205)[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4}|36[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{2})\b` - Detects credit card numbers from major payment networks:
     - **Visa**: Cards starting with 4 (13-16 digits)
     - **Mastercard**: Cards starting with 5[1-5] or 2[2-9] (16 digits)
     - **American Express**: Cards starting with 3[47] (15 digits)
     - **Discover**: Cards starting with 6(011|5\d{2}) (16 digits)
     - **TROY**: Turkish domestic scheme — starts with 9792, 65XX, 36, or 2205 (14 or 16 digits)
     - Supports Turkish and English keywords (credit, card, kredi, kart, troy, etc.)
     - Handles various formatting styles (spaces, hyphens, or no separators)
2. **CVV/CVC Security Codes**

   - `(cvc|cvv|security[\s_-]?code)[\s:=\-]{0,3}([0-9]{3})(?![0-9])` - Detects CVV (Card Verification Value) or CVC (Card Verification Code) security codes:
     - Matches 3-digit security codes
     - Supports keywords: cvc, cvv, security code
     - Handles various separators and formatting styles
     - Uses negative lookahead to prevent matching longer sequences

### Birthday Information

**Category Description:** Patterns for detecting birthday information including dates of birth, age, and date of birth.

**Patterns Included:**

1. **Date of Birth Detection**
   - `(?i)\b(doğum[\s_]?tarihi|dogum[\s_]?tarihi|birthday|birth[\s_]?date|date[\s_]?of[\s_]?birth|dob)\b[\s:]*(0[1-9]|[12][0-9]|3[01])[\/\.](0[1-9]|1[0-2])[\/\.]([0-9]{4})\b` - Detects date of birth information:
     - **Keywords**: Supports Turkish (doğum tarihi, dogum tarihi) and English (birthday, birth date, date of birth, dob) keywords
     - **Date Format**: DD/MM/YYYY or DD.MM.YYYY format
     - **Day**: 01-31 (validates day range)
     - **Month**: 01-12 (validates month range)
     - **Year**: 4-digit year format
     - **Case Insensitive**: Pattern uses case-insensitive matching
     - **Separators**: Supports both forward slash (/) and dot (.) as date separators
     - **Context Matching**: Requires keyword context before the date to reduce false positives

### Database Connection Strings

**Category Description:** Patterns for detecting database connection strings including database names, server addresses, and credentials.

**Patterns Included:**

1. **Database Protocol URLs**
   - `(?:postgresql://|postgres://|mysql://|mssql://|oracle://|jdbc:|sqlite:///|db2://|firebird://|redis://|mongodb://|cassandra://|couchdb://|neo4j://|influxdb://|elasticsearch://)[^\s<]+` - Detects direct database connection URLs: - **Relational Databases**: PostgreSQL, MySQL, MSSQL, Oracle, DB2, Firebird, SQLite - **NoSQL Databases**: MongoDB, Cassandra, CouchDB, Redis - **Graph Databases**: Neo4j - **Time-Series Databases**: InfluxDB - **Search Engines**: Elasticsearch - **JDBC Protocol**: Java Database Connectivity URLs

### Email Addresses

**Category Description:** Patterns for detecting email addresses.

**Patterns Included:**

1. **Email Address Detection**
   - `[A-Za-z0-9._+-]+@[A-Za-z0-9.-]+\.(?:com|org|net|edu|gov|mil|int|co|io|co\.uk|us|ca|de|au|fr|jp|com\.tr|cn|in|br|ru|it|es|pl|mx|za|nl|se|ch|be|no|fi|dk|kr|gr|pt|hu|at|cz|sk|lt|lv|bg|ro|az|hk|tw|ae|sa|il|tr)` - Detects email addresses with common top-level domains (TLDs):
     - **Local Part**: Alphanumeric characters, dots, underscores, plus signs, and hyphens (`[A-Za-z0-9._+-]+`)
     - **Domain Part**: Alphanumeric characters, dots, and hyphens (`[A-Za-z0-9.-]+`)
     - **TLD Support**: Covers major generic TLDs (com, org, net, edu, gov, mil, int) and country-code TLDs from 50+ countries including:
       - **North America**: us, ca, mx
       - **Europe**: uk (co.uk), de, fr, it, es, pl, nl, se, ch, be, no, fi, dk, gr, pt, hu, at, cz, sk, lt, lv, bg, ro
       - **Asia-Pacific**: jp, cn, in, au, kr, hk, tw, az, il, tr (com.tr)
       - **Middle East**: ae, sa
       - **Africa**: za
       - **Other**: co, io, br, ru


### Fax Numbers

**Category Description:** Patterns for detecting fax numbers.

**Patterns Included:**

1. **Fax Number Detection**
   - `(?i)\b(faks|fax)[\s:.-]?\s*(\+?(\d{1,4})?[-\s\.]?(\(?\d{1,4}\)?)?[-\s\.]?\d{1,}[-\s\.]?\d{1,}[-\s\.]?\d{1,}[-\s\.]?\d{1,})\b` - Detects fax numbers with keyword context:
     - **Keywords**: Supports Turkish (faks) and English (fax) keywords
     - **Case Insensitive**: Pattern uses case-insensitive matching
     - **International Format**: Supports optional country code with `+` prefix (1-4 digits)
     - **Area Code**: Optional area code in parentheses or without parentheses
     - **Format Variations**: Handles various separators (spaces, hyphens, dots, colons)
     - **Digit Groups**: Matches multiple digit groups separated by common formatting characters
     - **Context Matching**: Requires keyword context before the number to reduce false positives

### Github Credentials

**Category Description:** Patterns for detecting Github credentials.

**Patterns Included:**

1. **GitHub Token Detection**
   - `[gG][iI][tT][hH][uU][bB].*['|\"][0-9a-zA-Z]{35,40}['|\"]` - Detects GitHub tokens in quoted format:
     - **Keyword Matching**: Matches "github" keyword (case-insensitive through character class matching)
     - **Token Format**: Alphanumeric tokens with length between 35-40 characters
     - **Quoted Strings**: Detects tokens enclosed in single or double quotes
     - **Context Matching**: Requires "github" keyword before the token to reduce false positives
     - **Format**: Matches patterns like `github_token = "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"` or `github: 'ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'`


### IBAN Information

**Category Description:** Patterns for detecting IBAN numbers.

**Patterns Included:**

1. **IBAN Number Detection**
   - `\b[A-Z]{2}[0-9]{2}[ ]?(?:[0-9A-Z]{4}[ ]?){2,7}[0-9A-Z]{0,4}\b` - Detects IBAN numbers:
     - **Country Code**: Two uppercase letters (ISO 3166-1 alpha-2 country code, e.g., TR, DE, GB, FR)
     - **Check Digits**: Two digits for IBAN validation
     - **BBAN**: Basic Bank Account Number (country-specific format, typically 15-30 characters)
     - **Format**: Supports both spaced and non-spaced formats (e.g., `TR33 0006 1005 1978 6457 8413 26` or `TR330006100519786457841326`)
     - **Grouping**: Matches IBANs formatted in 4-character groups separated by optional spaces
     - **Length**: Total length typically ranges from 15 to 34 characters depending on the country
     - **Word Boundaries**: Uses word boundaries to prevent partial matches



### IP Addresses

**Category Description:** Patterns for detecting IP addresses.

**Patterns Included:**

1. **IPv4 Address Detection**
   - `(?<![A-Za-z0-9._/-])(?:(?:10|172\.(?:1[6-9]|2\d|3[01])|192\.168)\.(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)\.(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)\.(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)|(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)\.(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)\.(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)\.(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d))(?![A-Za-z0-9._/-])` - Detects IPv4 addresses with enhanced precision:
     - **Valid IPv4 Format**: Matches all valid IPv4 addresses with octets in the range 0-255
     - **Private IP Ranges**: Specifically covers RFC 1918 private IP address ranges:
       - `10.0.0.0/8` - Class A private network (10.x.x.x)
       - `172.16.0.0/12` - Class B private network (172.16.x.x - 172.31.x.x)
       - `192.168.0.0/16` - Class C private network (192.168.x.x)
     - **Public IP Addresses**: Also matches all public IPv4 addresses
     - **Octet Validation**: Each octet is validated to be in the range 0-255 using pattern `(?:25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)`
     - **False Positive Prevention**: Uses negative lookbehind `(?<![A-Za-z0-9._/-])` and negative lookahead `(?![A-Za-z0-9._/-])` to prevent matching IPs within:
       - URLs (e.g., `http://192.168.1.1` won't match the IP part incorrectly)
       - Tokens or identifiers (e.g., `api_key_192.168.1.1_token` won't match)
       - File paths or version numbers (e.g., `v1.2.3.4` won't match)
     - **Standalone Matching**: Ensures IP addresses are matched as complete, standalone entities


### Mac Addresses

**Category Description:** Patterns for detecting MAC addresses.

**Patterns Included:**

1. **MAC Address Detection**
   - `^([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})$` - Detects MAC addresses:
     - **Format**: Six groups of two hexadecimal digits (0-9, A-F, a-f)
     - **Separator Support**: Supports both colon (`:`) and hyphen (`-`) separators
     - **Standard Format**: Matches IEEE 802 standard MAC address format
     - **Examples**:
       - `00:1A:2B:3C:4D:5E` (colon-separated)
       - `00-1A-2B-3C-4D-5E` (hyphen-separated)
       - `aa:bb:cc:dd:ee:ff` (lowercase)
       - `AA:BB:CC:DD:EE:FF` (uppercase)
     - **Anchored**: Uses line anchors (`^` and `$`) to ensure complete MAC address matching
     - **Length**: Total of 12 hexadecimal digits (48 bits)



### OpenAI Credentials

**Category Description:** Patterns for detecting OpenAI credentials.

**Patterns Included:**

1. **OpenAI API Key Detection**
   - `sk-(?:live|test|proj|svcacct)?-[a-zA-Z0-9\-_]{80,}` - Detects OpenAI API keys:
     - **Prefix**: All OpenAI keys start with `sk-` (secret key)
     - **Key Types**: Supports multiple OpenAI key types with optional type identifiers:
       - `sk-live-` - Live/production API keys
       - `sk-test-` - Test/development API keys
       - `sk-proj-` - Project-specific API keys
       - `sk-svcacct-` - Service account API keys
       - `sk-` - Legacy format without type identifier
     - **Key Format**: Alphanumeric characters, hyphens, and underscores
     - **Length**: Minimum 80 characters after the prefix (typical OpenAI key length is 80-100+ characters)
     - **Examples**:
       - `sk-proj-AbCdEfGh1234567890...` (project key)
       - `sk-svcacct-XyZ123456789...` (service account key)
       - `sk-1234567890abcdef...` (legacy format)



### Password

**Category Description:** Patterns for detecting passwords.

**Patterns Included:**

1. **Password Assignment Detection**
   - `["\x27]?(?:password|passcode|parola|şifre|sifre|pwd|passphrase)["\x27]?\s*[:=]\s*["\x27]?(?!var\.)(?!\$\{)(?!os\.getenv)(?!os\.environ)(?!process\.env)(?![A-Za-z0-9._]+_password\b)(?=[^\s"\x27]{8,64})(?=[^\s"\x27]*[A-Za-z])(?=[^\s"\x27]*\d)([^\s"\x27]{8,64})["\x27]?(?:[;.,])?` - Detects hardcoded passwords with contextual filtering:
     - **Keywords**: Supports multiple languages and common password field names:
       - English: `password`, `passcode`, `pwd`, `passphrase`
       - Turkish: `parola`, `şifre`, `sifre`
     - **Quote Support**: Matches passwords with or without surrounding single/double quotes
     - **Assignment Operators**: Supports both colon (`:`) and equals (`=`) for assignments
     - **False Positive Filtering**: Uses negative lookahead to exclude common false positives:
       - `(?!var\.)` - Excludes variable references like `var.password`
       - `(?!\$\{)` - Excludes template literals like `${password}`
       - `(?!os\.getenv)` - Excludes Python environment variable reads
       - `(?!os\.environ)` - Excludes Python environment dictionary access
       - `(?!process\.env)` - Excludes Node.js environment variable reads
       - `(?![A-Za-z0-9._]+_password\b)` - Excludes variable names ending with `_password`
     - **Password Validation**: Uses positive lookahead to ensure realistic password patterns:
       - Length: 8-64 characters
       - Must contain at least one letter (`(?=[^\s"\x27]*[A-Za-z])`)
       - Must contain at least one digit (`(?=[^\s"\x27]*\d)`)
     - **Format Support**: Handles common syntax endings (semicolons, commas, periods)
     - **Examples of matches**:
       - `password = "MyP@ssw0rd123"`
       - `pwd: 'SecurePass2024'`
       - `parola="Güvenli123"`
     - **Examples of non-matches**:
       - `password = os.getenv("DB_PASSWORD")` (environment variable)
       - `user_password = some_variable` (variable reference)
       - `password = ${PASSWORD}` (template literal)

### Phone Numbers

**Category Description:** Patterns for detecting phone numbers in Turkey.

**Patterns Included:**

1. **Formatted Phone Numbers with Parentheses**

   - `0 (\d{3}) \d{3} \d{2} \d{2}` - Turkish phone numbers with space and parentheses format:
     - **Format**: `0 (XXX) XXX XX XX`
     - **Example**: `0 (555) 123 45 67`
     - **Usage**: Common format for mobile numbers with visual grouping
2. **Space-Separated Format**

   - `0\d{3} \d{3} \d{2} \d{2}` - Turkish phone numbers with space-separated format:
     - **Format**: `0XXX XXX XX XX`
     - **Example**: `0555 123 45 67`
     - **Usage**: Simplified format without parentheses
3. **International Format with Country Code (+90)**

   - `\+90 \d{3} \d{3} \d{4}` - Turkish phone numbers in international format:
     - **Format**: `+90 XXX XXX XXXX`
     - **Example**: `+90 555 123 4567`
     - **Usage**: International dialing format with 10 consecutive digits after country code
4. **International Format with Grouped Digits**

   - `\+90 \d{3} \d{3} \d{2} \d{2}` - Turkish phone numbers with international code and grouped format:
     - **Format**: `+90 XXX XXX XX XX`
     - **Example**: `+90 555 123 45 67`
     - **Usage**: International format with visual digit grouping
5. **International Format with Hyphens**

   - `\+90-\d{3}-\d{3}-\d{2}-\d{2}` - Turkish phone numbers with hyphen separators:
     - **Format**: `+90-XXX-XXX-XX-XX`
     - **Example**: `+90-555-123-45-67`
     - **Usage**: Hyphen-separated international format
6. **Contextual Phone Number Detection**

   - `(?i)\b(telefon|tel|gsm|cep|cell(?:ular)?(?:\s*no)?|phone(?:\s*number)?)\b.{0,30}?(?:\+?90\s*|0)?\s*\(?\d{3}\)?[\s.-]*\d{3}[\s.-]*\d{2}[\s.-]*\d{2}` - Context-aware phone number detection:
     - **Keywords**: Supports Turkish and English keywords:
       - Turkish: `telefon`, `tel`, `gsm`, `cep`
       - English: `cell`, `cellular`, `phone`, `phone number`
     - **Case Insensitive**: Pattern uses case-insensitive matching
     - **Flexible Format**: Matches various separators (spaces, dots, hyphens)
     - **Optional Country Code**: Supports both `+90` and `0` prefixes, or no prefix
     - **Context Window**: Allows up to 30 characters between keyword and number
     - **Separator Support**: Handles mixed separators (`-`, `.`, space)
     - **Examples**:
       - `Telefon: 0 (555) 123 45 67`
       - `GSM: +90 555 123 45 67`
       - `Phone: 555-123-45-67`
       - `Cep no: 0555.123.45.67`
7. **Strict International Format with Parentheses**

   - `^\+90\s*\(\d{3}\)\s*\d{3}\s\d{2}\s\d{2}$` - Anchored international format with strict validation:
     - **Format**: `+90 (XXX) XXX XX XX`
     - **Example**: `+90 (555) 123 45 67`
     - **Anchored**: Uses line anchors (`^` and `$`) to ensure complete number matching
     - **Usage**: Strict validation for standalone phone number entries


### Private Keys and Certificates

**Category Description:** Patterns for detecting private keys and certificates.

**Patterns Included:**

1. **Private Key Detection**

   - `(?s)(-----BEGIN (?:PRIVATE|RSA PRIVATE|DSA PRIVATE|EC PRIVATE) KEY-----.*?-----END (?:PRIVATE|RSA PRIVATE|DSA PRIVATE|EC PRIVATE) KEY-----)` - Detects various types of private keys in PEM format:
     - **DOTALL Mode**: `(?s)` enables multiline matching
     - **Key Types**: PRIVATE KEY (PKCS#8), RSA PRIVATE KEY (PKCS#1), DSA PRIVATE KEY, EC PRIVATE KEY
     - **Structure**: Matches complete PEM format including headers, base64-encoded key data, and footers
     - **Non-Greedy Matching**: Uses `.*?` to match individual keys when multiple are present
2. **SSL/TLS Certificate Detection**

   - `(?s)(-----BEGIN CERTIFICATE-----.*?-----END CERTIFICATE-----)` - Detects SSL/TLS certificates in PEM format:
     - **DOTALL Mode**: `(?s)` enables multiline matching
     - **Format**: Matches X.509 certificates in PEM encoding
     - **Use Cases**: SSL/TLS certificates, code signing, client authentication, CA certificates
     - **Non-Greedy Matching**: Captures individual certificates in certificate chains


### Slack Credentials

**Category Description:** Patterns for detecting Slack credentials.

**Patterns Included:**

1. **Slack API Token Detection**
   - `xox[baprs]-\d{4,20}-\d{4,20}-\w{8,24}` - Detects Slack API tokens:
     - **Prefix**: All Slack tokens start with `xox` followed by a type identifier
     - **Token Types**:
       - `xoxb-` - Bot tokens
       - `xoxa-` - Access tokens
       - `xoxp-` - User tokens
       - `xoxr-` - Refresh tokens
       - `xoxs-` - Session tokens
     - **Format**: Three segments separated by hyphens
       - First segment: 4-20 digits (workspace ID)
       - Second segment: 4-20 digits (token ID)
       - Third segment: 8-24 alphanumeric characters (secret part)
     - **Examples**: `xoxb-1234567890-1234567890-abcdefghijklmnop`, `xoxp-1234-5678-9012-abc123`


### Slack Webhook

**Category Description:** Patterns for detecting Slack webhooks.

**Patterns Included:**

1. **Slack Webhook URL Detection**
   - `(http://|https://)*hooks.slack.com/services/T[a-zA-Z0-9_]+/B[a-zA-Z0-9_]+/[a-zA-Z0-9_]+` - Detects Slack webhook URLs:
     - **Domain**: `hooks.slack.com/services/`
     - **Structure**: Three path segments
       - First segment: Starts with `T` (Team/Workspace ID)
       - Second segment: Starts with `B` (Bot/Channel ID)
       - Third segment: Alphanumeric secret token
     - **Protocol**: Supports both HTTP and HTTPS
     - **Example**: `https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXX`


### Turkish Identity Numbers

**Category Description:** Patterns for detecting Turkish identity numbers.

**Patterns Included:**

1. **Turkish Identity Number Detection**
   - `\b(?:TC[\s_\.]?Kimlik[\s_\.]?no|T\.C\.[\s_\.]?Kimlik[\s_\.]?no|kimlik[\s_\.]?no|kimlik[\s_\.]?numarası|T\.C\.[\s_\.]?kimlik[\s_\.]?numarası|tc[\s_\.]?kimlik[\s_\.]?no|tc[\s_\.]?kimlik[\s_\.]?numarası|identification[\s_\.]?number|ID[\s_\.]?number|TCKN|tc[\s_\.]?no|National[\s_\.]?Number|National[\s_\.]?Identity[\s_\.]?Number|person[\s_\.]?ID)\b[\s:._-]{0,5}["\x27]*(\d{10}[0,2,4,6,8])["\x27]*` - Detects Turkish identity numbers with context:
     - **Keywords**: Supports Turkish and English terms:
       - Turkish: `TC Kimlik no`, `T.C. Kimlik no`, `kimlik no`, `kimlik numarası`, `TCKN`, `tc no`
       - English: `identification number`, `ID number`, `National Number`, `National Identity Number`, `person ID`
     - **Format**: 11-digit number where the last digit must be even (0, 2, 4, 6, or 8)
     - **Validation**: Pattern enforces the last digit constraint (checksum rule)
     - **Separators**: Handles various separators between keyword and number (`:`, `.`, `_`, `-`, space)
     - **Quote Support**: Matches numbers with or without surrounding quotes
     - **Examples**: `TC Kimlik no: 12345678902`, `TCKN: "12345678904"`, `Identity Number: 98765432108`


### Turkish Tax Numbers

**Category Description:** Patterns for detecting Turkish tax numbers.

**Patterns Included:**

1. **Turkish Tax Number Detection**
   - `\b(?:Vergi[\s_\.]?no|Vergi[\s_\.]?kimlik[\s_\.]?numarası|Vergi[\s_\.]?kimlik[\s_\.]?no|VKN|Tax[\s_\.]?ID|Tax[\s_\.]?Number|Tax[\s_\.]?Identification[\s_\.]?Number)\b[\s:._-]{0,5}["\x27]*(\d{10})["\x27]*` - Detects Turkish tax numbers with context:
     - **Keywords**: Supports Turkish and English terms:
       - Turkish: `Vergi no`, `Vergi kimlik numarası`, `Vergi kimlik no`, `VKN`
       - English: `Tax ID`, `Tax Number`, `Tax Identification Number`
     - **Format**: Exactly 10 digits
     - **Separators**: Handles various separators between keyword and number (`:`, `.`, `_`, `-`, space)
     - **Quote Support**: Matches numbers with or without surrounding quotes
     - **Case Insensitive**: Pattern matches keywords in various cases
     - **Examples**: `Vergi no: 1234567890`, `VKN: "9876543210"`, `Tax ID: 1122334455`

### Passport Number

**Category Description:** Patterns for detecting passport numbers starting with 'U' followed by 8 digits, including Turkish passport formats.

**Patterns Included:**

1. **Passport Number Detection**
   - `(?i)\b(?:pasaport|passport|p_no|pasaport_no|psprt|belge)\b[^\w]{0,10}?\b(U\d{8})\b` - Detects passport numbers with context:
     - **Keywords**: Supports Turkish and English terms:
       - Turkish: `pasaport`, `p no`, `pasaport no`, `belge`
       - English: `passport`, `psprt`
     - **Format**: Letter `U` followed by exactly 8 digits (Turkish passport format)
     - **Context Window**: Allows up to 10 non-word characters between keyword and number
     - **Case Insensitive**: Pattern matches keywords in various cases
     - **Examples**: `Pasaport no: U12345678`, `Passport: U87654321`, `belge U11223344`


### Turkish ID Serial Number

**Category Description:** Patterns for detecting Turkish ID Card serial numbers (e.g., A12Z34567). Format consists of a letter, two digits, another letter, and five digits (9 characters total). Uses Turkish alphabet letters (excludes Q, W, X).

**Patterns Included:**

1. **Turkish ID Serial Number Detection**
   - `(?i)\b(?:seri\s*no|seri\s*numarası|id\s*serial|kimlik\s*seri|kimlik\s*seri\s*no)\b[^\w]{0,10}?\b([ABCDEFGHIJKLMNOPRSTUVYZ]\d{2}[ABCDEFGHIJKLMNOPRSTUVYZ]\d{5})\b` - Detects Turkish ID serial numbers with context:
     - **Keywords**: Supports Turkish and English terms:
       - Turkish: `seri no`, `seri numarası`, `kimlik seri`, `kimlik seri no`
       - English: `id serial`
     - **Format**: 9 characters — letter + 2 digits + letter + 5 digits (e.g. `A12Z34567`)
     - **Letter Set**: Turkish ID uses letters A–Z excluding Q, W, X
     - **Context Window**: Allows up to 10 non-word characters between keyword and serial
     - **Case Insensitive**: Keyword matching is case insensitive
     - **Examples**: `Seri no: A12Z34567`, `Kimlik seri B34K56789`, `ID serial: C56M90123`

### Mac Addresses

**Category Description:** Patterns for detecting MAC addresses.

**Patterns Included:**

1. **MAC Address Detection**
   - `^([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})$` - Detects MAC addresses:
     - **Format**: Six groups of two hexadecimal digits (0-9, A-F, a-f)
     - **Separator Support**: Supports both colon (`:`) and hyphen (`-`) separators
     - **Standard Format**: Matches IEEE 802 standard MAC address format
     - **Examples**:
       - `00:1A:2B:3C:4D:5E` (colon-separated)
       - `00-1A-2B-3C-4D-5E` (hyphen-separated)
       - `aa:bb:cc:dd:ee:ff` (lowercase)
       - `AA:BB:CC:DD:EE:FF` (uppercase)
     - **Anchored**: Uses line anchors (`^` and `$`) to ensure complete MAC address matching
     - **Length**: Total of 12 hexadecimal digits (48 bits)

### OpenAI Credentials

**Category Description:** Patterns for detecting OpenAI credentials.

**Patterns Included:**

1. **OpenAI API Key Detection**
   - `sk-(?:live|test|proj|svcacct)?-[a-zA-Z0-9\-_]{80,}` - Detects OpenAI API keys:
     - **Prefix**: All OpenAI keys start with `sk-` (secret key)
     - **Key Types**: Supports multiple OpenAI key types with optional type identifiers:
       - `sk-live-` - Live/production API keys
       - `sk-test-` - Test/development API keys
       - `sk-proj-` - Project-specific API keys
       - `sk-svcacct-` - Service account API keys
       - `sk-` - Legacy format without type identifier
     - **Key Format**: Alphanumeric characters, hyphens, and underscores
     - **Length**: Minimum 80 characters after the prefix (typical OpenAI key length is 80-100+ characters)
     - **Examples**:
       - `sk-proj-AbCdEfGh1234567890...` (project key)
       - `sk-svcacct-XyZ123456789...` (service account key)
       - `sk-1234567890abcdef...` (legacy format)

### Password

**Category Description:** Patterns for detecting passwords.

**Patterns Included:**

1. **Password Assignment Detection**
   - `["\x27]?(?:password|passcode|parola|şifre|sifre|pwd|passphrase)["\x27]?\s*[:=]\s*["\x27]?(?!var\.)(?!\$\{)(?!os\.getenv)(?!os\.environ)(?!process\.env)(?![A-Za-z0-9._]+_password\b)(?=[^\s"\x27]{8,64})(?=[^\s"\x27]*[A-Za-z])(?=[^\s"\x27]*\d)([^\s"\x27]{8,64})["\x27]?(?:[;.,])?` - Detects hardcoded passwords with contextual filtering:
     - **Keywords**: Supports multiple languages and common password field names:
       - English: `password`, `passcode`, `pwd`, `passphrase`
       - Turkish: `parola`, `şifre`, `sifre`
     - **Quote Support**: Matches passwords with or without surrounding single/double quotes
     - **Assignment Operators**: Supports both colon (`:`) and equals (`=`) for assignments
     - **False Positive Filtering**: Uses negative lookahead to exclude common false positives:
       - `(?!var\.)` - Excludes variable references like `var.password`
       - `(?!\$\{)` - Excludes template literals like `${password}`
       - `(?!os\.getenv)` - Excludes Python environment variable reads
       - `(?!os\.environ)` - Excludes Python environment dictionary access
       - `(?!process\.env)` - Excludes Node.js environment variable reads
       - `(?![A-Za-z0-9._]+_password\b)` - Excludes variable names ending with `_password`
     - **Password Validation**: Uses positive lookahead to ensure realistic password patterns:
       - Length: 8-64 characters
       - Must contain at least one letter (`(?=[^\s"\x27]*[A-Za-z])`)
       - Must contain at least one digit (`(?=[^\s"\x27]*\d)`)
     - **Format Support**: Handles common syntax endings (semicolons, commas, periods)
     - **Examples of matches**:
       - `password = "MyP@ssw0rd123"`
       - `pwd: 'SecurePass2024'`
       - `parola="Güvenli123"`
     - **Examples of non-matches**:
       - `password = os.getenv("DB_PASSWORD")` (environment variable)
     - `user_password = some_variable` (variable reference)
     - `password = ${PASSWORD}` (template literal)

### Phone Numbers

**Category Description:** Patterns for detecting phone numbers in Turkey.

**Patterns Included:**

1. **Formatted Phone Numbers with Parentheses**

   - `0 (\d{3}) \d{3} \d{2} \d{2}` - Turkish phone numbers with space and parentheses format:
     - **Format**: `0 (XXX) XXX XX XX`
     - **Example**: `0 (555) 123 45 67`
     - **Usage**: Common format for mobile numbers with visual grouping

2. **Space-Separated Format**

   - `0\d{3} \d{3} \d{2} \d{2}` - Turkish phone numbers with space-separated format:
     - **Format**: `0XXX XXX XX XX`
     - **Example**: `0555 123 45 67`
     - **Usage**: Simplified format without parentheses

3. **International Format with Country Code (+90)**

   - `\+90 \d{3} \d{3} \d{4}` - Turkish phone numbers in international format:
     - **Format**: `+90 XXX XXX XXXX`
     - **Example**: `+90 555 123 4567`
     - **Usage**: International dialing format with 10 consecutive digits after country code

4. **International Format with Grouped Digits**

   - `\+90 \d{3} \d{3} \d{2} \d{2}` - Turkish phone numbers with international code and grouped format:
     - **Format**: `+90 XXX XXX XX XX`
     - **Example**: `+90 555 123 45 67`
     - **Usage**: International format with visual digit grouping

5. **International Format with Hyphens**

   - `\+90-\d{3}-\d{3}-\d{2}-\d{2}` - Turkish phone numbers with hyphen separators:
     - **Format**: `+90-XXX-XXX-XX-XX`
     - **Example**: `+90-555-123-45-67`
     - **Usage**: Hyphen-separated international format

6. **Contextual Phone Number Detection**

   - `(?i)\b(telefon|tel|gsm|cep|cell(?:ular)?(?:\s*no)?|phone(?:\s*number)?)\b.{0,30}?(?:\+?90\s*|0)?\s*\(?\d{3}\)?[\s.-]*\d{3}[\s.-]*\d{2}[\s.-]*\d{2}` - Context-aware phone number detection:
     - **Keywords**: Supports Turkish and English keywords:
       - Turkish: `telefon`, `tel`, `gsm`, `cep`
       - English: `cell`, `cellular`, `phone`, `phone number`
     - **Case Insensitive**: Pattern uses case-insensitive matching
     - **Flexible Format**: Matches various separators (spaces, dots, hyphens)
     - **Optional Country Code**: Supports both `+90` and `0` prefixes, or no prefix
     - **Context Window**: Allows up to 30 characters between keyword and number
     - **Separator Support**: Handles mixed separators (`-`, `.`, space)
     - **Examples**:
       - `Telefon: 0 (555) 123 45 67`
       - `GSM: +90 555 123 45 67`
       - `Phone: 555-123-45-67`
       - `Cep no: 0555.123.45.67`

7. **Strict International Format with Parentheses**
   - `^\+90\s*\(\d{3}\)\s*\d{3}\s\d{2}\s\d{2}$` - Anchored international format with strict validation:
     - **Format**: `+90 (XXX) XXX XX XX`
     - **Example**: `+90 (555) 123 45 67`
     - **Anchored**: Uses line anchors (`^` and `$`) to ensure complete number matching
     - **Usage**: Strict validation for standalone phone number entries

### Private Keys and Certificates

**Category Description:** Patterns for detecting private keys and certificates.

**Patterns Included:**

1. **Private Key Detection**

   - `(?s)(-----BEGIN (?:PRIVATE|RSA PRIVATE|DSA PRIVATE|EC PRIVATE) KEY-----.*?-----END (?:PRIVATE|RSA PRIVATE|DSA PRIVATE|EC PRIVATE) KEY-----)` - Detects various types of private keys in PEM format:
     - **DOTALL Mode**: `(?s)` enables multiline matching
     - **Key Types**: PRIVATE KEY (PKCS#8), RSA PRIVATE KEY (PKCS#1), DSA PRIVATE KEY, EC PRIVATE KEY
     - **Structure**: Matches complete PEM format including headers, base64-encoded key data, and footers
     - **Non-Greedy Matching**: Uses `.*?` to match individual keys when multiple are present

2. **SSL/TLS Certificate Detection**
   - `(?s)(-----BEGIN CERTIFICATE-----.*?-----END CERTIFICATE-----)` - Detects SSL/TLS certificates in PEM format:
     - **DOTALL Mode**: `(?s)` enables multiline matching
     - **Format**: Matches X.509 certificates in PEM encoding
     - **Use Cases**: SSL/TLS certificates, code signing, client authentication, CA certificates
     - **Non-Greedy Matching**: Captures individual certificates in certificate chains

### Slack Credentials

**Category Description:** Patterns for detecting Slack credentials.

**Patterns Included:**

1. **Slack API Token Detection**
   - `xox[baprs]-\d{4,20}-\d{4,20}-\w{8,24}` - Detects Slack API tokens:
     - **Prefix**: All Slack tokens start with `xox` followed by a type identifier
     - **Token Types**:
       - `xoxb-` - Bot tokens
       - `xoxa-` - Access tokens
       - `xoxp-` - User tokens
       - `xoxr-` - Refresh tokens
       - `xoxs-` - Session tokens
     - **Format**: Three segments separated by hyphens
       - First segment: 4-20 digits (workspace ID)
       - Second segment: 4-20 digits (token ID)
       - Third segment: 8-24 alphanumeric characters (secret part)
     - **Examples**: `xoxb-1234567890-1234567890-abcdefghijklmnop`, `xoxp-1234-5678-9012-abc123`

### Slack Webhook

**Category Description:** Patterns for detecting Slack webhooks.

**Patterns Included:**

1. **Slack Webhook URL Detection**
   - `(http://|https://)*hooks.slack.com/services/T[a-zA-Z0-9_]+/B[a-zA-Z0-9_]+/[a-zA-Z0-9_]+` - Detects Slack webhook URLs:
     - **Domain**: `hooks.slack.com/services/`
     - **Structure**: Three path segments
       - First segment: Starts with `T` (Team/Workspace ID)
       - Second segment: Starts with `B` (Bot/Channel ID)
       - Third segment: Alphanumeric secret token
     - **Protocol**: Supports both HTTP and HTTPS
     - **Example**: `https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXX`

### Turkish Identity Numbers

**Category Description:** Patterns for detecting Turkish identity numbers.

**Patterns Included:**

1. **Turkish Identity Number Detection**
   - `\b(?:TC[\s_\.]?Kimlik[\s_\.]?no|T\.C\.[\s_\.]?Kimlik[\s_\.]?no|kimlik[\s_\.]?no|kimlik[\s_\.]?numarası|T\.C\.[\s_\.]?kimlik[\s_\.]?numarası|tc[\s_\.]?kimlik[\s_\.]?no|tc[\s_\.]?kimlik[\s_\.]?numarası|identification[\s_\.]?number|ID[\s_\.]?number|TCKN|tc[\s_\.]?no|National[\s_\.]?Number|National[\s_\.]?Identity[\s_\.]?Number|person[\s_\.]?ID)\b[\s:._-]{0,5}["\x27]*(\d{10}[0,2,4,6,8])["\x27]*` - Detects Turkish identity numbers with context:
     - **Keywords**: Supports Turkish and English terms:
       - Turkish: `TC Kimlik no`, `T.C. Kimlik no`, `kimlik no`, `kimlik numarası`, `TCKN`, `tc no`
       - English: `identification number`, `ID number`, `National Number`, `National Identity Number`, `person ID`
     - **Format**: 11-digit number where the last digit must be even (0, 2, 4, 6, or 8)
     - **Validation**: Pattern enforces the last digit constraint (checksum rule)
     - **Separators**: Handles various separators between keyword and number (`:`, `.`, `_`, `-`, space)
     - **Quote Support**: Matches numbers with or without surrounding quotes
     - **Examples**: `TC Kimlik no: 12345678902`, `TCKN: "12345678904"`, `Identity Number: 98765432108`

### Turkish Tax Numbers

**Category Description:** Patterns for detecting Turkish tax numbers.

**Patterns Included:**

1. **Turkish Tax Number Detection**
   - `\b(?:Vergi[\s_\.]?no|Vergi[\s_\.]?kimlik[\s_\.]?numarası|Vergi[\s_\.]?kimlik[\s_\.]?no|VKN|Tax[\s_\.]?ID|Tax[\s_\.]?Number|Tax[\s_\.]?Identification[\s_\.]?Number)\b[\s:._-]{0,5}["\x27]*(\d{10})["\x27]*` - Detects Turkish tax numbers with context:
     - **Keywords**: Supports Turkish and English terms:
       - Turkish: `Vergi no`, `Vergi kimlik numarası`, `Vergi kimlik no`, `VKN`
       - English: `Tax ID`, `Tax Number`, `Tax Identification Number`
     - **Format**: Exactly 10 digits
     - **Separators**: Handles various separators between keyword and number (`:`, `.`, `_`, `-`, space)
     - **Quote Support**: Matches numbers with or without surrounding quotes
     - **Case Insensitive**: Pattern matches keywords in various cases
     - **Examples**: `Vergi no: 1234567890`, `VKN: "9876543210"`, `Tax ID: 1122334455`

---

## Contributing

Contributions are welcome! Please follow these guidelines:

1. **Context-aware is required**: If applicable, patterns must use keyword pre-matching to reduce false positives
2. **Test with examples**: Include what should match and what should NOT match
3. **Keep it simple**: Follow the existing YAML structure in `regexes.yml`

Example contribution:

```yaml
# ✅ card_id: 5555-5555-5555-4444  → matched (has "card" keyword)
# ❌ user_id: 5555-5555-5555-4444  → not matched (no false positive)
- "(?:credit|card|kredi|kart)[^\d]{0,20}?\b(5\d{3}[\s-]?\d{4}[\s-]?\d{4}[\s-]?\d{4})\b"
```

---

## Authors

- Alp Keskin <@alpkeskin>
- Burak Tahtacı <@tahtaciburak>
- Emre Kaşkaval <@ekaskaval>
- Enes Can Güven <@enescanguven>
- Zeynel Acar <@zeynelacar>
- İlhami Selamet <@ilhamiselamet>
