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
          
          1-total school and student count: 
          
          # Calculate the Totals (Schools and Students)
                  school_count = len(school_data_complete_df["school_name"].unique())
                  student_count = school_data_complete_df["Student ID"].count()

                  # Calculate the Total Budget
                  total_budget = school_data_df["budget"].sum()
          
          2-average math, average reading, and overall average scores for all students in the schools:
          
                  # Calculate the Average Scores using the "clean_student_data".
                  # this average is calculated using total students, including the 'Nan values'
                  average_reading_score = school_data_complete_df["reading_score"].mean()
                  average_math_score = school_data_complete_df["math_score"].mean()
                  average_reading_score
                  average_math_score
                  
          3-total students in grade 9 at Thomas High School
          
                  # Get the number of students that are in ninth grade at Thomas High School.
                  # These students have no grades. 

                  student_count_grade_9 = school_data_complete_df.loc[(school_data_complete_df["school_name"]== "Thomas High School")
                                                                     & (school_data_complete_df["grade"]==   "9th")].count()["student_name"]                                                                                         
                  school_data_complete_df.isnull().sum()
                  # Get the total student count 
                  student_count = school_data_complete_df["Student ID"].count()

                  # Subtract the number of students that are in ninth grade at 
                  #Thomas High School from the total student count to get the new total student count.
                  student_count_new = (student_count)-(student_count_grade_9)

                  student_count_new
          
          4- Calculate the passing rates using the "clean_student_data". 
                   passing_math_count = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)].count()["student_name"]
                   passing_reading_count = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)].count()["student_name"]
                   passing_math_count
                   passing_reading_count           
          
          5-  Calculate the passing percentages with the new total student count.
                   passing_math_count_new= school_data_complete_df[(school_data_complete_df["math_score"] >= 70)&(school_data_complete_df["math_score"]!=np.nan)].count()                                                                                                                                                                  ["student_name"]
                  passing_reading_count_new= school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)&(school_data_complete_df["reading_score"]!=np.nan)].count()                                                                                                                                                          ["student_name"]
          
                  
          6- Calculate the percentage of passing math and reading scores per school.
                  passing_math_percentage = passing_math_count_new / student_count_new * 100
                  passing_reading_percentage = passing_reading_count_new / student_count_new *100
          
          
          7- Calculate the students who passed both reading and math.
                  passing_math_reading_new= school_data_complete_df[(school_data_complete_df["math_score"] >= 70)
                                                                  &(school_data_complete_df["math_score"]!=np.nan)
                                                                  & (school_data_complete_df["reading_score"] >= 70)
                                                                  & (school_data_complete_df["reading_score"]!=np.nan)]
                                                                  
                                                                  
                                                                  
          
          
          
          
          
          8-Calculate the number of students that passed both reading and math.
                  overall_passing_math_reading_count = passing_math_reading_new["student_name"].count()
                  
          
          7-.Calculate the overall passing percentage with new total student count
                 overall_passing_percentage = overall_passing_math_reading_count / student_count_new *100
                 overall_passing_percentage
          
          8- Creeate a DataFrame and format the columns
                  district_summary_df = pd.DataFrame(
                           [{"Total Schools": school_count, 
                           "Total Students": student_count, 
                           "Total Budget": total_budget,
                           "Average Math Score": average_math_score, 
                           "Average Reading Score": average_reading_score,
                           "% Passing Math": passing_math_percentage,
                           "% Passing Reading": passing_reading_percentage,
                           "% Overall Passing": overall_passing_percentage}])



                  # Format the "Total Students" to have the comma for a thousands separator.
                  district_summary_df["Total Students"] = district_summary_df["Total Students"].map("{:,}".format)
                  # Format the "Total Budget" to have the comma for a thousands separator, a decimal separator and a "$".
                  district_summary_df["Total Budget"] = district_summary_df["Total Budget"].map("${:,.2f}".format)
                  # Format the columns.
                  district_summary_df["Average Math Score"] = district_summary_df["Average Math Score"].map("{:.1f}".format)
                  district_summary_df["Average Reading Score"] = district_summary_df["Average Reading Score"].map("{:.1f}".format)
                  district_summary_df["% Passing Math"] = district_summary_df["% Passing Math"].map("{:.1f}".format)
                  district_summary_df["% Passing Reading"] = district_summary_df["% Passing Reading"].map("{:.1f}".format)
                  district_summary_df["% Overall Passing"] = district_summary_df["% Overall Passing"].map("{:.1f}".format)

                  # Display the data frame
                  district_summary_df

 output.4
 ![image](https://user-images.githubusercontent.com/85843030/126577665-7e84b882-958b-4bba-9857-39e4ebb62e4d.png)

      
          
