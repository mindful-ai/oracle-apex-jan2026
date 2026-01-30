```sql
-- Find schools with high graduation and college readiness

SELECT SCHOOL_NAME, BOROUGH, GRADUATION_RATE, COLLEGE_CAREER_RATE
FROM schools
WHERE GRADUATION_RATE >= 90
  AND COLLEGE_CAREER_RATE >= 80
ORDER BY COLLEGE_CAREER_RATE DESC;


-- Compare demand vs capacity (admissions pressure)

SELECT SCHOOL_NAME, SEATS, APPLICANTS,
       (APPLICANTS - SEATS) AS APPLICATION_GAP
FROM schools
WHERE APPLICANTS > SEATS
ORDER BY APPLICATION_GAP DESC;


-- Average attendance and safety by borough

SELECT BOROUGH,
       AVG(ATTENDANCE_RATE) AS avg_attendance,
       AVG(SAFE) AS avg_safety
FROM schools
GROUP BY BOROUGH
ORDER BY avg_attendance DESC;


-- Script

-- 1. Classify schools by graduation performance using CASE
SELECT
    SCHOOL_NAME,
    BOROUGH,
    GRADUATION_RATE,
    CASE
        WHEN GRADUATION_RATE >= 90 THEN 'High Performing'
        WHEN GRADUATION_RATE >= 75 THEN 'Average Performing'
        ELSE 'Needs Improvement'
    END AS performance_category
FROM schools;

-- 2. Calculate borough-level averages using aggregate functions
SELECT
    BOROUGH,
    ROUND(AVG(GRADUATION_RATE), 2) AS avg_graduation_rate,
    ROUND(AVG(ATTENDANCE_RATE), 2) AS avg_attendance_rate,
    ROUND(AVG(COLLEGE_CAREER_RATE), 2) AS avg_college_career_rate
FROM schools
GROUP BY BOROUGH;

-- 3. Rank schools by demand using a window function
SELECT
    SCHOOL_NAME,
    BOROUGH,
    APPLICANTS,
    SEATS,
    (APPLICANTS - SEATS) AS application_gap,
    RANK() OVER (
        PARTITION BY BOROUGH
        ORDER BY (APPLICANTS - SEATS) DESC
    ) AS demand_rank_in_borough
FROM schools
WHERE APPLICANTS > SEATS;


-- Goal: Schools offering more than 5 AP courses, with borough and total students.

-- Goal: Schools in a given neighborhood with attendance > 95%, including coordinates.

-- Goal: Compare average graduation rates for schools with vs without language classes.

SELECT
    SCHOOL_NAME,
    BOROUGH,
    TOTAL_STUDENTS,
    ADVANCED_PLACEMENT_COURSES
FROM schools
WHERE ADVANCED_PLACEMENT_COURSES > 5
ORDER BY ADVANCED_PLACEMENT_COURSES DESC;


SELECT
    SCHOOL_NAME,
    NEIGHBORHOOD,
    ATTENDANCE_RATE,
    LATITUDE,
    LONGITUDE
FROM schools
WHERE NEIGHBORHOOD = 'Harlem'
  AND ATTENDANCE_RATE > 95
ORDER BY ATTENDANCE_RATE DESC;


SELECT
    CASE
        WHEN LANGUAGE_CLASSES > 0 THEN 'Offers Language Classes'
        ELSE 'No Language Classes'
    END AS language_access,
    ROUND(AVG(GRADUATION_RATE), 2) AS avg_graduation_rate
FROM schools
GROUP BY
    CASE
        WHEN LANGUAGE_CLASSES > 0 THEN 'Offers Language Classes'
        ELSE 'No Language Classes'
    END;



```
