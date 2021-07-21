# School District Analysis
## 1. Purpose
      The purpose of this analysis is to create a school district summary and a school summary where one of the schools graders have been removed.
      The Thoma High School's results for math and reading for all the 9 graders have been set to Nan's.
      First part of the analysis is to create
      * district summary
      * school summary
      Remove the Thomas High School 9 graders result and update the
      * district summary
      * school summary
      
      Second part of the analysis is to sort the results and show:
      * the top five and bootom five schools
      * average scores for each school
      
      Third part of the analysis is 
      * to calculate the spending for each school
      * calculate the scores by school size
      * calculate the scores by type of schools
## 2. Resources

  - Data source: students_complete.csv
  - Software: Python 3.7.6, Visual Studio Code version 1.58
  - web application: Jupyter Notebook
## 3. Analysis
### 3.1 District Summary and School Summary
#### 3.1.1 District Summary
      The first step is to import dependencies such as pandas and numpy and to load and read the.csv file
      Since the data contains prefixes and suffixes, we need to clean up the data, by creating a new list called "prefixes_suffixes",
      and iterate through the words and replace them with empty space.
      # Dependencies and Setup
            import pandas as pd
            import numpy as np

            # File to Load (Remember to change the path if needed.)
            school_data_to_load = "Resources/schools_complete.csv"
            student_data_to_load = "Resources/students_complete.csv"

            # Read the School Data and Student Data and store into a Pandas DataFrame
            school_data_df = pd.read_csv(school_data_to_load)
            student_data_df = pd.read_csv(student_data_to_load)

            # Cleaning Student Names and Replacing Substrings in a Python String
            # Add each prefix and suffix to remove to a list.
            prefixes_suffixes = ["Dr. ", "Mr. ","Ms. ", "Mrs. ", "Miss ", " MD", " DDS", " DVM", " PhD"]

            # Iterate through the words in the "prefixes_suffixes" list and replace them with an empty space, "".
            for word in prefixes_suffixes:
                student_data_df["student_name"] = student_data_df["student_name"].str.replace(word,"")

            # Check names.
            student_data_df.head(10)

output.1
![image](https://user-images.githubusercontent.com/85843030/126567144-cbaf49e3-13c2-4ba6-a182-00c699aa8b71.png)

            
            
            
            
