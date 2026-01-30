
# Oracle APEX Student Guide
## Cards Region & Faceted Search

This guide walks you through:
1. Creating a **Cards Region** on the Home page
2. Creating a **new page with a Faceted Search region**

---

## Part 1: Creating a Cards Region (Students by Borough)

### Goal
Display one card per **Borough**, showing the **total number of students**.

### Step 1: Open Page Designer
1. Open your **APEX Application**
2. Go to **Pages**
3. Open the **Home Page**
4. Click **Edit Page**

### Step 2: Create the Cards Region
1. Right-click in the **Layout pane**
2. Select **Create Region**
3. Set:
   - Region Type: `Cards`
   - Title: `Students by Borough`

### Step 3: Add SQL Query
Set **Source Type** to `SQL Query` and paste:

```sql
SELECT
    BOROUGH,
    SUM(TOTAL_STUDENTS) AS TOTAL_STUDENTS
FROM schools
GROUP BY BOROUGH
ORDER BY BOROUGH;
```

### Step 4: Configure Card Attributes
- **Title**: `&BOROUGH.`
- **Body**: `Total Students: &TOTAL_STUDENTS.`
- **Icon (optional)**: `fa-users`

### Step 5: Save and Run
Click **Save** → **Run Page**

---

## Part 2: Creating a Faceted Search Page

### Goal
Allow users to filter and search schools using facets such as Borough, Neighborhood, and Performance.

### Step 1: Create a New Page
1. Click **Create Page**
2. Choose **Faceted Search**
3. Click **Next**

### Step 2: Select Data Source
- Source Type: `Table / View`
- Table Name: `SCHOOLS`

### Step 3: Choose Display Columns
Recommended:
- SCHOOL_NAME
- BOROUGH
- NEIGHBORHOOD
- TOTAL_STUDENTS
- GRADUATION_RATE
- ATTENDANCE_RATE

### Step 4: Configure Facets
Suggested facets:
- BOROUGH → Checkbox Group
- NEIGHBORHOOD → Checkbox Group
- INTEREST → Checkbox Group
- TOTAL_STUDENTS → Range Slider
- GRADUATION_RATE → Range Slider

### Step 5: Finalize Page
- Page Name: `School Search`
- Add page to Navigation Menu
- Click **Create Page**

### Step 6: Improve Results Region (Optional)
Change results region type to **Cards** and configure:
- Title: `&SCHOOL_NAME.`
- Subtitle: `&BOROUGH. — &NEIGHBORHOOD.`
- Body:
  - Students: `&TOTAL_STUDENTS.`
  - Graduation Rate: `&GRADUATION_RATE.%`

---

## Tips
- Add indexes on BOROUGH and NEIGHBORHOOD for performance
- Enable facet counts and charts for better UX
- Use cards for visual appeal, reports for data-heavy use

---

Happy building with Oracle APEX!
