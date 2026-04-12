# 🗄️ Parse Oracle SQL in VS Code with Gudu SQL Omni

**TL;DR:** [Gudu SQL Omni](https://marketplace.visualstudio.com/items?itemName=gudusoftware.gudu-sql-omni) parses SQL with a real syntax tree—not regex heuristics—then surfaces **column-level lineage**, **table/schema lineage**, **impact analysis**, **schema extraction**, **ER diagrams from DDL**, and **SQL validation**. Everything runs **locally** 🔒, so your Oracle scripts never leave your machine.

**SEO keywords:** Oracle SQL parser VS Code, Gudu SQL Omni Oracle, offline Oracle SQL lineage, VS Code SQL analysis

---

## 🎯 Why Oracle teams care: parsing vs. pretty printing

Parsing means building an **abstract syntax tree (AST)** and doing **semantic analysis** (for example, which physical columns flow into which outputs). That unlocks:

- 🔗 Lineage through CTEs, nested subqueries, and window functions
- 🛡️ Safer refactors when renaming columns or tables
- ⏱️ Faster onboarding when legacy Oracle SQL spans hundreds of lines

That is a different league from basic formatters or grep-only dependency hunting.

---

## 📦 Install and first run

1. Open VS Code (or Cursor).
2. Press `Ctrl+Shift+X` (Windows/Linux) or `Cmd+Shift+X` (macOS).
3. Search for **Gudu SQL Omni** and install (`gudusoftware.gudu-sql-omni`).

Open any `.sql` file; the extension activates. Run commands from the Command Palette or the editor context menu.

---

## ⚙️ Pin Oracle as the default dialect (recommended)

Omni can auto-detect dialects; Oracle projects are clearer with an explicit default. In **Settings (JSON)**:

```json
{
  "guduSQLOmni.general.defaultDbVendor": "oracle"
}
```

Optional lineage toggles (from vendor documentation):

```json
{
  "guduSQLOmni.lineage.showTemporaryResult": true,
  "guduSQLOmni.lineage.showRestrictGroupsRelation": true
}
```

---

## ⌨️ Shortcuts and commands (typical defaults)

On **Windows/Linux** (macOS may differ—verify in Command Palette):

| Action | Shortcut | What you get |
|--------|----------|----------------|
| **Data Lineage** | `Ctrl+Alt+L` | Interactive graph: upstream/downstream data flow |
| **Extract Schema** | `Ctrl+Shift+E` | Tables and columns discovered in SQL |
| **ER Diagram** | `Ctrl+Alt+E` | ER-style view from DDL-oriented statements |
| **Validate SQL** | `Ctrl+Shift+V` | Syntax check before CI or production |

**💡 Tip:** Right-click in the editor for lineage actions; right-click a folder in Explorer for **Analyze SQL Lineage** (workspace scan).

---

## ✨ Feature tour: what each capability does

### 🔬 Column-level lineage

Trace **which source columns** feed each output column through joins, unions, subqueries, CTEs, and window expressions.

**Best for:** “Where did this metric come from?”, KPI documentation, heavy Oracle reporting SQL.

### 🧩 Table-level lineage

A higher-level view of **how tables depend on each other** in the statement or project.

**Best for:** quick architectural reads before column-level dives.

### 🌐 Schema-level lineage

Dependencies across **schemas or databases** as expressed in SQL text.

**Best for:** cross-team contracts, migrations, and blast-radius thinking.

### 💥 Impact analysis

See **what breaks or changes** if you rename or remove a table or column—before you ship.

**Best for:** safe refactors on shared Oracle views and ETL SQL.

### 📊 ER diagrams from DDL

**Entity–relationship diagrams** from `CREATE TABLE`: tables, columns, types, relationships.

**Best for:** legacy DDL you want to *see* without another modeling tool.

### 📂 Workspace scanning and dbt support

Point Omni at a **folder** to analyze many `.sql` files and build a **unified lineage graph**. **dbt** models, sources, and refs can resolve during workspace scans.

**Best for:** monorepos, analytics repos, Oracle + warehouse hybrids.

### 🧭 Lineage index explorer

Browse **tables and columns** discovered across the workspace; **jump to file and line**.

**Best for:** navigation at scale—a lightweight in-editor catalog.

### ✅ Validate SQL

**Syntax validation** in the editor loop, alongside database and CI checks.

**Best for:** catching obvious Oracle SQL issues before expensive runs.

### 📤 Export

Export as **JSON** or **PNG** (per vendor docs)—docs, tickets, reviews, audit packs.

---

## 🔒 Privacy, performance, and enterprise fit

- **Offline-first:** Parsing and lineage run locally; SQL is not uploaded for analysis (per vendor documentation).
- **Bundled runtime:** A minimal **Java runtime** is included—usually no separate JDK install.
- **Large files:** Vendor FAQ cites optimization for very large SQL (on the order of up to about **10MB**).

---

## ❓ FAQ (answer-engine friendly)

**Does Gudu SQL Omni upload my Oracle SQL?**  
**No.** Core work runs **on your machine** inside VS Code.

**How do I parse Oracle SQL specifically?**  
Install the extension, set `"guduSQLOmni.general.defaultDbVendor": "oracle"`, open your `.sql` file, then run **Data Lineage** (`Ctrl+Alt+L` on Windows/Linux) or use the Command Palette / context menu.

**Is lineage regex-based?**  
Vendor messaging emphasizes **AST / syntax-tree parsing**, not regex, for CTEs, correlated subqueries, window functions, and complex joins.

**Does it support dbt?**  
**Yes**—workspace scanning can resolve dbt refs and sources when pointed at a dbt project.

**What about free tier limits?**  
Vendor docs describe a **free tier with caps** on tables/relationships—confirm current numbers on the Marketplace page and in-extension licensing.

---

## 🔗 Official links

- **Marketplace:** https://marketplace.visualstudio.com/items?itemName=gudusoftware.gudu-sql-omni  
- **Product site:** https://gudu-sql-omni.gudusoft.com/

---

## 📌 Disclosure

Pricing, tier limits, and shortcuts can change. Double-check the **Marketplace listing** and the Command Palette entries for **Gudu SQL Omni**.
