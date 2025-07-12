##**APEX Project - Application Inventory**

This project defines a data model and business logic to store and manage application metadata in an Oracle database, including an Oracle APEX script for front-end integration.

---------------------------------------------

**##Project Structure**

├── APP_INFO_DDL.sql -- SQL to create tables, trigger, and sequence
└── Apex_project/
└── f237343.sql -- APEX application export script


---

## APP_INFO_DDL.sql Overview

### 1. Tables

#### APP_INFO

Stores detailed metadata for applications, including functional ownership, development links, technologies used, and responsible developers.

| Column                                       | Type                 | Description                 |
|----------------------------------------------|----------------------|-----------------------------|
| ID                                           | NUMBER               | Primary key, auto-generated |
| APP_NO                                       | VARCHAR2(400)        | Application Number          |
| APPLICATION_TYPE                             | VARCHAR2(400)        | Type (e.g., Internal, External) |
| FUNCTIONAL_OWNER                             | VARCHAR2(4000)       | Business or IT owner        |
| TECHNOLOGY                                   | VARCHAR2(1000)       | Stack used (e.g., Java, .NET, APEX) |
| DEV_LINK, SIT_LINK, PROD_LINK                | VARCHAR2(4000)       | Environment URLs |
| DEVELOPER_SME_*                              | VARCHAR2(400)        | Primary/Secondary developers |
| APPLICATION_CREATED_ON                       | DATE                 | App inception date |
| CREATED_BY, UPDATED_BY                       | VARCHAR2(400)        | Audit columns |

#### SERVER_LIST

Stores server metadata for application hosting environments.

| Column        | Type          | Description |
|---------------|---------------|-------------|
| ID            | NUMBER        | Primary Key |
| SERVER_NAME   | VARCHAR2(200) | Hostname |
| SERVER_IP     | VARCHAR2(200) | IP Address |
| SERVER_TYPE   | VARCHAR2(200) | App/DB/Web |
| DB_SERVER     | VARCHAR2(200) | Associated DB server |

---

### 2. Trigger: `BIU_APP_INFO`

A **BEFORE INSERT OR UPDATE** trigger on the `APP_INFO` table.

- **Auto-assigns `ID`** using a sequence (`APP_INFO_SEQ`)
- Fills `CREATED_ON`, `CREATED_BY`, `UPDATED_ON`, `UPDATED_BY` using `v('APP_USER')`
- Ensures audit info is always captured

### 3. Sequence: `APP_INFO_SEQ`

Used to generate unique `ID` values for `APP_INFO` rows.

- Starts with: 82
- Increment: 1
- Max: 99999999999999

---

## APEX Application

`Apex_project/f237343.sql` is an **Oracle APEX Export File**.

- Contains the application logic/UI built on top of the `APP_INFO` schema
- To import:
  1. Open Oracle APEX
  2. Navigate to **App Builder → Import**
  3. Upload and install `f237343.sql`

---

## How to Deploy

### 1. Run SQL Setup

Execute `APP_INFO_DDL.sql` using tools like:

- Oracle SQL Developer
- SQL*Plus
- APEX SQL Workshop

```sql
@APP_INFO_DDL.sql



2. Import APEX Application
Use Oracle APEX → App Builder → Import → Run f237343.sql
