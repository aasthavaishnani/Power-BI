# 🏫 Student Analytics & Behavioral Insights Dashboard

Developed as a Power BI Academic Project | Gujarat, India.
An Advanced Power BI Business Intelligence solution designed to analyze student academic performance, monitor daily attendance trends, and track behavioral patterns for educational institutions.

---

## 📌 Project Overview

This project provides an interactive, data-driven dashboard built using **Power BI** to help educators, school administrators, and coordinators track student progress, detect early signs of chronic absenteeism, and monitor student behavior.

By integrating academic scores, daily attendance records, and behavioral logs, this dashboard offers a 360-degree view of institutional performance, enabling timely interventions.

---

## 🗂️ Data Architecture & Schema

The project utilizes four primary datasets with real-world noise (handled during data cleaning and preprocessing):

### 1. Students Master (`Students.xlsx`)

* `StudentID` (Primary Key)
* `Name`
* `Gender`
* `Class`
* `Section`

### 2. Academic Performance (`Scores.xlsx`)

* `StudentID` (Foreign Key)
* `Subject` (Maths, Science, English, etc.)
* `Term` (Term 1, Term 2)
* `Score`

### 3. Attendance Logs (`Attendance.xlsx`)

* `StudentID` (Foreign Key)
* `Date`
* `Status` (Present, Absent)
* `Reason`

### 4. Behavioral Records (`Behavior.xlsx`)

* `StudentID` (Foreign Key)
* `Date`
* `BehaviorType` (Participative, Helpful, Late, Disruptive, Absent without notice)
* `Notes` (Teacher Remarks)

---

## 📊 Dashboard Structure (Pages)

### 📈 Page 1: Academic Performance Overview

Focuses on school-wide and class-level academic achievements.

#### Top KPIs

* Average Score
* Total Students
* Passed Count
* Failed Count

#### Slicers

* Dynamic filtering by Class
* Dynamic filtering by Section
* Dynamic filtering by Subject

#### Visuals

* Subject-wise Performance Analysis
* Grade Distribution
* Class Performance vs Targets

---

### 📅 Page 2: Behavioral & Attendance Insights

Tracks daily operational metrics and flags students requiring immediate attention.

#### Top KPIs

* Overall Average Attendance %
* Total Incident Counts
* Chronic Absentees

#### Behavior Types Distribution (Donut Chart)

Visualizes the percentage breakdown of positive behaviors (Helpful, Participative) versus negative behaviors (Late, Disruptive).

#### Attendance Trend by Day of Week (Bar Chart)

Identifies weekly attendance patterns and highlights high-risk dropout days.

#### High-Risk Alerts / Attention Required (Table Visual)

Dynamically filters and displays students with negative behavioral incidents greater than three.

---

### 👤 Page 3: Individual Student Profile (Drillthrough Page)

A deep-dive view into a single student's record, accessible through drillthrough functionality from any student list.

#### Student Metadata Card

Displays:

* Student Name
* Student ID
* Gender
* Class & Section

#### Personal KPIs

* Student Average Score %
* Student Attendance %
* Performance Category (High / Medium / Low)

#### Subject-wise Scores vs Class Average

Clustered Column Chart comparing:

* Individual Student Scores
* Class Average Scores

#### Advanced Visual Tooltip (Subject Trend)

Hovering over any subject column displays a custom micro line chart showing the student's academic growth from Term 1 to Term 2.

#### Behavior Logs & Teacher Notes

Detailed table displaying:

* Date
* Behavior Type
* Teacher Remarks

---

## 🛠️ Core DAX Calculations & Metrics

### 1. Overall Attendance %

```dax
Attendance % =
DIVIDE(
    CALCULATE(
        COUNT(Attendance[Status]),
        Attendance[Status] = "Present"
    ),
    COUNT(Attendance[Status]),
    0
)
```

### 2. Chronic Absentees Count

*Adjusted for dataset threshold optimization where the minimum attendance baseline is 85%.*

```dax
Chronic_Absentees_Count =
COUNTROWS(
    FILTER(
        SUMMARIZE(
            Attendance,
            Attendance[StudentID],
            "Stud_Attend_Pct",
            DIVIDE(
                SUM(Attendance[IsPresent]),
                COUNT(Attendance[Status]),
                0
            )
        ),
        [Stud_Attend_Pct] < 0.85
    )
)
```

### 3. Subject Score %

```dax
% Score =
DIVIDE(
    AVERAGE(Scores[Score]),
    100,
    0
)
```

### 4. Class Average Score

```dax
Class Avg Score =
CALCULATE(
    AVERAGE(Scores[Score]),
    ALL(Students[Name])
)
```

### 5. Dynamic Performance Category

```dax
Performance Category =
IF(
    [% Score] >= 0.80,
    "HIGH",
    IF(
        [% Score] >= 0.50,
        "MEDIUM",
        "LOW"
    )
)
```

---

## 🎨 UI/UX Design System

### Theme & Palette

* Dark Green Primary: `#5B8C1D`
* Mint Green Accent Colors
* Clean Gray & White Backgrounds

### Layout Structure

* Left-side Persistent Navigation Panel
* Home Navigation
* Dashboard Navigation
* User Profile Navigation
* Top Global Slicer Panel

### Visual Styling

* Container-Based Layout
* Consistent Dashboard Design
* `12px` Rounded Corners
* Professional Subtle Borders
* Enhanced Dashboard Readability

---

## 🚀 How to Run the Project

1. Open **Power BI Desktop**.

2. Open the file **Student Dashboard.pbix**.

3. Ensure the following files are available in the same folder:

   * `Students.xlsx`
   * `Scores.xlsx`
   * `Attendance.xlsx`
   * `Behavior.xlsx`

4. If necessary, update file paths through:

   **Transform Data → Data Source Settings**

5. Click **Refresh** to load the latest data.

6. Use **Ctrl + Click** on navigation buttons to test interactions.

7. Right-click any student record and select **Drillthrough** to view the detailed student profile page.

---

## 🏆 Technologies Used

* Power BI Desktop
* Power Query
* DAX (Data Analysis Expressions)
* Data Modeling
* Interactive Visualizations
* Drillthrough Reports
* Conditional Formatting
* Custom Tooltips
* Dashboard Navigation

---

## 🎯 Key Outcomes

* Improved visibility into student academic performance.
* Early identification of chronic absenteeism.
* Enhanced monitoring of behavioral incidents.
* Data-driven intervention planning.
* Centralized student performance tracking.
* Interactive reporting for educators and administrators.

---

## 👨‍💻 Academic Learning Outcomes

This project demonstrates practical skills in:

* Business Intelligence
* Data Analytics
* Educational Data Analysis
* Dashboard Development
* Data Modeling
* DAX Calculations
* Reporting & Visualization
* Decision Support Systems

---

**Developed as part of an Academic Power BI Project focused on Educational Analytics, Student Performance Monitoring, and Behavioral Insight Reporting.**
