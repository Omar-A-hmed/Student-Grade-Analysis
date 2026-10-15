# Student-Grade-Analysis Project

Author: Omar Ahmed M  
Class / Section: 4th yr CSE B  
College: Aalim Muhammed Salegh College Of Engineering  
Date: 2026-10-15

## 1. Project Summary
This project reads student marks from an Excel file, validates the marks, computes totals and averages, determines pass/fail status and letter grades, identifies the highest and lowest averages, and summarizes subject-level averages. It uses Pandas for table operations and NumPy for conditional labeling.

## 2. Objective
Automate class marks analysis to:
- Identify missing and out-of-range marks.
- Calculate per-student totals and averages.
- Determine pass/fail status and assign letter grades.
- Identify top and bottom students by average.
- Compute subject-wise average scores and the highest-scoring subject.

## 3. Technologies Used
- Python (program logic)
- Pandas (read Excel, DataFrame operations)
- NumPy (conditional selection / labeling)
- Excel workbook `data.xlsx` expected as input (Sheet1)

## 4. Dataset Description
The workbook `data.xlsx` contains one worksheet (Sheet1) with ~10 student records. Expected columns:
- `Student ID`
- `Name`
- `Maths`
- `Science`
- `English`

All numeric subject marks are expected to be in the inclusive range 1–100.

## 5. Usage
1. Install requirements:
```bash
pip install pandas numpy openpyxl
```
2. Put `data.xlsx` in the same folder as the script.
3. Run the analysis script (example):
```bash
python analyze_marks.py
```
(The repository's script should load `data.xlsx`, call `class_Data(data)`, and print the results.)

## 6. Implementation (high level)
- Load Excel into a Pandas DataFrame: `pd.read_excel("data.xlsx")`.
- Create validation flags:
  - `Missing <Subject> Marks` using `.isna()`.
  - `Invalid <Subject> Marks` using `.notna() & ~.between(1,100)`.
- Compute `Total` and `Average` per student.
- Determine `Pass` if all three subject marks and the average are >= 50, else `Fail`.
- Assign letter `Grade` using thresholds: A (>=90), B (>=80), C (>=70), D (>=50), F (else).
- Label `Result`:
  - `"Highest"` for rows whose average == max average,
  - `"Lowest"` for rows whose average == min average,
  - Otherwise map grades to class labels (First/Second/Third/Average).
- Extract top/bottom student ID and Name into dedicated columns using `.loc[...]`.
- Compute subject means with `.mean()` and determine the highest-scoring subject.

## 7. Key Features
- Missing-value detection per subject.
- Invalid-range detection (marks outside 1–100).
- Per-student totals, averages, pass/fail, and letter grades.
- Automatic identification of highest and lowest average students.
- Subject-wise average calculation and highest-scoring subject reporting.

## 8. Output
The program prints:
- The original input DataFrame.
- A results table containing Student ID, Name, subject marks, Total, Average, Pass, Grade, Result, highest/lowest student fields, and validation flags.
- Subject average values and a message indicating which subject has the highest average.

## 9. Known issues / suggested fixes
While reviewing the README contents and referenced code, I noticed a few implementation issues you may want to fix:

- Trailing commas in `.loc[...]` calls (e.g., `df.loc[...,"Student ID",]`) are unnecessary; remove the trailing comma.
- The `highest_Scored_Subject` function uses global numeric variables (`Maths_Avg_in_Numbers`, etc.) instead of the function parameters, and its comparisons are incorrect (e.g., `if Maths_Avg_in_Numbers>Science_Avg_in_Numbers and English_Avg_in_Numbers:` does not compare Maths to English). Fix like:
  ```python
  def highest_scored_subject(math_avg, sci_avg, eng_avg):
      if math_avg >= sci_avg and math_avg >= eng_avg:
          print("Maths is the highest-scored subject with a score of", math_avg)
      elif sci_avg >= math_avg and sci_avg >= eng_avg:
          print("Science is the highest-scored subject with a score of", sci_avg)
      else:
          print("English is the highest-scored subject with a score of", eng_avg)
  ```
- When building formatted mean strings with f-strings, prefer using different quote styles or intermediate variables to avoid nested-quote issues. Example:
  ```python
  maths_avg = df["Maths"].mean()
  maths_avg_str = f"Maths={maths_avg:.1f}"
  ```
- The selection assigned to `Marks` in the README appeared to become a tuple due to stray commas—ensure you select a DataFrame slice directly (no trailing commas) and then `print(Marks)`.

## 10. Final outcome
With the above checks and fixes applied, the script will produce a clear report of student marks, validation flags, totals/averages, pass/fail and grades, top/bottom students, and subject-level summaries.

## 11. Repository
https://github.com/Omar-A-hmed/Student-Grade-Analysis
