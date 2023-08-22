# Pandas-Challenge
create and manipulate Pandas DataFrames to analyze school and standardized test data
# Background
You are the new Chief Data Scientist for your city's school district. In this capacity, you'll be helping the school board and mayor make strategic decisions regarding future school budgets and priorities.

As a first task, you've been asked to analyze the district-wide standardized test results. You'll be given access to every student's math and reading scores, as well as various information on the schools they attend. Your task is to aggregate the data to showcase obvious trends in school performance.
District Summary
Perform the necessary calculations and then create a high-level snapshot of the district's key metrics in a DataFrame.
```

# Dependencies and Setup
import pandas as pd
from pathlib import Path

# File to Load (Remember to Change These)
school_data_to_load = Path("Resources/schools_complete.csv")
student_data_to_load = Path("Resources/students_complete.csv")

# Read School and Student Data File and store into Pandas DataFrames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

# Combine the data into a single dataset.  
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
school_data_complete.head(20)
```

Include the following:

## Total number of unique schools
```
# Calculate the total number of unique schools
school_count = len(school_data_complete ["school_name"].unique())
school_count

```

## Total Students
```
# Calculate the total number of students
#student_count = len(school_data_complete ["student_name"].unique())
#Maybe they are students with the same name in different levels or different school
student_count = len(school_data_complete ["student_name"])
student_count
```
## Total budget
```
# Calculate the total budget
total_budget =sum(school_data ["budget"])
total_budget

```
## Total budget
```
# Calculate the average (mean) math score
average_math_score = school_data_complete ["math_score"].mean()
average_math_score
```
## Average math score
```
# Calculate the average (mean) reading score
average_reading_score = school_data_complete ["reading_score"].mean()
average_reading_score
```
## Average reading score
```
# Calculate the average (mean) reading score
average_reading_score = school_data_complete ["reading_score"].mean()
average_reading_score
```
## % passing math (the percentage of students who passed math)
```
# Use the following to calculate the percentage of students who passed math (math scores greather than or equal to 70)
passing_math_count = school_data_complete[(school_data_complete["math_score"] >= 70)].count()["student_name"]
passing_math_percentage = passing_math_count / float(student_count) * 100
passing_math_percentage
```
## % passing reading (the percentage of students who passed reading)
```
# Calculate the percentage of students who passed reading (hint: look at how the math percentage was calculated)  
passing_reading_count = school_data_complete[(school_data_complete["reading_score"] >= 70)].count()["student_name"]
passing_reading_percentage = passing_reading_count/ float(student_count) * 100
passing_reading_percentage 
```
## % overall passing (the percentage of students who passed math AND reading)
```
# Use the following to calculate the percentage of students that passed math and reading
passing_math_reading_count = school_data_complete[
    (school_data_complete["math_score"] >= 70) & (school_data_complete["reading_score"] >= 70)
].count()["student_name"]
overall_passing_rate = passing_math_reading_count /  float(student_count) * 100
overall_passing_rate
#to see the type of the result o know how to used it after 
type(school_count)
type(passing_math_percentage)
```
# Create a high-level snapshot of the district's key metrics in a DataFrame
```
district_summary = pd.DataFrame({
    'Total Schools': [school_count],
    'Total Students': [student_count],
    'Total Budget': [total_budget],
    'Average Math Score': [average_math_score],
    'Average Reading Score': [average_reading_score],
    '% Passing Math': [passing_math_percentage],
    '% Passing Reading': [passing_reading_percentage],
    '% Overall Passing': [overall_passing_rate]
})

# Formatting
district_summary["Total Students"] = district_summary["Total Students"].map("{:,}".format)
district_summary["Total Budget"] = district_summary["Total Budget"].map("${:,.2f}".format)
# Display the DataFrame
district_summary
```

# School Summary

Perform the necessary calculations and then create a DataFrame that summarizes key metrics about each school.

Include the following:

## School name
```
# Use the code provided to select all of the school types
#school_types = school_data_complete["type"].unique()

school_types = school_data_complete.groupby('school_name')['type'].unique().reset_index()
school_types.columns = ['school_name', 'School_type']
school_types
```
## School type
```
# Use the code provided to select all of the school types
#school_types = school_data_complete["type"].unique()

school_types = school_data_complete.groupby('school_name')['type'].unique().reset_index()
school_types.columns = ['school_name', 'School_type']
school_types
```
## Total students count per school
```
# Calculate the total student count per school
per_school_counts=school_data_complete["school_name"].value_counts()
per_school_student_counts = school_data_complete.groupby('school_name')['student_name'].count().reset_index()
per_school_student_counts.columns = ['school_name', 'per_school_student_count']
type(per_school_counts)

```
## Total school budget
```
# Calculate the total school budget and per capita spending per school
total_school_budget = school_data_complete.groupby('school_name')['budget'].max().reset_index()
total_school_budget.columns = ['school_name', 'total_school_budget']
total_school_budget.head(10)
```


## Per student budget
```
# Calculate the  budget per student spending per school
per_school_capita = total_school_budget['total_school_budget'] / per_school_student_counts['per_school_student_count']
per_school_capita_df = pd.DataFrame({
    'school_name': total_school_budget['school_name'],
    'per_school_capita': per_school_capita
})

per_school_capita_df.head(10)

```

## Average math score
```
# Calculate the average test scores per school
per_school_math = school_data_complete.groupby('school_name')['math_score'].mean()
```
## Average reading score
```
per_school_reading =school_data_complete.groupby('school_name')['reading_score'].mean()
```
## Average math &reading score
```
per_school_test_average = pd.DataFrame({
    'per_school_math': per_school_math,
    'per_school_reading': per_school_reading
})

per_school_test_average
```
## % passing math (the percentage of students who passed math)
```
# Calculate the number of students per school with math scores of 70 or higher
students_passing_math = school_data_complete[
    (school_data_complete["math_score"] >= 70)]
school_students_passing_math = students_passing_math.groupby(["school_name"]).size()
school_students_passing_math
```
## % passing reading (the percentage of students who passed reading)
```
# Calculate the number of students per school with reading scores of 70 or higher
students_passing_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70)]
school_students_passing_reading = students_passing_reading.groupby(["school_name"]).size()
school_students_passing_reading
```
## % overall passing (the percentage of students who passed math AND reading)
```
# Use the provided code to calculate the number of students per school that passed both math and reading with scores of 70 or higher
students_passing_math_and_reading = school_data_complete[
    (school_data_complete["reading_score"] >= 70) & (school_data_complete["math_score"] >= 70)
]
school_students_passing_math_and_reading = students_passing_math_and_reading.groupby(["school_name"]).size()
school_students_passing_math_and_reading
type(school_students_passing_math_and_reading)
```
## Highest-Performing Schools (by % Overall Passing)
Sort the schools by % Overall Passing in descending order and display the top 5 rows.
```
# Use the provided code to calculate the passing rates
per_school_passing_math = (school_students_passing_math /per_school_counts )* 100
per_school_passing_reading = (school_students_passing_reading / per_school_counts )* 100
overall_passing_rate = (school_students_passing_math_and_reading / per_school_counts )* 100

per_school_passing_df = pd.DataFrame({
    'per_school_passing_math': per_school_passing_math,
    'per_school_passing_reading': per_school_passing_reading,
    'overall_passing_rate': overall_passing_rate
})

```
## Create a DataFrame called `per_school_summary` with columns for the calculations above.

```
per_school_summary = pd.DataFrame({
    'School Type': school_types['School_type'].values,
    'Total Students': per_school_student_counts['per_school_student_count'].values,
    'Total School Budget': total_school_budget['total_school_budget'].values,
    'Per Student Budget': per_school_capita_df['per_school_capita'].values,
    'Average Math Score': per_school_test_average['per_school_math'].values,
    'Average Reading Score': per_school_test_average['per_school_reading'].values,
    '% Passing Math': per_school_passing_df['per_school_passing_math'].values,
    '% Passing Reading': per_school_passing_df['per_school_passing_reading'].values,
    '% Overall Passing': per_school_passing_df['overall_passing_rate'].values
}, index=[school_types['school_name'].values])  
# Formatting
per_school_summary["Total School Budget"] = per_school_summary["Total School Budget"].map("${:,.2f}".format)
per_school_summary["Per Student Budget"] = per_school_summary["Per Student Budget"].map("${:,.2f}".format)
# Display the DataFrame
per_school_summary.head(5)
```

## Highest-Performing Schools (by % Overall Passing)
```
# Sort the schools by `% Overall Passing` in descending order
sorted_per_school_summary = per_school_summary.sort_values(by='% Overall Passing', ascending=False)

# Display the top 5 rows
top_5_schools = sorted_per_school_summary.head(5)
top_5_schools
```
## Lowest-Performing Schools (by % Overall Passing)
Sort the schools by % Overall Passing in ascending order and display the top 5 rows.
```
# Sort the schools by `% Overall Passing` in ascending order and display the top 5 rows.
## Save the results in a Dadata frame called "bottom_schools".
bottom_schools  = per_school_summary.sort_values(by='% Overall Passing', ascending=True)
bottom_schools.head(5)
```
## Math Scores by Grade
Perform the necessary calculations to create a DataFrame that lists the average math score for students of each grade level (9th, 10th, 11th, 12th) at each school.
```
 Use the code provided to separate the data by grade
ninth_graders = school_data_complete[school_data_complete["grade"] == "9th"]
tenth_graders = school_data_complete[school_data_complete["grade"] == "10th"]
eleventh_graders = school_data_complete[school_data_complete["grade"] == "11th"]
twelfth_graders = school_data_complete[school_data_complete["grade"] == "12th"]

# Group by `school_name` and take the mean of the `math_score` column for each grade.
ninth_grade_math_scores = ninth_graders.groupby('school_name')['math_score'].mean()
tenth_grader_math_scores = tenth_graders.groupby('school_name')['math_score'].mean()
eleventh_grader_math_scores = eleventh_graders.groupby('school_name')['math_score'].mean()
twelfth_grader_math_scores = twelfth_graders.groupby('school_name')['math_score'].mean()

# Combine each of the scores above into a single DataFrame called `math_scores_by_grade`
math_scores_by_grade = pd.concat(
    [ninth_grade_math_scores, tenth_grader_math_scores, eleventh_grader_math_scores, twelfth_grader_math_scores],
    axis=1
)
# Set column names
math_scores_by_grade.columns = ['9th', '10th', '11th', '12th']  
# Minor data wrangling
math_scores_by_grade.index.name = None
# Display the DataFrame
math_scores_by_grade
```
## Reading Scores by Grade
Create a DataFrame that lists the average reading score for students of each grade level (9th, 10th, 11th, 12th) at each school.
Scores by School Spending
```
# Use the code provided to separate the data by grade
ninth_graders = school_data_complete[(school_data_complete["grade"] == "9th")]
tenth_graders = school_data_complete[(school_data_complete["grade"] == "10th")]
eleventh_graders = school_data_complete[(school_data_complete["grade"] == "11th")]
twelfth_graders = school_data_complete[(school_data_complete["grade"] == "12th")]

# Group by `school_name` and take the mean of the `reading_score` column for each .
ninth_grade_reading_scores = ninth_graders.groupby('school_name')['reading_score'].mean()
tenth_grader_reading_scores = tenth_graders.groupby('school_name')['reading_score'].mean()
eleventh_grader_reading_scores = eleventh_graders.groupby('school_name')['reading_score'].mean()
twelfth_grader_reading_scores = twelfth_graders.groupby('school_name')['reading_score'].mean()

# Combine each of the scores above into a single DataFrame called `reading_scores_by_grade`
reading_scores_by_grade = pd.concat(
    [ninth_grade_reading_scores, tenth_grader_reading_scores, eleventh_grader_reading_scores, twelfth_grader_reading_scores],
    axis=1
)
reading_scores_by_grade.columns = ['9th', '10th', '11th', '12th']  
# Minor data wrangling
reading_scores_by_grade = reading_scores_by_grade[["9th", "10th", "11th", "12th"]]
reading_scores_by_grade.index.name = None
# Display the DataFrame
reading_scores_by_grade
```
# Scores by School Spending

## Create a table that breaks down school performance based on average spending ranges (per student).
### Use the code provided below to create four bins with reasonable cutoff values to group school spending.
```
# Establish the bins pd.cut
spending_bins = [0, 585, 630, 645, 680]

labels = ["<$585", "$585-630", "$630-645", "$645-680"]
```
# Create a copy of the school summary since it has the "Per Student Budget" 
```
school_spending_df = per_school_summary.copy()
```
### Use pd.cut to categorize spending based on the bins.
```
school_spending_df["Spending Ranges (Per Student)"] = 
school_spending_df
```
### Use the following code to then calculate mean scores per spending range.
```
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()
Use the scores above to create a DataFrame called spending_summary.
```
### Include the following metrics in the table:
Average math score,Average reading score,% passing math (the percentage of students who passed math),% passing reading (the percentage of students who passed reading),% overall passing (the percentage of students who passed math AND reading),Scores by School Size


### Use the following code to bin the per_school_summary.

size_bins = [0, 1000, 2000, 5000]
labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
### Use pd.cut on the "Total Students" column of the per_school_summary DataFrame.
```
# Establish the bins and labels
spening_bins = [0, 585, 630, 645, 680]
labels = ["<$585", "$585-630", "$630-645", "$645-680"]
# Use pd.cut() to categorize spending based on the bins
school_spending_df["Spending Ranges (Per Student)"] = pd.cut(
    school_spending_df["Per Student Budget"],
    bins=spending_bins,
    labels=labels
)
```
#  Calculate averages for the desired columns. 
```
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()
```
## Create a DataFrame called size_summary that breaks down school performance based on school size (small, medium, or large).
```

```
