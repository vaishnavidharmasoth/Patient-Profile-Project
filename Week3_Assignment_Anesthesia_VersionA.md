# Week 3 Assignment: Anesthesia Dataset Analysis

## I 320D: Data Science for Biomedical Informatics | Spring 2026

### 📋 Assignment Version A

---

## 🎯 This Week's Mantra

> **"Every Column Tells a Story"**

In this assignment, you'll apply the 10-Point Data Inspection to a real-world healthcare dataset focused on anesthesia outcomes. By the end, you'll understand not just *what* the data contains, but *why* each variable matters for clinical decision-making and patient safety.

---

## Learning Objectives

By completing this assignment, you will be able to:

1. ✅ Apply the systematic 10-Point Inspection to a healthcare dataset
2. ✅ Identify and classify feature types (continuous, discrete, categorical, ordinal)
3. ✅ Detect and document data quality issues (missing values, unexpected values)
4. ✅ Research and document clinical meaning for healthcare variables
5. ✅ Create meaningful data groupings based on clinical standards
6. ✅ Formulate answerable research questions about anesthesia outcomes

---

## About the Dataset

**Dataset:** Anesthesia Patient Outcomes  
**File:** `Anesthesia_Dataset.csv`  
**Target Variable:** `Outcome` (surgical outcome: 0 = unsuccessful, 1 = successful)

### Clinical Context

Anesthesia management is a critical component of surgical care, directly impacting patient safety and surgical outcomes. This dataset contains patient information that anesthesiologists and surgical teams use to assess risk factors, plan anesthesia protocols, and predict potential complications. Understanding these variables is crucial for:

- Pre-operative risk assessment and patient optimization
- Anesthesia type selection based on patient characteristics
- Post-operative complication prediction and prevention
- Quality improvement in surgical care
- Resource allocation and surgical planning

---

## Getting Started

First, load the dataset and import your libraries:

```python
# Import libraries
import pandas as pd
import numpy as np

pd.set_option('display.max_columns',None)
pd.set_option('display.width',None)

print("Libraries imported successfully!")
print(f"pandas version: {pd.__version__}")
print(f"numpy version: {np.__version__}")

# Load the dataset
df = pd.read_csv('Anesthesia_Dataset.csv')
print ("Dataset loaded successfully")

# Display first few rows to confirm it loaded
df['SurgeryType'] = df['SurgeryType'].str.title()
df.head()
```

---

## Part 1: The 10-Point Data Inspection (40 points)

Complete each inspection step and document your findings.

### Step 1: Shape (4 points)

**Your Code:**
```
df.shape
```

**Your Findings:**
- How many rows (observations)?
  300
- How many columns (features)?
  12
- What does each row represent in clinical terms?
  Each row represents a patient profile for individuals going under anesthesia, evaluating before, during, and after surgery.

---

### Step 2: Column Names (4 points)

**Your Code:**
```
df.columns
```

**Your Findings:**
- List all column names:
  PatientID, Age, Gender, BMI, SurgeryType, SurgeryDuration, AnesthesiaType, PreoperativeNotes, PostoperativeNotes, PainLevel, Complications, and Outcome
- Which columns might need further research to understand?
  Some columns need more research because their meaning is not always clear to the general public. SurgeryType needs research on different kinds of procedures, like Neurological or Cardiovascular surgeries. AnesthesiaType also needs clarification since different types, like general or local anesthesia, can affect patients in different ways. PreoperativeNotes and PostoperativeNotes are written in clinical terms, so they need research to understand the meanings properly. 
---

### Step 3: Data Types (4 points)

**Your Code:**
```
df.dtypes
```

**Your Findings:**
- Which columns are numeric?
  PatientID, Age, BMI, PainLevel, and Outcome
- Which columns are categorical (object/string)?
  Gender, SurgeryType, SurgeryDuration, AnesthesiaType, PreoperativeNotes, PostoperativeNotes, Complications
- Are there any data types that seem incorrect? (Hint: Look at SurgeryDuration)
  SurgeryDuration should not be listed as object since time is measured in integer values (seconds/minutes/hours). It is written as object since the integers are written with 'min' symbolizing minutes afterwards.
---

### Step 4: First Look (4 points)

**Your Code:**
```
df.head()
```

**Your Findings:**
- What do the actual values look like?

- Do you notice anything unusual or unexpected?
  SurgeryDuration is stored as text with 'min' next to it, which means it can't be analyzed numerically without cleaning. PreoperativeNotes and PostoperativeNotes are also unstructured text.
- Are there any values that might need cleaning before analysis?
  SurgeryDuration needs to be converted to a numeric column. Categorical columns might need preprocessing to make it easier for analysis.
---

### Step 5: Last Look (4 points)

**Your Code:**
```
df.tail()
```

**Your Findings:**
- Does the data end cleanly?
  Yes
- Are the last rows consistent with the first rows?
  Yes
---

### Step 6: Memory Usage (4 points)

**Your Code:**
```
df.memory_usage()
```

**Your Findings:**
- How much memory does the dataset use?
  Around 29 KB
- Is this a "small" or "large" dataset by data science standards?
  This is small as KB are far less than the standard MB and GB used in datasets
---

### Step 7: Missing Values (4 points)

**Your Code:**
```
df.isnull().sum()
```

**Your Findings:**
- Which columns have missing values (according to `.isnull()`)?
  Complications is the only column with missing values.
- What percentage of each column is missing?
  Around 24% of Complications is missing
- Are there any columns where missing data might be clinically significant?
  Missing data in Complications is significant because it changes how we evaluate complication rates in patient safety.
---

### Step 8: Duplicates (4 points)

**Your Code:**
```
df.duplicated()
```

**Your Findings:**
- Are there any duplicate rows?
  No
- Are all patient IDs unique?
  Yes
- Why is it important to check for duplicates in medical data?
  Duplicate records can change analysis outcome results. In medical data, this can lead to wrong conclusions and harm patients with wrong implementation in real life practices. For example, if a patient who had respiratory distress with general anesthesia is a duplicated data point, it could make general anesthesia look more dangerous than it actually is.
---

### Step 9: Basic Statistics (4 points)

**Your Code:**
```
df.describe()
```

**Your Findings:**
- What is the age range in the dataset?
  33 to 72
- What is the range of BMI values?
  23 to 32
- What is the range of PainLevel values?
  2 to 7
- Do any min/max values seem impossible or clinically unlikely?
  All of these fall in realistic ranges clinically.
---

### Step 10: Unique Counts (4 points)

**Your Code:**
```
df.nunique()
```

**Your Findings:**
- Which columns have very few unique values (likely categorical)?
  Gender, SurgeryType, AnesthesiaType, and Outcome
- Which columns have many unique values (likely continuous or IDs)?
  PatientID, Age, BMI, SurgeryDuration, PreoperativeNotes, and PostoperativeNotes
- Does the number of unique PatientIDs match the number of rows?
  Yes

---

## Part 2: Data Dictionary (20 points)

Complete the following data dictionary. For each column, you must:
1. **Research** the clinical meaning
2. **Identify** the feature type (Continuous, Discrete, Categorical-Nominal, Categorical-Ordinal, Binary, Identifier)
3. **Document** the valid values/range you observe
4. **Note** any issues or questions

| Column | Description | Feature Type | Valid Values/Range | Notes/Issues |
|--------|-------------|--------------|-------------------|--------------|
| `PatientID` | Unique number representing each patient. | Identifier | Positive Numbers greater than 1 | This column alone shouldn't be used for statistical analysis, just for identification |
| `Age` | The patient's age during surgery, in years. | Discrete | Between 0 (newborns) to 100 (elderly) | Watch out for any negative values or anything way higher than 100. |
| `Gender` | The biological sex of the patient. | Categorical-Nominal | Male/Female | Making sure the gender labels are consistent (ex: M/F or Male/Female) |
| `BMI` | The Body Mass Index, which measures someone's body fat based on their height and weight | Continous | 15-40 | Beware of extreme outliers out of the normal BMI range, or text values instead of integers. |
| `SurgeryType` | The category of surgery the patient went through. | Categorical-Nominal | Different names like Cardiovascular, Neurological, Cosmetic, Orthopedic. | Make sure names are consistent. |
| `SurgeryDuration` | The length of the surgery in minutes. | Continuous | 10-500 minutes| Watch out for any text in this column. |
| `AnesthesiaType` | The category of anesthesia administered during surgery.| Categorical-Nominal | Terms like Local or General | Make sure labels stay consistent to this category |
| `PreoperativeNotes` | Clinical notes from before the surgery. | Categorical-Nominal | Hypertension, diabetes, stable, no allergies | Needs cleaning before analysis. |
| `PostoperativeNotes` | Clinical notes from after the surgery. | Categorical-Nominal | Minimal pain no complications, pain slow recovery | Needs cleaning before analysis. |
| `PainLevel` | The intensity of pain the patient had after the surgery. | Categorical-Ordinal | 1-10 | Watch out for any number out of this range. |
| `Complications` | Any issues in the surgery. | Categorical-Nominal | none, nausea, delayed recovery, respiratory distress | Make sure the categories only have the appropriate values. |
| `Outcome` | The results of the surgery after it ends. | Binary | 0/1| Make sure they are just 0 and 1 for the values |

### Clinical Research Questions for Version A

Answer these questions based on your research (you may need to use external sources):

**1. What are the main types of anesthesia (General, Local, Regional, etc.)? What are the key differences in terms of patient risk and recovery?**

Your answer:
General anesthesia is when the patient is completely unconscious while Regional numbs a large part of the body and the patient can stay awake or just lightly sedated. Local anesthesia only numbs a small area and the patient can stay fully awake. General has the highest risk because it can directly affect the respiratory and cardiovascular system when the patient is fully unconscious while regional and local have lower risks because the patient does not have to be fully unconscious. The recovery time is directly proportional to the intensity of the anesthesia, so someone with local anesthesia would recover faster than someone with general anesthesia. 

---

**2. What is the clinical significance of BMI in anesthesia risk assessment? At what BMI levels do anesthesiologists typically consider a patient "high-risk"?**

Your answer:
BMI helps in anesthesia risk assessments because it can help doctors check if there are any airway difficulties with excess body fat, risk of sleep apnea in overweight patients, or any cardiovascular strains. A higher BMI can create difficulty with intubations, recovery complications, and have a risk of blood clots. Anesthesiologists would consider a BMI of 35-40 to be dangerous to a patient's success with surgery given the issues to their respiratory and cardiovascular systems. 

---

**3. What are the most common complications that can occur during or after anesthesia? Which ones in this dataset are considered serious?**

Your answer:
Some common complications include vomiting, bleeding, sore throat, low blood pressure, and respiratory distress. In this dataset, respiratory distress is the most serious since it can directl affect the airway, creating problems for oxygen intake and could need emergency assistance.

---

**4. How is post-operative pain typically measured in clinical settings? What does a pain scale of 1-10 represent?**

Your answer:
Post-operative pain is measures on a scale of 1-10 with patients self-reporting how intense their pain feels. The 1-10 scale works with 1 being the least pain and 10 being the most pain. 

---

## Part 3: Data Validation (15 points)

### 3.1 Age Validation (5 points)

Write code to check:
- How many ages are below 18?
  0
- How many ages are above 90?
  0
- What is the actual age range?
  33 to 72
- What is the distribution of ages by decade?
  30s have 49, 40s have 50, 50s have 98, 60s have 54, 70s have 49

**Your Code:**
```
df['Age'].describe()
df['Age'].value_counts().sort_index()
```

**Your Findings:**

- Are there any impossible age values?
  No
- What age groups are most represented in surgical patients?
  The 50s
- Does this distribution make clinical sense for elective surgeries?
  Yes because a lot of people in that range begin having surgeries like hip replacement which are not emergency.
---

### 3.2 Surgery Duration Validation (5 points)

Write code to examine SurgeryDuration values closely. Notice that the values include " min" as text.

**Your Code:**
```
df['SurgeryDuration'] = df['SurgeryDuration'].str.replace(' min', '')
df['SurgeryDuration'] = df['SurgeryDuration'].astype(int)

df['SurgeryDuration'].describe()
```

**Your Findings:**

- What is the format of the SurgeryDuration column?
  The original format is text because there is 'min' recorded at the end of each number.
- How would you convert this to a numeric value for analysis?
  I would use .str.replace() to remove 'min' and convert the column to numeric with pd.to_numeric()
- What is the range of surgery durations once converted?
  The range is shown as 60 as the minimum to 240 as the maximum.
- Are there any surgery durations that seem unusually short or long?
  All of them seem normal for a given surgery.
---

### 3.3 Complications Validation (5 points)

Write code to see all unique values and their counts for the Complications column.

**Your Code:**
```
df['Complications'].value_counts(dropna=False)
```

**Your Findings:**

- What are all the possible complication values?
  None, Nausea mild bleeding, Delayed recovery, Respiratory distress
- How many patients had "None" for complications?
  73
- What percentage of patients experienced some form of complication?
  75%
- Are there any complications that might be considered more serious than others?
  Yes, respiratory is the most severe because it can directly affect someone's regular breathing.
---

## Part 4: Create Clinical Age Groups (10 points)

Create a new column called `age_group` that categorizes patients into clinically-meaningful groups.

### Version A: Anesthesia Risk Categories

Use these categories based on anesthesia risk considerations:

| Age Group | Range | Clinical Rationale |
|-----------|-------|-------------------|
| Young Adult | 18-39 | Generally lowest anesthesia risk, rapid recovery |
| Middle Adult | 40-54 | Moderate risk, potential onset of comorbidities |
| Mature Adult | 55-64 | Increased perioperative risk factors |
| Senior | 65-74 | Significant anesthesia considerations |
| Elderly | 75+ | Highest risk, requires careful monitoring |

### Your Code:

```python
df['age_group'] = pd.cut(
    df['Age'],
    bins=[18, 40, 55, 65, 75],
    labels=['Young Adult', 'Middle Adult', 'Mature Adult', 'Senior'],
    include_lowest=True
)
```

### Verify your groupings worked:

```python
df['age_group'].value_counts()
```

### Calculate outcome rate by age group:

```python
df.groupby('age_group')['Outcome'].mean()
```

### Analysis Questions:

**1. How many patients are in each age group?**

Your answer: Young adults have 49, Middle adults have 95, Mature adults have 107, and seniors have 49.

---

**2. What is the successful outcome rate (percentage) for each age group?**

Your answer: Young adults have 59%, middle adults have 40%, mature adults have 53%, and seniors have 53%. 

---

**3. At what age does surgical outcome appear to change most? Does this match what you learned in your clinical research about anesthesia risk?**

Your answer: The biggest change is with the middle adult group since the successful outcome rate drops to 40% which is lower than all other age groups.

---

## Part 5: Research Questions (15 points)

### 5.1 Write Three Answerable Questions (9 points)

Write three questions that THIS dataset can answer. Remember: the data can show relationships and patterns, but cannot prove causation.

**Your questions must explore these specific areas:**

1. **A question about anesthesia type and complications:**

Is there a relationship between the type of anesthesia used (general, regional, local) and the occurence of post-operative complications?
---

2. **A question comparing surgery types and outcomes:**

Are there some types of surgery that have a higher rate of unsuccessful outcomes than other types of surgery in this data set?
---

3. **A question about the combination of BMI AND surgery duration:**

In patients with higher BMI, is there a correlation between longer surgery times and higher complication rates or lower success rates?
---

### 5.2 Identify One Question the Data CANNOT Answer (3 points)

Write one question about **medication dosage or specific anesthesia protocols** that this dataset cannot answer, and explain why.

**Question:**
Does an increase in the dose of a particular anesthetic drug (ex: propofol dose in mg/kg) lower the risk of post-operative respiratory distress?

**Why it cannot be answered with this data:**
The dataset doesn't have specific information about the type of anesthetic drugs used, the dose of the drugs in mg/kg, the rate of infusion, or the time of administration. The dataset only has general information about the type of anesthesia, and not the specific protocols or techniques used. This means that we can't determine the effect of changes in the dose or the protocol on the rate of complications.

---

### 5.3 Grouping Analysis (3 points)

Answer this question using a groupby analysis:

**"What is the average pain level for each surgery type?"**

**Your Code:**
```
df.groupby('SurgeryType')['PainLevel'].mean()
```

**Your Interpretation:**

Which surgery type has the highest average pain level? Which has the lowest? What might explain these differences?
Neurological surgeries (4.515) have the highest average, followed by Orthopedic surgeries (4.51) and then Cosmetic(4.5). Cardiovascular surgeries (4.461) have the lowest average pain level. Neurological and Orthopedic surgeries include nerves, bones, and the spine, which are very sensitive areas and may be the reason for the higher levels of pain. Cardiovascular surgeries have more pain management strategies, which could be the reason for lower levels of pain.

---

## Submission Checklist

Before submitting, verify you have completed:

- [ ] **Part 1:** All 10 inspection steps with code AND written findings
- [ ] **Part 2:** Complete data dictionary with all 12 columns filled in
- [ ] **Part 2:** Answered all 4 clinical research questions
- [ ] **Part 3:** All 3 validation checks with code and answers
- [ ] **Part 4:** Created `age_group` column using the **Anesthesia Risk Categories**
- [ ] **Part 4:** Calculated outcome rate by age group with interpretation
- [ ] **Part 5:** Three research questions (anesthesia/complications, surgery/outcomes, BMI+duration)
- [ ] **Part 5:** One unanswerable question about medication/protocols
- [ ] **Part 5:** Pain level by surgery type groupby analysis

---

## Grading Rubric

| Component | Points | Requirements for Full Credit |
|-----------|--------|------------------------------|
| Part 1: 10-Point Inspection | 40 | All 10 steps complete with working code AND thoughtful written analysis |
| Part 2: Data Dictionary | 20 | All 12 columns documented with correct feature types and clinical research |
| Part 3: Data Validation | 15 | All validation checks complete with working code and insightful answers |
| Part 4: Age Groups | 10 | Working code that creates correct groups AND meaningful interpretation |
| Part 5: Research Questions | 15 | Three good questions in specified areas, one clear limitation, groupby analysis complete |
| **Total** | 100 | |

---

## Hints (Read Before You Get Stuck!)

### ⚠️ Common Pitfalls:

1. **SurgeryDuration contains text** - The values look like "217 min" which is a string, not a number. You'll need to extract the numeric portion before doing any calculations with this column.

2. **Multiple values in some columns** - PreoperativeNotes and Complications may contain multiple conditions separated by commas. Think about how this affects analysis.

3. **Outcome encoding** - Make sure you understand what 0 and 1 represent before interpreting your results.

4. **Age distribution** - This dataset contains adult surgical patients. The age groupings reflect perioperative risk categories.

### 💡 Pro Tips:

- Use `value_counts()` liberally to understand categorical columns
- Use `value_counts(dropna=False)` to see if there are any null values
- For the SurgeryDuration column, consider using string methods like `.str.replace()` to clean the data
- When something seems weird, investigate it—don't assume it's an error
- Always state what the data CAN tell you vs. what would require additional information

---

## Useful Resources

- **ASA Physical Status Classification:** https://www.asahq.org/standards-and-guidelines/asa-physical-status-classification-system
- **Anesthesia Types Overview:** https://www.asahq.org/madeforthismoment/preparing-for-surgery/types-of-anesthesia/
- **BMI and Surgical Risk:** https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4890841/
- **Pain Assessment Scales:** https://www.ncbi.nlm.nih.gov/books/NBK556098/
- **Pandas Documentation:** https://pandas.pydata.org/docs/

---

*Remember: "Every Column Tells a Story" - your job is to figure out what that story is!*

---
