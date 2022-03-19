# School District Analysis
## Overview of the school district analysis: 
Maria asked me to do an analysis of math and reading score district for the School Board. The Board asked for a district level summary of the data, a summary of the data per school, the top and bottom peroforming schools, math and reading scores by grade, scores by spending per student, scores by school size, and scores by school type. After performing the analysis, there was evidence of academic dishonesty as Thomas High School. It seems that the math and reading scores for the 9th graders had been altered. Due to the lack of knowledge around the incident, Maria asked me to replace the math and reading scores for 9th graders at Thomas High School with NaNs and redo the analysis. We decided to do that rather fill them all with zeroes becasue the functions in pandas will ignore the NaN values but would count the zeroes in the analysis and skew the results.

## Results:
### District Summary

![OG District Summary](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/original_district_summary_df.png)
|:--:|
| <b>Original District Summary</b>|

![Updated District Summary](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/updated_district_summary_df.png)
|:--:|
| <b>Updated District Summary</b>|

Surprisingly, the district summary didn't change very much. The differences in the average math scores, % passing math, % passing reading, and % overall passing are all within .3 points and the average reading score didn't change at all. 

### School Summary

The school summary was affected more than the district summary. I recalculated the averages and passing percentages for Thomas High School to reflect the change in the data. In the first iteration of the dataframe, the scores are very low because it is based on all of the students at Thomas High School even though the 9th graders don't have any scores. To adjust, I performed the calculations again using the total number of 10th-12th graders at Thomas High School to properly reflect the schools' performance. Luckily, I was able to use .loc function and .count() function in pandas to get that number:

    # syntax breakdown: named the count = df.loc[(df[[column1] <conditional operator> condition 1 <logical operator> df[column2] <conditional operator> condition2].count() - this let us get the count of the rows that met those two conditions
    not_ninth_count = school_data_complete_df.loc[(school_data_complete_df['school_name'] == 'Thomas High School') & (school_data_complete_df['grade'] != '9th'),'Student ID'].count()


None of the other schools are affected by this so I did the analysis the same way I did orginally to get the per school summary dataframe and then repalced the Thomas School metrics with the updated calculations to account for the change in the data we had to make.  I was able to replace those scores using the .loc function again. Below is the code I used to replace the passing percentage for math scores for Thomas High School in the Per School Summary Dataframe:

    # syntax breakdown: df.loc["Index/Row", "Column"] = new value
    per_school_summary_df.loc["Thomas High School", "% Passing Math"] = Thomas_passing_math_percentage

I refactored that code to do the same for the new calculations for the reading passing percentages and overall passing percentages. None of the other schools are affected so I did the analysis the same way I did orginally and then repalced the Thomas School metrics with the updated calculations to account for the change in the data we had to make. 

![OG School Summary](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/original_school_sumary_df.png)
|:--:|
| <b>Per School Summary - Before Updated Calculations</b>|

![Updated School Summary](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/updated_school_sumary_df.png)
|:--:|
| <b>Per School Summary - After Updated Calculations</b>|

Comparing the tables above, you can see that before making the updates, it seemed that Thomas High School was not performing very well in comparison to the other schools. After the update, however, you can see that the school is actually performing very well! However, the other metrics were also affected by the replacement of these scores. 

### Other Metric Breakdowns

- Math and Reading Scores by Grade

In the tables below, you can see that "nan" is the value for the 9th graders at Thomas High School. That occurs because there arent any values to perform the average on. This also shows that the change in the data didn't affect any of the other numbers in these tables.

Original Math Scores       |  Updated Math Scores
:-------------------------:|:-------------------------:
![OG Math Scores](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/og_math_scores_by_grade.png)|![OG Math Scores](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/updated_math_scores_by_grade.png)

Original Reading Scores    |  Updated Reading Scores
:-------------------------:|:-------------------------:
![OG Reading Scores](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/og_reading_scores_by_grade.png)|![OG Reading Scores](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/updated_reading_scores_by_grade.png)

- Scores by School Spending

Due to the large volume of data, the affect of removing the scores for one grade in one school is hard to see when looking at the data for all of the schools. Here, the only metric affected by the change is in the $630-$644 bin becuase that is where Thomas High School is located. 

![OG by Spending](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/og_avgs_by_spending.png)
|:--:|
| <b>Original Scores by Spending per Student</b>|

![Updated by Spending](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/updated_avgs_by_spending.png)
|:--:|
| <b>Updated Scores by Spending per Student</b>|

As you can see, for that bin the average scores for math and reading increased within one point and the percentages increased by two or three percent.

- Scores by School Size and School Type

The scores by school size and by schoool type actually change after we updated the data. Due to the large amount of data and the smaller amount of groups, the changed data has less of an affect on the results.

![OG by Size](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/og_avgs_by_size.png)
|:--:|
| <b>Original Scores by School Size</b>|

![Updated by Size](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/updated_avgs_by_size.png)
|:--:|
| <b>Updated Scores by School Size</b>|

![OG by Type](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/og_avgs_by_type.png)
|:--:|
| <b>Original Scores by School Type</b>|

![Updated by Type](https://github.com/rmward17/School_District_Analysis/blob/main/Resources/updated_avgs_by_type.png)
|:--:|
| <b>Updated Scores by School Type</b>|

## Summary: 
As we can see across all of the metrics, the impact of the academic dishonesty was very small on the school district data. The changes are evident in the district summary averages and percentages, math scores by grade, reading scores by grade, and the scores by spending. While these changes are within what seems to be a small range, it is up to the school board on what they deem is acceptable. It is a good sign though that the changes were small because it means the impact of the academic dishonesty wasn't detrimental to decsions that may have been made based on the original analysis. This could have had a more drastic impact if we had to adjust 9th grade scores at multiple schools or multiple grades within Thomas High School. We may want to do a high level analysis on just the 9th grade scores in the district to have a better understanding of the impact of the changed data. Hopefully the school board gets to the bottom of what happened and we can updae the data to include those accurate scores.

