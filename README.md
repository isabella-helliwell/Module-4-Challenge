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

         Next step is to replace all the 9th grade student's math and reading grades with "NaN" (not a number) value for Thomas High School.
         
             # Refactor the code in Step 2 to replace the math scores with NaN's.

             student_data_df.loc[(student_data_df["school_name"] =="Thomas High School")&(student_data_df["grade"] =="9th"), "reading_score"]=np.nan
             student_data_df.loc[(student_data_df["school_name"] =="Thomas High School")&(student_data_df["grade"] =="9th"), "math_score"]=np.nan
             # Check the student data for NaN's. 
             student_data_df.tail(10)
            
output.2
![image](https://user-images.githubusercontent.com/85843030/126567757-e80f079f-c25e-4cbc-aac6-5ede651b005b.png)

          We need to combine the shcool data and the student data in order to be able to carry on with the analysis. This is done by using a common
          column and merge the two data sets together into a single data set.
             # Combine the data into a single dataset
                  school_data_complete_df = pd.merge(student_data_df, school_data_df, how="left", on=["school_name", "school_name"])
                  school_data_complete_df.tail(10)

output.3
![image](https://user-images.githubusercontent.com/85843030/126568214-efe70d7d-807d-4b1d-8531-5a896837150f.png)

          Next is to carry out some calculations, 
          1-total school and student count 
          2-average math, average reading, and overall average scores for all students in the schools
          3-total students in grade 9 at Thomas High School
          4-new students count, 
          
