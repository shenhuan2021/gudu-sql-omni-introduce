# 🚀 Solving DataHub’s Limitation in Parsing Complex SQL Lineage (gsp-datahub-sidecar Validation)


## 🧩 Background

When using DataHub for data lineage analysis, a common issue is:

👉 **Failure to correctly parse complex SQL (especially BigQuery procedural SQL)**

As mentioned in this issue:

- DataHub issue #11654

Typical symptoms include:

- Multi-statement SQL (e.g., `BEGIN...END`) cannot be parsed
- Missing or incomplete lineage
- Column-level lineage cannot be extracted

---

## 💡 Solution

Use the sidecar tool provided by Gudu:

👉 **gsp-datahub-sidecar**

GitHub repository:  
https://github.com/gudusoftware/gsp-datahub-sidecar

### Capabilities

- Supports BigQuery procedural SQL
- Supports column-level lineage
- No modification required to DataHub itself

---

## 🎯 Validation Objective

This article verifies whether the following pipeline works:

```
BigQuery procedural SQL
↓
gsp-datahub-sidecar
↓
DataHub GMS
↓
DataHub UI lineage visualization
```

---

## 🧪 Test Steps (Reproducible)

> Prerequisite: You already have a running DataHub instance  
> (GMS default: http://localhost:8080)

---

### Step 0: Install the Sidecar

```bash
pip install git+https://github.com/gudusoftware/gsp-datahub-sidecar.git
```

👉 If you are using macOS + Homebrew Python (PEP 668 issue), recommended:

```bash
brew install pipx
pipx install git+https://github.com/gudusoftware/gsp-datahub-sidecar.git
```

Verify installation:

```bash
gsp-datahub-sidecar --version
```

---

### Step 1: Get Test SQL

```bash
git clone https://github.com/gudusoftware/gsp-datahub-sidecar.git
cd gsp-datahub-sidecar
```

Use the built-in example:

```
examples/bigquery_procedural.sql
```

---

### Step 2: Validate SQL Parsing (Dry Run Mode)

```bash
gsp-datahub-sidecar \
--mode authenticated \
--user-id YOUR_USER_ID \
--secret-key YOUR_SECRET_KEY \
--sql-file examples/bigquery_procedural.sql \
--dry-run
```

> **Dry-run mode is for debugging only. It parses lineage for review but does NOT send data to the DataHub server.**

✅ Example output:

```
Detected procedural SQL — sending as single statement

Lineage: PROJECT.DATASET.VIEW_NAME --> TEMP_TABLE (12 columns)
Lineage: TEMP_TABLE_DELTA --> FINAL_OUTPUT (5 columns)

[DRY RUN] Would emit 10 MCPs
```
<img width="2506" height="458" alt="image" src="https://github.com/user-attachments/assets/098c4830-c5ed-4ac2-b1af-0575d51babca" />

---

### Step 3: Ingest into DataHub

#### Test with BigQuery SQL

```bash
gsp-datahub-sidecar \
--mode authenticated \
--user-id YOUR_USER_ID \
--secret-key YOUR_SECRET_KEY \
--sql-file examples/bigquery_procedural.sql
```

✅ Example output:

```
Detected procedural SQL — sending as single statement

Lineage: PROJECT.DATASET.VIEW_NAME --> TEMP_TABLE (12 columns)
Lineage: TEMP_TABLE_DELTA --> FINAL_OUTPUT (5 columns)

Emitted 10 MCPs
```

---

### 🔍 Verify Table-Level Lineage

1. In DataHub UI, search for `temp_table`, you should see:

```
project.dataset.view_name → temp_table
```
<img width="2434" height="1172" alt="image" src="https://github.com/user-attachments/assets/bfd5007c-9041-43f1-a152-210a711c2b54" />


2. Search for `final_output`, you should see:

```
temp_table_delta → final_output
```
<img width="1462" height="1008" alt="image" src="https://github.com/user-attachments/assets/76ac4aaa-14a5-4df3-b7fb-9c795b3b2964" />

---

### 🔬 Verify Column-Level Lineage

You should observe column-level relationships such as:

- `email`
- `idfield`

mapped between `temp_table_delta` and `final_output`.
<img width="1576" height="750" alt="image" src="https://github.com/user-attachments/assets/8468bbdc-5d5d-4241-9797-ddcb635224ea" />

---

### 🧪 Test with Oracle SQL

```bash
gsp-datahub-sidecar \
--mode authenticated \
--user-id YOUR_USER_ID \
--secret-key YOUR_SECRET_KEY \
--sql-file examples/oracle_create_view.sql \
--db-vendor dbvoracle \
--datahub-server http://localhost:8080
```

In the UI, search for `vsal`, and you should see correct table and column lineage relationships.
<img width="1538" height="1268" alt="image" src="https://github.com/user-attachments/assets/e7cd3fca-5fb2-4610-8a4c-3e6e39049b44" />

---

### 🏠 Test Using Self-Hosted Mode

```bash
gsp-datahub-sidecar --mode self_hosted \
--sqlflow-url http://localhost:8165/api/gspLive_backend/sqlflow/generation/sqlflow/exportFullLineageAsJson \
--user-id YOUR_USER_ID \
--secret-key YOUR_SECRET_KEY \
--sql-file examples/oracle_create_view.sql \
--db-vendor dbvoracle
```

> ⚠️ Note: You must install and run SQLFlow locally in advance.

---

## 📊 Validation Results

| Capability                  | Result |
|---------------------------|--------|
| Procedural SQL Parsing     | ✅     |
| Table-Level Lineage        | ✅     |
| Column-Level Lineage       | ✅     |
| DataHub Integration        | ✅     |
| UI Visualization           | ✅     |

---

## 🧨 Key Conclusion

👉 **gsp-datahub-sidecar effectively solves DataHub's limitations in parsing complex SQL**

Especially suitable for:

- 20+ mainstream databases
- Multi-statement SQL
- Column-level lineage analysis

---

If your problem is:

> DataHub cannot correctly parse complex SQL lineage

Then:

👉 **This tool has been validated as a reliable solution**

---

## 🔗 Related Project

https://github.com/gudusoftware/gsp-datahub-sidecar
