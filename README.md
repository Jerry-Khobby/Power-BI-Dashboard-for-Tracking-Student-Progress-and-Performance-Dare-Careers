# Dare Careers – Student Progress & Performance Dashboard

**Power BI + AWS Cloud Integration Project**

---

## Project Overview

Dare Careers is a training organization delivering programs in:

* **Power BI**
* **AWS Cloud**

To effectively monitor learner engagement, academic performance, and program outcomes, this project delivers a comprehensive **Power BI dashboard** that tracks:

* Daily Zoom attendance
* Daily participation
* Weekly lab and quiz scores
* Certification and graduation status
* Cohort-level and learner-level performance

The dashboard enables program managers and trainers to:

* Monitor engagement trends
* Identify at-risk learners
* Measure certification and graduation rates
* Compare performance across tracks and cohorts

---

##  Project Development Approach

### Phase 1: Power BI (Standalone Model)

1. Data cleaning and transformation inside Power BI
2. Data modeling and relationship establishment
3. Creation of DAX measures
4. Dashboard design and visualization

This phase focused strictly on the **Power BI track data**.

---

### Phase 2: AWS Cloud Integration

1. Cleaned and structured AWS Cloud datasets
2. Followed the same modeling principles used in Phase 1
3. Appended AWS data to Power BI data
4. Ensured model integrity and measure consistency

Final model supports:

* Multi-track reporting (Power BI + AWS)
* Unified learner analytics
* Cohort comparison across programs

---

##  Data Structure

The dataset is organized as follows:

### Zoom Attendance

* Separate directories for Power BI and AWS
* 10 weeks of data (Week 1 – Week 10)
* Daily attendance logs
* Attendance rule:

  > A learner is marked as **Attended = 1** if time spent > 30 minutes

---

###  Labs & Quizzes

* Weekly lab grades (10 weeks)
* Weekly quiz grades (10 weeks)
* Structured by Track and Week

---

### Participation Records

* Daily participation tracking
* Engagement scores per learner

---

###  Status Records

* Certification Status
* Graduation Status
* Program Completion Status

---

##  Data Modeling

The model follows a structured star-schema approach:

* **Fact Tables**

  * Zoom Attendance
  * Participation Records
  * Labs
  * Quizzes

* **Dimension Tables**

  * Learners
  * Date Table
  * Status of Participant

* **Measures Table**

  * All DAX measures centralized for performance and organization

---

### Data Model Screenshot

*Insert screenshot here*

```markdown
![Data Model](images/data_model.png)
```

---

#  Dashboard Pages

---

# Page 1: Overall Performance Metrics

##  Objective

Provide high-level program performance insights for stakeholders.

---

## Visualizations

###  Bar Charts

* Graduation Rate %
* Certification Rate %
* Dropout Rate %
* Average Attendance %
* Average Participation %
* Average Assessment Score

###  Summary Cards

* Total Learners
* Total Certifications
* Total Graduations
* Total Dropouts

---

###  Filters (Slicers)

* Cohort
* Track (Power BI / AWS)
* Certification Status
* Learner Status

---

###  Page 1 Screenshot

*Insert screenshot here*

```markdown
![Overall Performance](images/page1_overall_metrics.png)
```

---

#  Page 2: Detailed Learner Insights

##  Objective

Provide granular learner-level analytics for trainers and program managers.

---

## Visualizations

###  Learner Table

Displays:

* Learner Name
* Track
* Attendance %
* Participation %
* Average Quiz Score
* Average Lab Score
* Certification Status
* Graduation Status

---

### Summary Cards

* Count of Labs
* Average Labs per Learner
* Total Hours in Class
* Average Attendance %
* Average Participation %
* Average Assessment Score

---

###  Filters

* Cohort
* Track
* Month
* Week
* Learner Status
* Program Status

---

###  Page 2 Screenshot

*Insert screenshot here*

```markdown
![Learner Insights](images/page2_detailed_insights.png)
```

---

#  Business Logic & Rules

##  Attendance Rule

A learner is considered **Attended** if:

```text
Zoom Duration > 30 minutes
```

This rule applies across all attendance calculations.

---

#  DAX Measures

All measures are centralized in the **Measures Table**.

Below are the key calculated measures used in the project.

---

##  Certification & Graduation

```DAX
Certification Rate % =
DIVIDE([Total Certifications], [Total Learners], 0) * 100
```

```DAX
Graduation Rate % =
DIVIDE([Total Graduations], [Total Learners], 0) * 100
```

---

## Learner Counts

```DAX
Total Learners = DISTINCTCOUNT(Learners[email])
```

```DAX
Total Certifications =
CALCULATE(
    COUNTROWS('Status of Participant'),
    'Status of Participant'[Certification Status] = "Certified"
)
```

---

##  Attendance Metrics

```DAX
Attended Sessions =
CALCULATE(
    COUNTROWS('Zoom Attendance'),
    'Zoom Attendance'[Attended] = 1
)
```

```DAX
Average Attendance % =
DIVIDE([Total Hours in Class] * 60, [Total Class Sessions]*120)
```

```DAX
Total Hours in Class =
VAR AttendedMinutes =
    CALCULATE(
        SUM('Zoom Attendance'[Duration Mins]),
        'Zoom Attendance'[Attended] = 1
    )
RETURN
DIVIDE(AttendedMinutes, 60, 0)
```

---

##  Assessment Metrics

```DAX
Average Lab Score = AVERAGE(Labs[Lab Score])
```

```DAX
Average Quiz Score = AVERAGE(Quizzes[Quiz Score])
```

```DAX
Average Assessment Score =
DIVIDE(
    [Average Lab Score] + [Average Quiz Score],
    2,
    0
)
```

---

##  Participation Metrics

```DAX
Average Participation % =
[Total Participation Records]/[Attended Sessions]
```

```DAX
Duplicate Participation Count =
SUMX(
    SUMMARIZE(
        'Participation Records-2',
        'Participation Records-2'[email],
        'Participation Records-2'[Date],
        "RowCount", COUNTROWS('Participation Records-2')
    ),
    IF([RowCount] > 1, [RowCount] - 1, 0)
)
```

---

#  Key Insights Enabled

This dashboard allows Dare Careers to:

* Identify learners below attendance threshold
* Track weekly engagement trends
* Monitor certification performance per cohort
* Compare AWS vs Power BI track performance
* Detect duplicate participation entries
* Measure program effectiveness

---

# Technologies Used

* Power BI Desktop
* DAX (Data Analysis Expressions)
* Data Modeling (Star Schema)
* AWS Cloud Dataset Integration
* Power Query (Data Cleaning & Transformation)

---

#  Future Improvements

* Automate AWS ingestion via API
* Add predictive dropout risk scoring
* Deploy dashboard to Power BI Service
* Implement Row-Level Security (RLS)
* Add trend forecasting visuals


