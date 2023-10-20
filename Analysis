## Part 2: Analyses and Inferences

### Section 1: High vs Mid-range Ratings
#### Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.
For this section of the analysis, restaurants in the city of Phoenix were chosen as the criteria to be analyzed. From there, additional data points are needed including hours, stars, review count, neighborhood, and postal code to perform the requested analyses. An initial query provides a basic overview of this information.
````sql
    SELECT b.name,
      h.hours,
      b.stars,
      b.review_count,
      b.neighborhood,
      b.postal_code
    FROM business b
      LEFT JOIN category c ON c.business_id = b.id
      LEFT JOIN hours h ON h.business_id = c.business_id
    WHERE b.city = 'Phoenix'  
      AND c.category = 'Restaurants'
    GROUP BY b.name
````
name                                   | hours                 | stars | review_count | neighborhood | postal_code 
-------------------------------------- | --------------------- | :---: | -----------: | ------------ | ----------:
Bootleggers Modern American Smokehouse | Saturday\|11:00-22:00 |   4.0 |          431 |              | 85028       
Charlie D's Catfish & Chicken          | Saturday\|11:00-18:00 |   4.5 |            7 |              | 85034       
Five Guys                              | Saturday\|10:00-22:00 |   3.5 |           63 |              | 85008       
Gallagher's                            | Saturday\|9:00-2:00   |   3.0 |           60 |              | 85024       
Matt's Big Breakfast                   | None                  |   4.0 |          188 |              | 85016       
McDonald's                             | Saturday\|5:00-0:00   |   2.0 |            8 |              | 85004       

Because none of the rows in the selected dataset include values in the neighborhood category, this column can be ignored. Postal code will be the chosen variable looked at for Section 1, part iii. 
	
However, Matt's Big Breakfast is also missing values in the hours tab. While one solution would be to filter out hours that are NULL, effectively dropping that business from the analysis, Matt's Big Breakfast accounts for one-sixth (16.67%) of the dataset. Dropping 16.67% of the dataset would likely significantly impact any results. Additionally, dropping the NULL value rows conditionally in just the hours query, but not in the following queries, could also skew the results. Had the dataset included many more businesses within the selected criteria where each business accounted for a smaller percentage of the total dataset, dropping may have been an option. 
	
Instead, a Google search shows that Matt's Big Breakfast, located in postal code 85016 is open between the hours of 7am-2pm, 7 days a week.	A potential drawback of this method concerns recency of data. While the provided Yelp dataset is static and has an unknown origin date, the Google search for Matt's Big Breakfast shows hours for January 2023. Therefore, several captures of the website for Matt's Big Breakfast using random dates in 2017, 2019, and 2021 were extracted using the [Wayback Machine](https://archive.org/web). This additional research shows very little fluctuation in operating hours over the years, thus the assumption will be made that the current 2023 operating hours are a fair representation for the sake of this analysis.
	
Ideally, a temporary table, phoenix_restaurants, would be created that could be referenced for the remainder of the exercise. Then, the hours for Matt's Big Breakfast would be updated in the phoenix_restaurants table using an INSERT INTO clause. However, the interactive code block supplied in this course for the exercise did not allow writing to the database, even using a temporary clause such as CREATE TEMP TABLE or CREATE VIEW. Therefore Section 1, part i will be analyzed with the additional information accounted for manually. 

__Section 1, part i: Do the two groups you chose to analyze have a different distribution of hours?__

Comparing the two groups⁠—those with a 4-5 star overall review and those with a 2-3 star overall review⁠—shows a slight difference regarding weekly operational hours. In both groups, all businesses were open 7 days a week but had varying total operational hours. 
		
First, a comparison was performed between the restaurants that scored in the 4-5 star overall review category. Charlie D's was open from 11am-6pm Monday through Saturday, and open only 3 hours on Sundays resulting in a total of 45 hours per week. Similarly, Matt's Big Breakfast, as discussed in the section above, operates 7 hours per day, 7 days a week or 49 hours per week. Alternately, Bootleggers operated 77 hours per week, between 11am-10pm every day. 
````sql
    SELECT b.name,
      h.hours
    FROM business b
      LEFT JOIN category c ON c.business_id = b.id
      LEFT JOIN hours h ON h.business_id = c.business_id
    WHERE b.city = 'Phoenix'
      AND c.category = 'Restaurants'
      AND b.stars BETWEEN '4.0' AND '5.0'
    ORDER BY b.stars DESC
    --Do not GROUP BY b.name, otherwise full operating hours will not be shown
````
<details>
<summary>Table with hours for 4-5 star restaurants </summary>
  
name                                     | hours                 
---------------------------------------- | ----------------------
| Charlie D's Catfish & Chicken          | Monday\|11:00-18:00    
| Charlie D's Catfish & Chicken          | Tuesday\|11:00-18:00   
| Charlie D's Catfish & Chicken          | Friday\|11:00-18:00    
| Charlie D's Catfish & Chicken          | Wednesday\|11:00-18:00 
| Charlie D's Catfish & Chicken          | Thursday\|11:00-18:00  
| Charlie D's Catfish & Chicken          | Sunday\|13:00-16:00    
| Charlie D's Catfish & Chicken          | Saturday\|11:00-18:00  
| Matt's Big Breakfast                   | None                  
| Bootleggers Modern American Smokehouse | Monday\|11:00-22:00    
| Bootleggers Modern American Smokehouse | Tuesday\|11:00-22:00   
| Bootleggers Modern American Smokehouse | Friday\|11:00-22:00    
| Bootleggers Modern American Smokehouse | Wednesday\|11:00-22:00 
| Bootleggers Modern American Smokehouse | Thursday\|11:00-22:00  
| Bootleggers Modern American Smokehouse | Sunday\|11:00-22:00    
| Bootleggers Modern American Smokehouse | Saturday\|11:00-22:00  
</details>
  
Next, businesses with overall ratings between 2-3 were examined. Five Guys operated on a similar schedule to Bootleggers but opened an hour earlier, thus operating 12 hours a day, 7 days a week or 84 hours each week. Gallagher's had the most varied hours, open 3 days a week from 11am-midnight (or 13 hours a day), 2 days a week from 11-2am (15 hours a day), Saturdays from 9am-2am (17 hours), and Sundays from 9am-midnight (15 hours) making their total weekly operating hours 101 hours. Finally, McDonald's began their hours earliest, opening at 5am each day and were open until 11pm 5 days a week (18 hours a day) and open until midnight twice a week (19 hours a day) bringing their total operating hours to 128 hours a week.
````sql
    SELECT b.name,
      h.hours
    FROM business b
      LEFT JOIN category c ON c.business_id = b.id
      LEFT JOIN hours h ON h.business_id = c.business_id
    WHERE b.city = 'Phoenix'
      AND c.category = 'Restaurants'
      AND b.stars BETWEEN '2.0' AND '3.9'
    ORDER BY b.stars DESC
    --Do not GROUP BY b.name, otherwise full operating hours will not be shown
````
 <details>
<summary>Table with hours for 2-3 star restaurants</summary>
 
 name        | hours                 
------------ | ----------------------
 Five Guys   | Monday\|10:00-22:00    
 Five Guys   | Tuesday\|10:00-22:00   
 Five Guys   | Friday\|10:00-22:00    
 Five Guys   | Wednesday\|10:00-22:00 
 Five Guys   | Thursday\|10:00-22:00  
 Five Guys   | Sunday\|10:00-22:00    
 Five Guys   | Saturday\|10:00-22:00  
 Gallagher's | Monday\|11:00-0:00     
 Gallagher's | Tuesday\|11:00-0:00    
 Gallagher's | Friday\|11:00-2:00     
 Gallagher's | Wednesday\|11:00-0:00  
 Gallagher's | Thursday\|11:00-2:00   
 Gallagher's | Sunday\|9:00-0:00      
 Gallagher's | Saturday\|9:00-2:00    
 McDonald's  | Monday\|5:00-23:00     
 McDonald's  | Tuesday\|5:00-23:00    
 McDonald's  | Friday\|5:00-0:00      
 McDonald's  | Wednesday\|5:00-23:00  
 McDonald's  | Thursday\|5:00-23:00   
 McDonald's  | Sunday\|5:00-23:00     
 McDonald's  | Saturday\|5:00-0:00    
</details>
  
Looking at these values, a possible correlation may exist between how many hours a business in Phoenix is open per day or week and their overall rating. In this analysis, the businesses that were open a fewer number of hours each day rated higher than those that were open longer. However, with only 6 businesses to compare, the sample size is too small to draw any significant conclusions.

__Section 1, part ii: Do the two groups you chose to analyze have a different number of reviews?__
````sql
  SELECT SUM(CASE WHEN b.stars BETWEEN '4.0' and '5.0' THEN b.review_count ELSE 0 END) AS '4-5 star rating',
    SUM(CASE WHEN b.stars BETWEEN '2.0' and '3.9' THEN b.review_count ELSE 0 END) AS '2-3 star rating'
  FROM business b
    LEFT JOIN category c ON c.business_id = b.id
  WHERE b.city = 'Phoenix'
    AND c.category = 'Restaurants'
 ````
4-5 star rating | 2-3 star rating 
--------------: | --------------:
626             | 131 

As seen in the output above, there are far more reviews for restaurants that have 4-5 star ratings than those than have 2-3 star ratings.

Data validation can be performed to ensure we receive the correct values by using the following code and tallying total review counts per group by hand:
````sql
    SELECT DISTINCT b.name,
      b.stars,
      b.review_count
    FROM business b
      LEFT JOIN category c ON c.business_id = b.id
      LEFT JOIN hours h ON h.business_id = c.business_id
    WHERE b.city = 'Phoenix'
      AND c.category = 'Restaurants'
    ORDER BY b.stars DESC
````
name                                   | stars | review_count 
-------------------------------------- | ----- | ------------:
Charlie D's Catfish & Chicken          |   4.5 |            7 
Matt's Big Breakfast                   |   4.0 |          188 
Bootleggers Modern American Smokehouse |   4.0 |          431 
Five Guys                              |   3.5 |           63 
Gallagher's                            |   3.0 |           60 
McDonald's                             |   2.0 |            8 

__Section 1, part iii. Are you able to infer anything from the location data provided between these two groups? Explain.__

No. As discussed previously, the sample size of only six restaurants is far too small to make any robust or statistically sound inferences. 
		
Even if such limitations were to be disregarded, little information can be inferred from the included data. No two restaurants share the same postal code and the use of a map, such as the one provided on [Phoenix.org](https://www.phoenix.org/maps/zip-code-map/) to provide the relational location of each restaurant in the table, reveals no additional correlations between relative location in Phoenix and star rating. Even if such correlation was present, this could be accounted for by spatial correlation (that is, two data points that are near each other may be correlated simply by way of being in close proximity to each other) rather than meaningful differences in star rating. In the case of this analysis, spatial correlation could mean that the same people frequent multiple restaurants in the area, meaning we measure those customers' tastes in food rather than variation of quality or service in the restaurants.

### Section 2: Open vs Closed

#### Group business based on the ones that are open and the ones that are closed. List two differences between the ones that are still open and the ones that are closed.
````sql
  SELECT
    CASE WHEN b.is_open IS 1 THEN 'Yes' ELSE 'No' END AS 'Is Open?',--rename boolean integers to 'yes' or 'no' values
    COUNT(b.is_open) AS '# of Businesses',
    ROUND(AVG(b.stars),1) AS 'Average Star Rating',
    SUM(b.review_count) AS 'Total Reviews'
  FROM business b
  GROUP BY b.is_open
````
Is Open? | # of Businesses | Average Star Rating | Total Reviews 
-------- | --------------: | ------------------: | ------------:
No       |            1520 |                 3.5 |         35261 |
Yes      |            8480 |                 3.7 |        269300 |

* Differences
  * The first difference noted is that there are far more businesses that are open in the dataset than ones that are closed.
  * Businesses that are open have received far more reviews than those that are closed.

### Section 3: Choose a type of analysis to conduct on the Yelp dataset and prepare the data for analysis.

__Section 3, part i: Indicate type of analysis.__

For this analysis, a comparison will be drawn among popular cuisine types to determine what type of cuisine is the most prevalent in the United States and if prevalence is synonymous with most popular and/or best rated.

__Section 3, part ii: Write 1-2 brief paragraphs on the type of data needed and why it's needed.__

For this analysis, the data needed at the most basic level will be category, stars, reviews, postal code, and whether a business is still open or not. However, to properly query the dataset to get the information needed for the analysis, some additional filtering and aggregate functions need to be performed.

Cuisine type will be filtered for the following basic cuisines: Traditional American, Chinese, Mexican, Italian, Indian, Japanese, Greek, French, and Irish. 

Next, because this analysis will only be looking at cuisine within the United States, a filter is added to limit postal code to 5 characters since the US uses 5-digit zip codes. To ensure that limiting the postal code length to 5 characters does filter out any locations not in the US, a data validation query is run.
````sql
    SELECT b.state,
      b.postal_code
    FROM category c
      JOIN business b ON c.business_id = b.id
    WHERE c.category IN ('Chinese', 'Japanese', 'Indian', 'Mexican', 'Italian', 
        'Irish', 'American (Traditional)', 'French', 'Greek')
      AND LENGTH(b.postal_code) = 5
    ORDER BY state
````

<details>
<summary>State & Postal Code Table</summary>

state | postal_code
----- | -----------
AZ    | 85028      
AZ    | 85034      
AZ    | 85340      
AZ    | 85024      
AZ    | 85268      
AZ    | 85204      
AZ    | 85225      
AZ    | 85353      
IL    | 61820      
NC    | 28215      
NC    | 28215      
NV    | 89146      
NV    | 89134      
NV    | 89169      
OH    | 44114      
OH    | 44202      
OH    | 44026      
OH    | 44256      
OH    | 44054      
PA    | 15108      
PA    | 15220      
WI    | 53711      
WI    | 53562      
</details>

Then, because this analysis will look at prevalence vs popularity, a number of aggregate functions will be performed to count the number of businesses in each category, sorted into closed vs open status as well as determining the avergage star rating per category, and reviews both total and average per each business per category.

__Section 3, part iii: Code and output of prepared dataset.__
````sql
    SELECT c.category AS 'Category',
      CASE WHEN b.is_open IS 1 THEN 'Yes' ELSE 'No' END AS 'Is Open?',
      COUNT(c.category) AS '# of Businesses',
      SUM(b.review_count) AS 'Total Reviews',
      ROUND(AVG(b.review_count),0) AS 'Average # of Reviews',
      ROUND(AVG(b.stars),1) AS 'Average Star Rating'
    FROM category c
      JOIN business b ON c.business_id = b.id
    WHERE c.category IN ('Chinese', 'Japanese', 'Indian', 'Mexican', 'Italian', 
        'Irish', 'American (Traditional)', 'French', 'Greek')
      AND LENGTH (postal_code) = 5
    GROUP BY b.is_open,
      c.category
    ORDER BY b.is_open DESC,
      SUM(b.review_count) DESC
````

| Category               | Is Open? | # of Businesses | Total Reviews | Average # of Reviews | Average Star Rating 
------------------------ | -------- | --------------: | ------------: | -------------------: | ------------------:
| American (Traditional) | Yes      |               8 |          1114 |                  139 |                 3.8 
| Chinese                | Yes      |               2 |           789 |                  395 |                 3.8 
| Mexican                | Yes      |               3 |           209 |                   70 |                 3.5 
| Indian                 | Yes      |               1 |            32 |                   32 |                 3.5 
| French                 | No       |               1 |           168 |                  168 |                 4.0 
| Irish                  | No       |               1 |           141 |                  141 |                 3.0 
| Italian                | No       |               1 |           129 |                  129 |                 4.0 
| American (Traditional) | No       |               3 |            14 |                    5 |                 3.8 
| Greek                  | No       |               1 |             4 |                    4 |                 5.0 
| Mexican                | No       |               1 |             4 |                    4 |                 2.0 
| Japanese               | No       |               1 |             3 |                    3 |                 4.5 
