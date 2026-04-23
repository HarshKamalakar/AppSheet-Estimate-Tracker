# 📂 AppSheet Data Dictionary & Validation Logic

This project utilizes advanced AppSheet expressions to enforce strict data integrity, create dynamic UI behaviors, and prevent user errors at the point of entry. Below is the detailed column-level logic:

---

### 1. Estimate No.
**Purpose:** Serves as a unique identifier. The logic ensures it is exactly a 6-digit number and prevents duplicate entries.

**Valid_If:** Ensures the number is between 100000 and 999999 (6 digits) and checks the database to guarantee uniqueness.
```sql
AND(
  [_THIS] >= 100000,
  [_THIS] <= 999999,
  NOT(IN([_THIS], SELECT(Database[Estimate No.], [Sr.No.] <> [_THISROW].[Sr.No.])))
)
```

**Invalid value error:** Provides contextual feedback based on the specific error type.
```sql
IFS(
  [_THIS] < 100000, "Number is too short! Please enter exactly 6 digits.",
  [_THIS] > 999999, "Number is too long! Please enter exactly 6 digits.",
  IN([_THIS], SELECT(Database[Estimate No.], [Sr.No.] <> [_THISROW].[Sr.No.])), "Duplicate entry! This ERP number already exists in the database.",
  TRUE, "Invalid entry. Please enter a unique 6-digit ERP number."
)
```

**Editable_If:** Locks the field once the row is saved to the database.
```sql
NOT(IN([_THISROW].[Sr.No.], Database[Sr.No.]))
```

---

### 2. Name of Estimate
**Purpose:** Captures the description of the work, ensuring it contains text and not just numeric values.

**Valid_If:** Checks that the uppercase and lowercase versions of the string are not identical, effectively ensuring alphabetical characters are present.
```sql
FIND(UPPER([_THIS]), LOWER([_THIS])) = 0
```

**Invalid value error:** > "Estimate name cannot be only numbers. Please include letters."

**Editable_If:** ```sql
NOT(IN([_THISROW].[Sr.No.], Database[Sr.No.]))
```

---

### 3. Division (Cascading Dropdown)
**Purpose:** Dynamically filters the available Division options based on the previously selected Circle.

**Valid_If:**
```sql
IFS(
  [Circle]="Ujjain", LIST("UJJAIN_EAST", "UJJAIN_WEST", "UJJAIN_O&M", "TARANA", "NAGDA", "MAHIDPUR", "BARNAGAR"),
  [Circle]="Dewas", LIST("DEWAS_CITY", "DEWAS_O&M", "BAGLI", "KANNOD", "SONKATCH"),
  [Circle]="Shajapur", LIST("SHAJAPUR", "SHUJALPUR"),
  [Circle]="Agar", LIST("AGAR", "SUSNER"),
  [Circle]="Ratlam", LIST("RATLAM_CITY", "RATLAM_O&M", "JAORA", "ALOT"),
  [Circle]="Mandsaur", LIST("MANDSAUR", "MALHARGARH", "GAROTH", "SITAMAU"),
  [Circle]="Neemuch", LIST("NEEMUCH", "JAWAD", "MANASA")
)
```

---

### 4. Inward Date
**Purpose:** Standardizes date inputs by replacing various separators (`/`, `.`) with dashes (`-`) and enforces a strict date threshold.

**Valid_If:** Ensures the parsed date is not before March 20, 2026.
```sql
AND(
  ISNOTBLANK([_THIS]),
  NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 3)) >= 2026,
  DATE(CONCATENATE(
    NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 3)),
    "-",
    NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 2)),
    "-",
    NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 1))
  )) >= DATE("2026-03-20")
)
```

**Invalid value error:** > "Invalid date. Must be DD MM YYYY and from 20-03-2026 onwards."

---

### 5. Status of Estimate
**Purpose:** Prevents logical inconsistencies between the record's status and its action history.

**Valid_If:** Forces the status to change from "Pending" if action details have been logged.
```sql
IFS(
  OR(ISNOTBLANK([Action date]), ISNOTBLANK([Action letter no.])),
  [_THIS] <> "Pending",
  TRUE,
  TRUE
)
```

**Invalid value error:** > "Please change the Status. It cannot remain 'Pending' if an Action Letter or Date has been entered."

---

### 6. Reason for Pending
**Purpose:** Conditional field that only requires input when an estimate is stuck in a pending state.

**Required_If:** ```sql
[Status of estimate] = "Pending"
```

**Reset on edit?:** Automatically clears the data if the status is updated to something else.
```sql
[Status of estimate] <> "Pending"
```

---

### 7. Action Date
**Purpose:** Chronological validation to ensure actions are not backdated before the inward date.

**Valid_If:** Reuses the text-substitution logic to parse both Action Date and Inward Date, ensuring Action Date >= Inward Date.
```sql
IF( ISBLANK([_THIS]), TRUE,
  AND(
    NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 3)) >= 2026,
    DATE(CONCATENATE(
      NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 3)),
      "-",
      NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 2)),
      "-",
      NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([_THIS], "/", "-"), ".", "-"), "-"), 1))
    )) >= DATE(CONCATENATE(
      NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([Inward Date], "/", "-"), ".", "-"), "-"), 3)),
      "-",
      NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([Inward Date], "/", "-"), ".", "-"), "-"), 2)),
      "-",
      NUMBER(INDEX(SPLIT(SUBSTITUTE(SUBSTITUTE([Inward Date], "/", "-"), ".", "-"), "-"), 1))
    ))
  )
)
```

**Required_If:** Mandates a date if a resolution status is selected or a letter number is provided.
```sql
OR( 
  IN([Status of estimate], {"Approved", "Returned", "Forwarded to Higher offices"}), 
  ISNOTBLANK([Action letter no.]) 
)
```
