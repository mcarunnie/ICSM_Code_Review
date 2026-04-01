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

Before performing any code review, check if the `abap-adt` MCP server is connected by attempting a test call. If the connection fails or `.claude.json` has placeholder values, ask the user for the following details and update `.claude.json` accordingly:

1. **SAP URL** — e.g. `http://your-sap-host:50000`
2. **SAP Client** — e.g. `100`
3. **SAP Username**
4. **SAP Password**

Once collected, update the `env` section in `.claude.json` with the provided values. Inform the user that `.claude.json` is in `.gitignore` and will not be committed to GitHub.

---

## Code Review Checks

When performing a code review, always check ALL of the following categories and report findings grouped by category.

### 1. Naming Conventions

#### Local Variables
| Prefix | Usage |
|--------|-------|
| `lv_` | Local variable (scalar) |
| `lt_` | Local internal table |
| `ls_` | Local structure |
| `lo_` | Local object reference |
| `<ls_>` | Local field symbol (structure) |
| `<lv_>` | Local field symbol (scalar) |

#### Global Variables
| Prefix | Usage |
|--------|-------|
| `mv_` | Global variable (scalar) |
| `mt_` | Global (member) internal table |
| `ms_` | Global structure |
| `mo_` | Global object reference |
| `<fs_>` | Global field symbol |

#### Local Types
| Prefix | Usage |
|--------|-------|
| `lty_` | Local type definition |

#### Global Types
| Prefix | Usage |
|--------|-------|
| `gt_` | Global type definition (table type) |

#### Method Parameters
| Prefix | Usage |
|--------|-------|
| `iv_` | Importing scalar |
| `it_` | Importing table |
| `is_` | Importing structure |
| `ev_` | Exporting scalar |
| `et_` | Exporting table |
| `rv_` | Returning value |
| `ct_` | Changing table |
| `cs_` | Changing structure |

#### Object Naming
- **Namespace: `/CTCO/` is mandatory for all objects. `Z` or `Y` namespace is NOT allowed.**
- Classes: `/CTCO/CL_`
- Interfaces: `/CTCO/IF_`
- Methods: descriptive `verb_noun` style (e.g., `get_material`, `calculate_price`)
- Tables: `/CTCO/`
- Function groups: `/CTCO/`
- Programs/Reports: `/CTCO/R_`

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
