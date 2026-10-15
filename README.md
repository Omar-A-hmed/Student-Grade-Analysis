Student-Grade-Analysis Project
Project documentation
This project reads student marks from an Excel file, checks subject scores, calculates totals and averages, and assigns pass, grade, and result labels. It uses Pandas for table operations and NumPy for conditional labels. The main learning challenge was combining three subject columns into one Total column.
1 Project Title
Student-Grade-Analysis Project
Student name: Omar Ahmed.M
Class / section: 4th yr CSE B 
College Name: Aalim Muhammed Salegh College Of Engineering
Date:15/10/26
2 Objective
Automate class marks analysis: identify missing and out-of-range marks, calculate totals and averages, determine pass/fail status and grades, identify highest and lowest averages, and report subject averages.
3 Technologies Used
•	Python for program logic.
•	Pandas for reading Excel and DataFrame operations.
•	NumPy for conditional labels.
•	Excel workbook data.xlsx as the expected input.
4 Dataset Description
The workbook contains one worksheet, Sheet1, with 10 student records and five columns: Student ID, Name, Maths, Science, and English. All 10 records have values in all five columns. The subject marks are whole numbers from 45 to 98. The workbook therefore contains no missing values in the fields used by the program.
5 Implementation
1 Imports and input file
import pandas as pd gives access to Pandas as pd. import numpy as np imports NumPy as np for conditional selection. pd.read_excel("data.xlsx") reads the workbook into data. The file must be in the current working folder and its headers must match those used in the code.
2 Define and start the analysis function
def class_Data(data): defines a function that receives the loaded table. df = pd.DataFrame(data) makes a DataFrame for column and row operations. print(df) displays the original data before calculated columns are added.
3 Identify missing marks
For each subject, isna() returns True for a missing mark and False otherwise. The three results are stored in Missing Math Marks, Missing Science Marks, and Missing English Marks. Each flag corresponds to one student row.
4 Identify marks outside the expected range
Each invalid check combines notna(), which selects marks that exist, with ~between(1,100), which selects values outside the inclusive 1-to-100 range. The & operator combines the checks row by row. The three Invalid columns flag present but out-of-range scores. The code flags these values but does not remove or correct them.
5 Calculate a total and average for each student
df["Total"] = df["Maths"] + df["Science"] + df["English"] adds the three subject columns horizontally, row by row. Each row gets one total while the original columns remain. I found this challenging because I did not know how to combine multiple columns into one calculated column. df["Average"] = df["Total"]/3 divides each total by three.
6 Decide pass or fail
Each comparison, such as df["Maths"] >= 50, produces a True or False value for every row. Parentheses group the four checks, and & combines them element by element: all three marks and the Average must be at least 50. np.where(condition, "Pass", "Fail") writes Pass when all checks are True and Fail otherwise. I found it difficult to understand how to combine each row's values and checks to produce one label per student; this expression performs that row-wise check. With three numeric marks, an average of at least 50 is implied when every subject mark is at least 50, but the code checks it explicitly.
7 Assign a letter grade
Nested np.where expressions test Average from the highest threshold downward: 90 or more is A, 80 or more B, 70 or more C, 50 or more D, and lower averages F. The first satisfied condition determines the grade.
8 Label overall and class results
The Result expression first compares each Average with the maximum and minimum. A maximum is labeled Highest and a minimum Lowest. Other rows receive First Class for A, Second Class for B, Third Class for C, and Average for D or F. Ties at the maximum or minimum label all matching rows. Highest is checked first, so a one-student dataset would be labeled Highest.
9 Show the IDs and names of top and bottom students
df.loc selects the Student ID or Name for rows labeled Highest or Lowest. These four columns have values only on matching rows; other rows show blank/NaN.
10 Calculate average marks by subject
mean() calculates the average of each subject column. The f-strings create display labels such as Maths=73.8; separate numeric variables keep the means for comparison. Pandas mean skips missing values by default.
11 Print the highest-scoring subject
highest_Scored_Subject is intended to compare the three subject means and print the highest. In the supplied function, the parameters are unused. Also, a condition like Maths_Avg_in_Numbers > Science_Avg_in_Numbers and English_Avg_in_Numbers only checks whether the English mean is truthy; it does not compare Maths with English. The printed subject can therefore be wrong. Correct logic should compare each mean directly with both others. Ties are not explicitly handled.
12 Assemble and print the output
The Marks expression selects identity, scores, calculations, outcomes, student IDs/names for highest and lowest rows, and validation flags. The commas after that selection make Marks a tuple containing the selected DataFrame and three formatted subject-average strings, rather than a single DataFrame. print(Marks) displays the tuple. The final function call passes the numeric subject means to highest_Scored_Subject.
Calls class_Data with the Excel data and starts the analysis.
import pandas as pd
Loads Pandas for tables; pd is the short name used below.
import numpy as np
Loads NumPy as np, used to choose labels such as Pass or Fail.
data = pd.read_excel("data.xlsx")
Reads data.xlsx and stores its table in data. The file must be in Python current folder.
def class_Data(data):
Starts the class_Data function. It receives the loaded table; the indented lines belong to it.
df = pd.DataFrame(data)
Makes a Pandas table called df from the input data.
print(df)
Shows the original table before new columns are added.
df["Missing Math Marks"] = df["Maths"].isna()
Checks each Maths cell and marks True when it is empty, False otherwise.
df["Missing Science Marks"] = df["Science"].isna()
Checks each Science cell for an empty value.
df["Missing English Marks"] = df["English"].isna()
Checks each English cell for an empty value.
df["Invalid Math Marks"] = df["Maths"].notna() & ~df["Maths"].between(1,100)
Flags Maths values that exist but are below 1 or above 100. notna means present; between checks the range; ~ reverses that check; & requires both.
df["Invalid Science Marks"] = df["Science"].notna() & ~df["Science"].between(1,100)
Flags present Science marks outside 1 through 100.
df["Invalid English Marks"] = df["English"].notna() & ~df["English"].between(1,100)
Flags present English marks outside 1 through 100. These lines flag values but do not fix them.
df["Total"] = df["Maths"] + df["Science"] + df["English"]
Adds the three marks across each row, giving every student one Total. Original columns remain.
df["Average"] = df["Total"]/3
Divides each student total by three and saves the answer as Average.
df["Pass"] = np.where((df["Maths"]>=50) & (df["Science"]>=50) & (df["English"]>=50) & (df["Average"]>=50),"Pass","Fail")
Checks each row: all three marks and the average must be at least 50. & joins the True/False checks. np.where writes Pass if all are true, otherwise Fail.
df["Grade"] = np.where(df["Average"]>=90,"A",np.where(df["Average"]>=80,"B",np.where(df["Average"]>=70,"C",np.where(df["Average"]>=50,"D","F"))))
Assigns A for average >=90, B for >=80, C for >=70, D for >=50, else F. Nested np.where checks the next threshold when the prior one fails.
df["Result"] = np.where(df["Average"]==df["Average"].max(),"Highest",np.where(df["Average"]==df["Average"].min(),"Lowest",np.where(df["Grade"]=="A","First Class",np.where(df["Grade"]=="B","Second Class",np.where(df["Grade"]=="C","Third Class","Average")))))
Labels the maximum average Highest, else the minimum Lowest. Others are labeled by grade: A First Class, B Second Class, C Third Class, D/F Average. Tied maximum/minimum students all get the label.
df["Highest Student ID"] = df.loc[df["Result"]=="Highest","Student ID"]
Finds IDs for Highest rows. Other rows have no value in this new column.
df["Lowest Student ID"] = df.loc[df["Result"]=="Lowest","Student ID",]
Finds IDs for Lowest rows. The trailing comma is unnecessary and can be removed.
df["Highest Name"] = df.loc[df["Result"]=="Highest","Name"]
Finds names for Highest rows. Other rows have no value here.
df["Lowest Name"] = df.loc[df["Result"]=="Lowest","Name",]
Finds names for Lowest rows. The trailing comma is unnecessary.
Maths_Average = f"Maths={float(df["Maths"].mean())}"
Calculates the Maths mean and formats it as text, e.g. Maths=73.8. This nested quote form may need Python 3.12 or adjusted quotes.
Science_Average = f"Science={float(df["Science"].mean())}"
Calculates and formats the Science mean for display.
English_Average = f"English={float(df["English"].mean())}"
Calculates and formats the English mean for display.
Maths_Avg_in_Numbers = df["Maths"].mean()
Stores the numeric Maths mean for comparisons.
Science_Avg_in_Numbers = df["Science"].mean()
Stores the numeric Science mean for comparisons.
English_Avg_in_Numbers = df["English"].mean()
Stores the numeric English mean for comparisons.
def highest_Scored_Subject(Maths_Avg,Science_Avg,English_Avg):
Defines a function intended to compare and print the highest subject average.
if Maths_Avg_in_Numbers>Science_Avg_in_Numbers and English_Avg_in_Numbers:
Intended to test if Maths is highest. But the last part only checks that English mean is nonzero; it does not compare Maths to English.
print("Maths is the Highest Scored Subject with a score of",Maths_Average)
Prints the Maths message and formatted mean if the if condition is true.
elif Science_Avg_in_Numbers>Maths_Avg_in_Numbers and English_Avg_in_Numbers:
Intended to test if Science is highest. As written, it only checks that English is nonzero instead of comparing Science with English.
print("Science is the Highest Scored Subject with a score of",Science_Average)
Prints the Science message and formatted mean if the elif condition is true.
else:
Runs when neither earlier condition was true.
print("English is the Highest Scored Subject",English_Average)
Prints English as highest in the remaining case; earlier comparison problems can make this answer incorrect.
Marks = df[["Student ID","Name","Maths","Science","English","Total","Average","Pass","Grade","Result","Highest Student ID","Highest Name","Lowest Student ID","Lowest Name","Missing Math Marks","Invalid Math Marks","Missing Science Marks","Invalid Science Marks","Missing English Marks","Invalid English Marks"]],Maths_Average,Science_Average,English_Average
This selects Student ID, Name, all three subject marks, Total, Average, Pass, Grade, Result, the highest/lowest IDs and names, and each missing/invalid flag. The commas after the selected DataFrame make Marks a tuple containing that DataFrame and the three formatted mean strings.
print(Marks)
Displays that tuple: the selected table plus the three mean strings.
highest_Scored_Subject(Maths_Avg_in_Numbers,Science_Avg_in_Numbers,English_Avg_in_Numbers)
Calls the helper and passes the three numeric means. The function currently does not use these parameters.
class_Data(data)
6 Key Features
•	Missing and invalid mark checks for all subjects.
•	Per-student total and average.
•	Pass/fail and letter grade assignment.
•	Highest/lowest average labels and intended student identification.
•	Average score by subject and intended highest-subject message.
7 Output and Screenshots
The program is intended to print the input DataFrame, selected result fields with calculations and validation flags, and a subject-average message. A screenshot of a successful program run was not provided.
Based on the supplied workbook and the calculations described, 9 of 10 students meet the Pass conditions. The subject averages are Maths 73.8, Science 74.2, and English 75.8. Vikram Singh (AMS007) has the highest average at 94.33, and Rahul Verma (AMS005) has the lowest at 48.33. These figures are calculated from the workbook values; they are not presented as a captured program run.
8 Final Outcome
The workflow brings common class-results calculations together and can produce totals, averages, validation flags, pass/fail labels, grades, and summary information when the expected columns and numeric marks are present. For this workbook, the highest average belongs to Vikram Singh and the lowest to Rahul Verma.
9 Challenges and Learning
A challenge I faced was figuring out how to combine multiple DataFrame columns into a single column. I knew how to work with Maths, Science, and English separately, but did not know how to add them for each student. I learned that Pandas adds the columns row by row with df["Total"] = df["Maths"] + df["Science"] + df["English"]. That creates one Total value per student while keeping the original subject columns. I also practiced missing/invalid checks, averages, and conditional classifications.
Another challenge was deciding how to combine several conditions for each student in the Pass calculation. I was unsure how to check the Maths, Science, and English marks and the Average together, row by row, to decide whether each student passed. I learned that the & operator joins these True/False checks element by element across the DataFrame rows, and np.where writes Pass when all four conditions are True and Fail otherwise. This produces a Pass or Fail label for each student.
10 GitHub Repository
Repository link: https://github.com/Omar-A-hmed/Student-Grade-Analysis
