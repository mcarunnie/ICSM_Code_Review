# ICSM Code Review — Claude Code Project

This project uses Claude Code with the `abap-adt` MCP server to perform automated code reviews on ABAP objects in the SAP system.

## How to Run a Review

Simply tell Claude Code:
- "Review class ZCL_MY_CLASS"
- "Review report Z_MY_REPORT"
- "Review function module Z_MY_FM"

Claude will fetch the source code from SAP and review it against the checks below.

---

## SAP Connection

The `abap-adt` MCP server is pre-configured in `.claude.json`. Update the `env` section with your SAP credentials before use.

---

## Code Review Checks

When performing a code review, always check ALL of the following categories and report findings grouped by category.

### 1. Naming Conventions
> _(To be defined — add ICSM-specific naming rules here)_
- Variables: `lv_` (local variable), `lt_` (local table), `ls_` (local structure), `lo_` (local object)
- Parameters: `iv_` (importing), `ev_` (exporting), `rv_` (returning), `ct_` (changing table)
- Global: `gv_`, `gt_`, `gs_`, `go_`
- Classes: `ZCL_`, interfaces: `ZIF_`, methods: descriptive verb_noun

### 2. Performance
- SELECT * used instead of specific fields
- SELECT statement inside a LOOP
- Missing `UP TO n ROWS` where applicable
- Full table scan (no WHERE clause)
- Unnecessary SORT on large internal tables
- READ TABLE without BINARY SEARCH on unsorted tables
- Nested SELECTs instead of JOINs

### 3. SELECT / Database Issues
- Missing `sy-subrc` check after SELECT
- Using obsolete `SELECT SINGLE` for non-key reads
- Row-by-row DB fetch instead of bulk `INTO TABLE`
- Missing index usage (check WHERE field order)
- Redundant or duplicate SELECT statements

### 4. Code Quality
- Hard-coded values (use constants or config tables)
- Unused variables or parameters
- Dead / unreachable code
- Missing or incorrect error handling
- Overly complex logic that can be simplified
- Obsolete ABAP statements

### 5. Security
- Missing authority checks (`AUTHORITY-CHECK`)
- Hard-coded usernames, passwords, or system values
- Unvalidated user input passed to DB or system calls

---

## Review Output Format

Structure every review as follows:

```
## Code Review: <OBJECT_NAME>

### Summary
<1-2 sentence overall assessment>

### Findings

#### Naming Conventions
| # | Line | Issue | Recommendation |
|---|------|-------|----------------|

#### Performance
| # | Line | Issue | Recommendation |

#### SELECT / Database
| # | Line | Issue | Recommendation |

#### Code Quality
| # | Line | Issue | Recommendation |

#### Security
| # | Line | Issue | Recommendation |

### Overall Rating
[ ] Clean  [ ] Minor Issues  [ ] Major Issues  [ ] Critical
```

If a category has no findings, write "No issues found."
