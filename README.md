# CMSMS Module Rules

Shared rule definitions used by [ModuleChecker](https://dev.cmsmadesimple.org/projects/modulechecker) (PHP/CMSMS admin)

## Overview

This repository contains the JSON rule definitions that power automated module scanning. Both projects consume these rules from their own `rules/` directory.

```
rules/
├── cmsms/       # 36 CMSMS compliance rules (CMSMS_CMS_001-036)
├── security/    # 20 security rules (CMSMS_SEC_001-020)
├── quality/     # 17 code quality rules (CMSMS_QUAL_001-017)
└── README.md
```

## Usage

**ModuleChecker** loads rules via `lib/class.RuleEngine.php` at scan time.
**cmsms-scanner** loads rules via `scanner/rules.py` at CLI invocation.

Both projects should point to (or clone) this repo into their `rules/` directory.

---

## Rule Reference

This document lists all automated rules enforced by ModuleChecker. Rules are organized into three categories: **CMSMS Compliance**, **Security**, and **Code Quality**.

Each rule has a severity level indicating how critical it is:

| Severity | Meaning |
|----------|---------|
| **high** | Must fix before distribution. Security vulnerabilities, broken installs, or major compliance failures. |
| **medium** | Should fix. Deprecated APIs, naming collisions, missing best practices. |
| **low** | Recommended improvement. Code style, minor optimizations, informational. |
| **info** | Informational only. Documentation and licensing suggestions. |

---

## CMSMS Compliance (36 rules)

| Rule ID | Severity | Description |
|---------|----------|-------------|
| CMSMS_CMS_001 | medium | Module table without `mod_` or `module_` prefix after CMS_DB_PREFIX |
| CMSMS_CMS_002 | medium | Hardcoded absolute path instead of CMS path constants |
| CMSMS_CMS_003 | low | Hardcoded user-facing string in SetError/SetMessage |
| CMSMS_CMS_004 | medium | Deprecated `global $gCms` for database access |
| CMSMS_CMS_005 | low | `echo ProcessTemplate()` — deprecated output pattern |
| CMSMS_CMS_006 | high | Admin action file missing `CheckPermission()` call |
| CMSMS_CMS_007 | medium | Direct HTML output in action file instead of Smarty template |
| CMSMS_CMS_008 | medium | SQL query with hardcoded table name missing `CMS_DB_PREFIX` |
| CMSMS_CMS_009 | medium | Raw HTML `<form>` in frontend template instead of `{form_start}` |
| CMSMS_CMS_010 | low | Module uses another module without `GetDependencies()` |
| CMSMS_CMS_011 | medium | Template variable output without `|escape` modifier |
| CMSMS_CMS_012 | high | Smarty `{php}` tag in template |
| CMSMS_CMS_013 | medium | Generic class filename in `lib/` without module prefix |
| CMSMS_CMS_014 | low | Preference key without module name prefix |
| CMSMS_CMS_015 | medium | Permission name without module identifier |
| CMSMS_CMS_016 | low | Event name without module prefix |
| CMSMS_CMS_017 | low | Smarty variable assigned without module prefix |
| CMSMS_CMS_018 | medium | `error_reporting()` or `display_errors` set in production code |
| CMSMS_CMS_019 | medium | Undocumented use of external service |
| CMSMS_CMS_020 | medium | Missing recommended module metadata method |
| CMSMS_CMS_021 | medium | Debug output statement left in production code |
| CMSMS_CMS_022 | high | Module folder structure missing required files or directories |
| CMSMS_CMS_023 | high | PHP file missing `CMS_VERSION` direct access guard |
| CMSMS_CMS_024 | medium | Class or function defined without namespace or module prefix |
| CMSMS_CMS_025 | info | PHP file missing license header comment |
| CMSMS_CMS_026 | high | Uninstall does not clean up resources created by install |
| CMSMS_CMS_027 | high | Database table creation does not follow CMSMS conventions |
| CMSMS_CMS_028 | low | Direct `$_GET` access instead of `$params` in action file |
| CMSMS_CMS_029 | low | Legacy `cmsms()->GetDb()` instead of `cms_utils::get_db()` |
| CMSMS_CMS_030 | medium | Module missing permission constants |
| CMSMS_CMS_031 | medium | Deprecated `$this->smarty` module property |
| CMSMS_CMS_032 | medium | Deprecated module parameter registration methods |
| CMSMS_CMS_033 | medium | Deprecated `GetNotificationOutput()` method |
| CMSMS_CMS_034 | low | Deprecated `cms_html_entity_decode()` function |
| CMSMS_CMS_035 | medium | Deprecated `strftime()` function |
| CMSMS_CMS_036 | high | Dependency on removed core module MenuManager |

---

## Security (20 rules)

| Rule ID | Severity | Description |
|---------|----------|-------------|
| CMSMS_SEC_001 | high | User input flows into SQL query |
| CMSMS_SEC_002 | high | User input flows into echo/print output (XSS) |
| CMSMS_SEC_003 | high | User input flows into file path operation |
| CMSMS_SEC_004 | high | User input flows into include/require |
| CMSMS_SEC_005 | high | User input flows into shell command |
| CMSMS_SEC_006 | high | SQL query with concatenated variable after WHERE/SET/VALUES |
| CMSMS_SEC_007 | high | SQL query with PHP variable interpolation in double-quoted string |
| CMSMS_SEC_008 | medium | LIKE clause with concatenated search term |
| CMSMS_SEC_009 | high | Dynamic ORDER BY or table name from user input |
| CMSMS_SEC_010 | high | `eval()` with variable argument |
| CMSMS_SEC_011 | high | `unserialize()` on external or variable data |
| CMSMS_SEC_012 | high | File upload with user-controlled destination filename |
| CMSMS_SEC_013 | medium | Frontend form submission handler without CSRF validation |
| CMSMS_SEC_014 | high | Hardcoded credential assigned to variable |
| CMSMS_SEC_015 | high | User input decoded from base64 then used in dangerous sink |
| CMSMS_SEC_016 | high | `preg_replace` with `/e` modifier on user input |
| CMSMS_SEC_017 | high | Header injection via user input in `header()` |
| CMSMS_SEC_018 | high | Code obfuscation tool detected (Zend/SourceGuardian/ionCube) |
| CMSMS_SEC_019 | high | Backtick operator used for shell execution |
| CMSMS_SEC_020 | high | Dangerous function used (passthru, proc_open, dl, str_rot13, popen, pcntl_exec) |

---

## Code Quality (17 rules)

| Rule ID | Severity | Description |
|---------|----------|-------------|
| CMSMS_QUAL_001 | low | Inline `<script>` block in PHP echo output |
| CMSMS_QUAL_002 | low | Large inline `<style>` block in template |
| CMSMS_QUAL_003 | medium | Database query inside a loop body (N+1 problem) |
| CMSMS_QUAL_004 | high | Database access in Smarty template |
| CMSMS_QUAL_005 | medium | Error suppression operator (`@`) on I/O or parsing function |
| CMSMS_QUAL_006 | medium | Action file mixes DB queries with HTML echo |
| CMSMS_QUAL_007 | medium | Deeply nested control structures (4+ levels) |
| CMSMS_QUAL_008 | low | Overly large file (500+ lines) |
| CMSMS_QUAL_009 | low | `SELECT *` in production query |
| CMSMS_QUAL_010 | medium | Catch block that silently swallows exception |
| CMSMS_QUAL_011 | medium | Deprecated PHP function used |
| CMSMS_QUAL_012 | medium | Localhost or loopback URL hardcoded |
| CMSMS_QUAL_013 | medium | Form submit handler without POST-Redirect-Get pattern |
| CMSMS_QUAL_014 | high | Disallowed file type in module directory |
| CMSMS_QUAL_015 | medium | Short PHP open tag used instead of `<?php` |
| CMSMS_QUAL_016 | medium | URL shortener link detected |
| CMSMS_QUAL_017 | medium | Heredoc syntax with variable interpolation |

---

## Scoring (ModuleChecker)

- Every module starts at **100 points**
- Each unique **error**: severity x 2 points deducted
- Each unique **warning**: severity x 1 point deducted
- **PASS**: Score >= 70 with no errors
- **WARNING**: Score 70-100 with errors
- **FAIL**: Score < 70 or critical findings

---

## Rule Schema

Each JSON rule file follows this structure:

```json
{
  "rule_id": "CMSMS_SEC_001",
  "title": "Short description",
  "category": "security|cmsms|quality",
  "severity": "high|medium|low|info",
  "confidence": "high|medium|low",
  "description": "What this rule detects",
  "detection": {
    "type": "pattern|heuristic",
    "patterns": ["regex1", "regex2"]
  },
  "applies_to": ["php", "tpl"],
  "explanation": "Why this matters",
  "fix": "How to fix it",
  "examples": { "bad": "...", "good": "..." },
  "exclude_if": [
    { "type": "contains|regex|filename_regex", "value": "..." }
  ],
  "tags": ["sql", "injection"],
  "score": 10,
  "enabled": true
}
```

## Contributing

1. Create a JSON file in the appropriate `rules/` subdirectory
2. Follow the schema above
3. Test patterns against known bad/good code
4. Add `exclude_if` conditions to reduce false positives
5. Both consumers auto-load new rules on next scan
