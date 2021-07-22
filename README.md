# School District Analysis
## 1. Purpose
      The purpose of this analysis is to create a school district summary and a school summary where one of the schools graders have been removed.
      The Thomas High School's results for math and reading for all the 9 graders have been set to Nan's.
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
  - 
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
                   passing_math_count_new= school_data_complete_df[(school_data_complete_df["math_score"] >= 70)&(school_data_complete_df["math_score"]!=np.nan)].count()                                                                                                                                                      ["student_name"]                                        passing_reading_count_new= school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)(school_data_complete_df["reading_score"]!=np.nan)].count()                                                                                                                                               ["student_name"]          
                  
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
                  
          
          9-.Calculate the overall passing percentage with new total student count
                 overall_passing_percentage = overall_passing_math_reading_count / student_count_new *100
                 overall_passing_percentage
          
          10- Creeate a DataFrame and format the columns
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

      
 #### 3.1.2 School Summary
 
             The following is given for the school analysis:
                  # Determine the School Type
                  per_school_types = school_data_df.set_index(["school_name"])["type"]

                  # Calculate the total student count.
                  per_school_counts = school_data_complete_df["school_name"].value_counts()

                  # Calculate the total school budget and per capita spending
                  per_school_budget = school_data_complete_df.groupby(["school_name"]).mean()["budget"]
                  # Calculate the per capita spending.
                  per_school_capita = per_school_budget / per_school_counts

                  # Calculate the average test scores.
                  per_school_math = school_data_complete_df.groupby(["school_name"]).mean()["math_score"]
                  per_school_reading = school_data_complete_df.groupby(["school_name"]).mean()["reading_score"]

                  # Calculate the passing scores by creating a filtered DataFrame.
                  per_school_passing_math = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)]
                  per_school_passing_reading = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)]

                  # Calculate the number of students passing math and passing reading by school.
                  per_school_passing_math = per_school_passing_math.groupby(["school_name"]).count()["student_name"]
                  per_school_passing_reading = per_school_passing_reading.groupby(["school_name"]).count()["student_name"]

                  # Calculate the percentage of passing math and reading scores per school.
                  per_school_passing_math = per_school_passing_math / per_school_counts * 100
                  per_school_passing_reading = per_school_passing_reading / per_school_counts * 100

                  # Calculate the students who passed both reading and math.
                  per_passing_math_reading = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)
                                                                 & (school_data_complete_df["math_score"] >= 70)]

                  # Calculate the number of students passing math and passing reading by school.
                  per_passing_math_reading = per_passing_math_reading.groupby(["school_name"]).count()["student_name"]

                  # Calculate the percentage of passing math and reading scores per school.
                  per_overall_passing_percentage = per_passing_math_reading / per_school_counts * 100
             
 
           Create a DataFrame for the above and format the DataFrame columns:
           
                 per_school_summary_df = pd.DataFrame({
                        "School Type": per_school_types,
                        "Total Students": per_school_counts,
                        "Total School Budget": per_school_budget,
                        "Per Student Budget": per_school_capita,
                        "Average Math Score": per_school_math,
                        "Average Reading Score": per_school_reading,
                        "% Passing Math": per_school_passing_math,
                        "% Passing Reading": per_school_passing_reading,
                        "% Overall Passing": per_overall_passing_percentage})

            # Format the Total School Budget and the Per Student Budget
                        per_school_summary_df["Total School Budget"] = per_school_summary_df["Total School Budget"].map("${:,.2f}".format)
                        per_school_summary_df["Per Student Budget"] = per_school_summary_df["Per Student Budget"].map("${:,.2f}".format)
                        per_school_summary_df.head(15)
           
output .5

![image](https://user-images.githubusercontent.com/85843030/126662100-eeacf662-eb54-4e25-b670-8da6c324c8d2.png)



         Analysis for Thomas High School
                  
           1. Get the number of 10th-12th graders from Thomas High School (THS).

                  tenth_graders_count= school_data_complete_df[(school_data_complete_df["school_name"] == "Thomas High School")
                                                            &(school_data_complete_df["grade"] == "10th")]["Student ID"].count()
                                         
                  eleventh_graders_count = school_data_complete_df[(school_data_complete_df["school_name"] == "Thomas High School")
                                                            &(school_data_complete_df["grade"] == "11th")]["Student ID"].count()


                  twelfth_graders_count= school_data_complete_df[(school_data_complete_df["school_name"] == "Thomas High School")
                                                            &(school_data_complete_df["grade"] == "12th")]["Student ID"].count()
                  
            2. THS is the Thomas High School Graders without the 9th graders
                  THS_total_graders=tenth_graders_count+eleventh_graders_count+twelfth_graders_count
                  
                                                                                                                                                                   
            3. Get all the students passing math from THS
                  THS_passing_math= school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School")
                                                                  & (school_data_complete_df["math_score"] >= 70)
                                                                  & (school_data_complete_df["math_score"]!=np.nan)]["Student ID"].count()

             4 . Get all the students passing reading from THS
                  THS_passing_reading= school_data_complete_df.loc[(school_data_complete_df["school_name"]== "Thomas High School")
                                                                  & (school_data_complete_df["reading_score"] >= 70)
                                                                  & (school_data_complete_df["reading_score"]!=np.nan)]["Student ID"].count()
                                                 
             5. Get all the students passing math and reading from THS
                  THS_passing_math_reading=school_data_complete_df.loc[(school_data_complete_df["school_name"]== "Thomas High School")
                                                                  &(school_data_complete_df["math_score"] >= 70)
                                                                  &(school_data_complete_df["reading_score"] >= 70)]["Student ID"].count()        
                                                    
                                                    
                                                    
             6. Calculate the percentage of 10th-12th grade students passing math from Thomas High School. 
                  THS_total_graders= tenth_graders_count + eleventh_graders_count + twelfth_graders_count                                                                                           THS_passing_math_percentage = THS_passing_math / THS_total_graders * 100                                                   
                                                    
                                                    
              7. Calculate the percentage of 10th-12th grade students passing reading from Thomas High School.
                  THS_passing_reading_percentage = THS_passing_reading / THS_total_graders * 100                                                   
                                                    
                                                    
              8. Calculate the overall passing percentage of 10th-12th grade from Thomas High School. 
                  THS_passing_math_reading_percentage = THS_passing_math_reading / THS_total_graders * 100 
                                                                                                    
             9 . Replace the passing math percent for Thomas High School in the per_school_summary_df.
                  per_school_summary_df.loc["Thomas High School", "% Passing Math"] = THS_passing_math_percentage                           
                  per_school_summary_df
    
 output .6
![image](https://user-images.githubusercontent.com/85843030/126689828-f352fb68-6523-4023-9171-9a3745493644.png)


   #### 3.1.3 High and Low Performing Schools
              The five top and five bottom performing schools are sorted on their overall percentage pass.
              
              # Sort and show top five performing schools.
                  top_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending=False)
                  top_schools.head()
              
                  
  output .7 
  ![image](https://user-images.githubusercontent.com/85843030/126663197-b61e6998-1786-4eef-bdbf-3d1e599fde31.png)

            # Sort and show top five worst performing schools.
            bottom_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending=True)
            bottom_schools.head()


output .8
![image](https://user-images.githubusercontent.com/85843030/126663578-303c427a-f6d4-4e8f-9d78-d10f5ad16837.png)

#### 3.1.4 Math and Reading Scores by Grade
            The following is given:
                  # Create a Series of scores by grade levels using conditionals.

                        ninth_graders=school_data_complete_df[(school_data_complete_df["grade"] == "9th")]
                        tenth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "10th")]
                        eleventh_graders = school_data_complete_df[(school_data_complete_df["grade"] == "11th")]
                        twelfth_graders = school_data_complete_df[(school_data_complete_df["grade"] == "12th")]

                        # Group each school Series by the school name for the average math score.

                        ninth_grade_math_scores = ninth_graders.groupby(["school_name"]).mean()["math_score"]

                        tenth_grade_math_scores = tenth_graders.groupby(["school_name"]).mean()["math_score"]

                        eleventh_grade_math_scores = eleventh_graders.groupby(["school_name"]).mean()["math_score"]

                        twelfth_grade_math_scores = twelfth_graders.groupby(["school_name"]).mean()["math_score"]



                  # Group each school Series by the school name for the average reading score.
                        ninth_grade_reading_scores = ninth_graders.groupby(["school_name"]).mean()["reading_score"]

                        tenth_grade_reading_scores = tenth_graders.groupby(["school_name"]).mean()["reading_score"]

                        eleventh_grade_reading_scores = eleventh_graders.groupby(["school_name"]).mean()["reading_score"]

                        twelfth_grade_reading_scores = twelfth_graders.groupby(["school_name"]).mean()["reading_score"]


            1. Combine each Series for average math scores by school into single data frame.

                        math_scores_by_grade = pd.DataFrame({
                                       "9th": ninth_grade_math_scores,
                                       "10th": tenth_grade_math_scores,
                                       "11th": eleventh_grade_math_scores,
                                       "12th": twelfth_grade_math_scores})

                        math_scores_by_grade.head(15)
                        





            2. Combine each Series for average reading scores by school into single data frame.
                        reading_scores_by_grade = pd.DataFrame({
                                          "9th": ninth_grade_reading_scores,
                                            "10th": tenth_grade_reading_scores,
                                            "11th": eleventh_grade_reading_scores,
                                            "12th": twelfth_grade_reading_scores})

                        reading_scores_by_grade.head(15)
                    

 


              3. Format each grade column and order the columns from 9th grade to 12th grade
                    # Format each grade column.

                              math_scores_by_grade["9th"] = math_scores_by_grade["9th"].map("{:.1f}".format)

                              math_scores_by_grade["10th"] = math_scores_by_grade["10th"].map("{:.1f}".format)

                              math_scores_by_grade["11th"] = math_scores_by_grade["11th"].map("{:.1f}".format)

                              math_scores_by_grade["12th"] = math_scores_by_grade["12th"].map("{:.1f}".format)

                              reading_scores_by_grade["9th"] = reading_scores_by_grade["9th"].map("{:,.1f}".format)

                              reading_scores_by_grade["10th"] = reading_scores_by_grade["10th"].map("{:,.1f}".format)

                              reading_scores_by_grade["11th"] = reading_scores_by_grade["11th"].map("{:,.1f}".format)

                              reading_scores_by_grade["12th"] = reading_scores_by_grade["12th"].map("{:,.1f}".format)

                     # Make sure the columns are in the correct order.

                              math_scores_by_grade = math_scores_by_grade[["9th", "10th", "11th", "12th"]]
                              reading_scores_by_grade = reading_scores_by_grade[["9th", "10th", "11th", "12th"]]

                     # Remove the index.
                              math_scores_by_grade.index.name = None
                              reading_scores_by_grade.index.name = None

                     # Display the data frame

                              math_scores_by_grade.head(15)
             
output. 9 
![image](https://user-images.githubusercontent.com/85843030/126669004-ffebfc4a-a233-456a-b15f-134d3fd23ee3.png)



                     # Display the data frame
                              reading_scores_by_grade.head(15)
                              
output. 10
![image](https://user-images.githubusercontent.com/85843030/126669122-8c6fc027-7e87-4ad9-b30c-0bd2661ebcd2.png)



  #### 3.1.5 Scores by School Spending
           The school spending is calculated here, where we define some ranges, and categorize the schools into small, medium and large
           We have already calculated per schools capita spending in the school summary section by dividing the school budget/tot number of schools
           per_school_capita = per_school_budget / per_school_counts
           
 output. 11 
 ![image](https://user-images.githubusercontent.com/85843030/126670693-068d75c9-d707-4472-81f0-656c87ad4e51.png)
 

           
           We want to get the statistic value for the spending so we can get an idea of the ranges.
           #get the stats for the spending
           per_school_capita.describe() 
           
           
  output. 12
  ![image](https://user-images.githubusercontent.com/85843030/126670985-943507e7-aa44-4a80-8056-5e63e5379383.png)

             
            # Establish the spending bins and group names.
                        spending_bins = [0, 585, 630, 645, 675]
                        group_names = ["<$584", "$585-629", "$630-644", "$645-675"]          

            # Categorize spending based on the bins.
                        per_school_summary_df["Spending Ranges (Per Student)"] = pd.cut(per_school_capita, spending_bins, labels=group_names)
                        per_school_summary_df.head()


output. 13
![image](https://user-images.githubusercontent.com/85843030/126672994-4120178e-9c52-4e0e-bb44-3c3c00243e54.png)


            # Calculate the average math, reading, and overall percentage pass for each school for the 4 bins.
            
                        spending_math_scores = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Math Score"]

                        spending_reading_scores = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["Average Reading Score"]

                        spending_passing_math = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Math"]

                        spending_passing_reading = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Passing Reading"]

                        overall_passing_spending = per_school_summary_df.groupby(["Spending Ranges (Per Student)"]).mean()["% Overall Passing"]        

            # Create the DataFrame
            spending_summary_df = pd.DataFrame({
                      "Average Math Score" : spending_math_scores,
                      "Average Reading Score": spending_reading_scores,
                      "% Passing Math": spending_passing_math,
                      "% Passing Reading": spending_passing_reading,
                      "% Overall Passing": overall_passing_spending})

            # Format the DataFrame 
                        spending_summary_df["Average Math Score"] = spending_summary_df["Average Math Score"].map("{:.1f}".format)

                        spending_summary_df["Average Reading Score"] = spending_summary_df["Average Reading Score"].map("{:.1f}".format)

                        spending_summary_df["% Passing Math"] = spending_summary_df["% Passing Math"].map("{:.0f}".format)

                        spending_summary_df["% Passing Reading"] = spending_summary_df["% Passing Reading"].map("{:.0f}".format)

                        spending_summary_df["% Overall Passing"] = spending_summary_df["% Overall Passing"].map("{:.0f}".format)

                  spending_summary_df


output. 14
![image](https://user-images.githubusercontent.com/85843030/126673977-7ba8ed12-48b3-4d71-bfa1-0983637bf44b.png)

        
#### 3.1.6 Scores by School Size
            Group the scores by school size, where the ranges of small, medium, and large are chosen depending on the school sizes in
            total student column in output. 5.
            
            
                  # Establish the bins.
                  per_school_counts
                  size_bins = [0, 1000, 2000, 5000]
                  group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
                  # Categorize spending based on the bins.
                  per_school_summary_df["School Size"] = pd.cut(per_school_summary_df["Total Students"], size_bins, labels=group_names)

                  per_school_summary_df.head(15)
          
                  
output.15 
![image](https://user-images.githubusercontent.com/85843030/126675196-d8cfff5e-732b-4e31-a810-25305dd3d422.png)


                  The average math, reading scores and the percentage passing grades are calculated and shown against the school size
                  
                  # Calculate averages for the desired columns. 

                        size_math_scores = per_school_summary_df.groupby(["School Size"]).mean()["Average Math Score"]

                        size_reading_scores = per_school_summary_df.groupby(["School Size"]).mean()["Average Reading Score"]

                        size_passing_math = per_school_summary_df.groupby(["School Size"]).mean()["% Passing Math"]

                        size_passing_reading = per_school_summary_df.groupby(["School Size"]).mean()["% Passing Reading"]

                        size_overall_passing = per_school_summary_df.groupby(["School Size"]).mean()["% Overall Passing"]

                        # Assemble into DataFrame. 
                        size_summary_df = pd.DataFrame({
                                  "Average Math Score" : size_math_scores,
                                  "Average Reading Score": size_reading_scores,
                                  "% Passing Math": size_passing_math,
                                  "% Passing Reading": size_passing_reading,
                                  "% Overall Passing": size_overall_passing})

                                  # Format the DataFrame  
                        size_summary_df["Average Math Score"] = size_summary_df["Average Math Score"].map("{:.1f}".format)

                        size_summary_df["Average Reading Score"] = size_summary_df["Average Reading Score"].map("{:.1f}".format)

                        size_summary_df["% Passing Math"] = size_summary_df["% Passing Math"].map("{:.0f}".format)

                        size_summary_df["% Passing Reading"] = size_summary_df["% Passing Reading"].map("{:.0f}".format)

                        size_summary_df["% Overall Passing"] = size_summary_df["% Overall Passing"].map("{:.0f}".format)

                        size_summary_df
                        
output. 16
![image](https://user-images.githubusercontent.com/85843030/126675881-c1691a83-a494-4316-a65c-43b7c2e7d367.png)


   #### 3.1.7 Scores by School Type
            Divide the school types into district and charter to see if there are any significant differences in academic performance.
            
                  # Calculate averages for the desired columns. 
                        type_math_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Math Score"]

                        type_reading_scores = per_school_summary_df.groupby(["School Type"]).mean()["Average Reading Score"]

                        type_passing_math = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Math"]

                        type_passing_reading = per_school_summary_df.groupby(["School Type"]).mean()["% Passing Reading"]

                        type_overall_passing = per_school_summary_df.groupby(["School Type"]).mean()["% Overall Passing"]     

                  # Assemble into DataFrame. 
                        type_summary_df = pd.DataFrame({
                                  "Average Math Score" : type_math_scores,
                                  "Average Reading Score": type_reading_scores,
                                  "% Passing Math": type_passing_math,
                                  "% Passing Reading": type_passing_reading,
                                  "% Overall Passing": type_overall_passing})
                                  
                   # Format the DataFrame 
                        type_summary_df["Average Math Score"] = type_summary_df["Average Math Score"].map("{:.1f}".format)

                        type_summary_df["Average Reading Score"] = type_summary_df["Average Reading Score"].map("{:.1f}".format)

                        type_summary_df["% Passing Math"] = type_summary_df["% Passing Math"].map("{:.0f}".format)

                        type_summary_df["% Passing Reading"] = type_summary_df["% Passing Reading"].map("{:.0f}".format)

                        type_summary_df["% Overall Passing"] = type_summary_df["% Overall Passing"].map("{:.0f}".format)

                        type_summary_df                                  
                                  

                    
  output .17
  ![image](https://user-images.githubusercontent.com/85843030/126676576-15da310d-9cb9-45da-b842-9e27caba4eda.png)

            
 ## 4 Results and comaprisons
  ### 4.1 Effect on district summary:
        After replacing the 9th grader student's grades with a "NaN', it can bee seen (comparing output 4 and 18)that the effect on the overall percentage pass
        and grades are insignificant
            
            
  output. 18 : original district_summary_df
  ![image](https://user-images.githubusercontent.com/85843030/126686498-3392b253-df9d-4109-8fe6-41e2aec030ae.png)

           
  ### 4.2 Effetc on school summary
        Comparing output.5 and output 6, we can see that in output.5, where the percentage passing rates have been calculated inclusive of the 
        'NaN' values for the Thomas High School, the rates are much lower, due to the fact that the percentage calculations for the 
        Thomas High School take into account all grades, hence the exsistance of the 9th graders with no marks, will bring the scores down.
        In output .6, the percentage score calculations have only taken into account the students for the 10th-12th graders, hence the calculated
        scores will be much higher without any blank values bringing the scores down.
        
### 4.3 Effect on Math and Reading Scores by Grade
        
        Removing the 9th gradres from the scores by grades, will have no effect on the table itself, apart from where the "9th" column is visible
        for the Thomas High School, it will show a "nan" value as spected.
        
        
### 4.3 Effect on Scores by School Spending

      There is no affect on this criteria, because in our analysis we have already replaced the Thomas High School scores with the scores that
      were calculated using only 10-12 graders results.
      It is also worth noting that the total students at the Thomas High School is still shown as 1635. Despite the fact that we have used 
      1635(tot)-461(9th graders) =1174 for the anaysis were the 9th graders have been omitted.
   
 ### 4.4 Effect by School Size & School Type
      No effect on adding the 'nan's" to Thomas High School 9 graders as mentioned above. The math, reading scores have already taken into account
      the non values of the 9 graders previously, and therefore any additional analysis here will nopt directly affect the results.
  

