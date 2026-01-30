
# Oracle APEX Lab: CRUD Form with Fetch by ID (SCHOOLS Table)

## Objective
In this experiment, you will build a **single Oracle APEX Form page** that can:

- Fetch a school record by **ID**
- **Update** the record if it exists
- **Create** a new record if it does not exist
- **Delete** an existing record

This uses **Dynamic Actions** and **Automatic APEX DML** (latest APEX versions).

---

## Prerequisites
- Oracle APEX 22.x / 23.x or later
- Table `SCHOOLS` with primary key `ID`
- Basic familiarity with Page Designer

---

## Step 1: Verify the Table and Primary Key

Ensure `SCHOOLS.ID` is the primary key.

```sql
ALTER TABLE schools ADD CONSTRAINT schools_pk PRIMARY KEY (ID);
```

---

## Step 2: Create the Form Page

1. App Builder → **Create Page**
2. Select **Form**
3. Choose **Form on a Table or View**
4. Table Name → `SCHOOLS`
5. Primary Key Column → `ID`
6. Finish the wizard

---

## Step 3: Configure the Primary Key Item

Select item **P9_ID**:

- Type → Hidden
- Primary Key → Yes
- Source Type → Database Column
- Source Column → ID
- Value Protected → Yes

---

## Step 4: Add an ID Search Item

Create a new page item:

- Name → `P9_ID_SEARCH`
- Type → Number Field
- Label → Enter School ID

---

## Step 5: Create Dynamic Action (Fetch by ID)

### Client-side Condition
- Item is NOT NULL → `P9_ID_SEARCH`

### True Action → Execute Server-side Code

```plsql
BEGIN
   SELECT
      ID,
      SCHOOL_NAME,
      BOROUGH,
      NEIGHBORHOOD,
      INTEREST,
      METHOD,
      TOTAL_STUDENTS,
      GRADUATION_RATE,
      ATTENDANCE_RATE,
      COLLEGE_CAREER_RATE,
      SAFE,
      SEATS,
      APPLICANTS,
      LATITUDE,
      LONGITUDE,
      LANGUAGE_CLASSES,
      ADVANCED_PLACEMENT_COURSES,
      SCHOOL_SPORTS
   INTO
      :P9_ID,
      :P9_SCHOOL_NAME,
      :P9_BOROUGH,
      :P9_NEIGHBORHOOD,
      :P9_INTEREST,
      :P9_METHOD,
      :P9_TOTAL_STUDENTS,
      :P9_GRADUATION_RATE,
      :P9_ATTENDANCE_RATE,
      :P9_COLLEGE_CAREER_RATE,
      :P9_SAFE,
      :P9_SEATS,
      :P9_APPLICANTS,
      :P9_LATITUDE,
      :P9_LONGITUDE,
      :P9_LANGUAGE_CLASSES,
      :P9_ADVANCED_PLACEMENT_COURSES,
      :P9_SCHOOL_SPORTS
   FROM schools
   WHERE ID = :P9_ID_SEARCH;
EXCEPTION
   WHEN NO_DATA_FOUND THEN
      NULL;
END;
```

Items to Submit:
```
P9_ID_SEARCH
```

Items to Return:
```
P9_ID,
P9_SCHOOL_NAME,
P9_BOROUGH,
P9_NEIGHBORHOOD,
P9_INTEREST,
P9_METHOD,
P9_TOTAL_STUDENTS,
P9_GRADUATION_RATE,
P9_ATTENDANCE_RATE,
P9_COLLEGE_CAREER_RATE,
P9_SAFE,
P9_SEATS,
P9_APPLICANTS,
P9_LATITUDE,
P9_LONGITUDE,
P9_LANGUAGE_CLASSES,
P9_ADVANCED_PLACEMENT_COURSES,
P9_SCHOOL_SPORTS
```

---

## Step 6: Verify Automatic DML Process

Processing → Automatic Row Processing (APEX DML)

- Table → `SCHOOLS`
- Primary Key → `ID`
- Operations → Insert / Update / Delete

---

## Step 7: Button Conditions

| Button | Condition |
|------|----------|
| CREATE | P9_ID IS NULL |
| SAVE | P9_ID IS NOT NULL |
| DELETE | P9_ID IS NOT NULL |

---

## Step 8: Testing

- Existing ID → Populate → Save updates
- New ID → Insert
- Delete → Remove record

---

## Learning Outcomes

Students learn CRUD, Dynamic Actions, and APEX DML.
