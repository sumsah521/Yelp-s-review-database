# Questions & Answers, part 1

## Part 1: Profiling the Data
__1. Find the total number of records for each table:__

````sql
    SELECT *
    FROM [] --where [] is replaced by the desired table for each query
````
* Results
  * attribute = 10,000
  * business = 10,000
  * category = 10,000
  * checkin = 10,000
  * elite_years = 10,000
  * friend = 10,000
  * hours = 10,000
  * photo = 10,000
  * review = 10,000
  * tip = 10,000
  * user = 10,000

__2. Find the total distinct records for each table:__
````sql
    SELECT COUNT (DISTINCT {}) --where {} is replaced by desired primary or foreign key column name
    FROM [] --where [] is replaced by the desired table
````
* Results
  * attribute = 1,115
  * business = 10,000
  * category = 2,643
  * checkin = 493
  * elite_years = 2,780
  * friend = 11
  * hours = 1,562
  * photo = 10,000
  * review = 10,000
  * tip = 3,979
  * user = 10,000

__3. Are there any columns with null values in the Users table?__
````sql
    SELECT *
    FROM user
    WHERE id IS NULL
      OR name IS NULL
      OR review_count IS NULL
      OR yelping_since IS NULL
      OR useful IS NULL
      OR funny IS NULL
      OR cool IS NULL
      OR fans IS NULL
      OR average_stars IS NULL
      OR compliment_hot IS NULL
      OR compliment_more IS NULL
      OR compliment_profile IS NULL
      OR compliment_cute IS NULL
      OR compliment_list IS NULL
      OR compliment_note IS NULL
      OR compliment_plain IS NULL
      OR compliment_cool IS NULL
      OR compliment_funny IS NULL
      OR compliment_writer IS NULL
      OR compliment_photos IS NULL
````
* Results
  * (Zero rows) / No null values

__4. For each table and column listed below, display the smallest, largest, and average value for the following fields:__
````sql
    SELECT --replace "stars" with desired column 
      MIN(stars), 
      MAX(stars),
      AVG(stars)
    FROM review --replace "review" with desired table 
````
Table    | Column       | Minimum    | Maximum    | Average
-------- | ------------ |----------: | ---------: | ---------:
review   | stars        |1           | 5          |3.7082
business | stars        |1           | 5          |3.6549
tip      | likes        |0           | 2          |0.0144
checkin  | count        |1           | 53         |1.9414
user     | review_count |0           | 2000       |24.2995

<sup>*The above table was produced by running each corresponding table/column combination for min, max, and average seen in the query above*</sup>

__5. List the cities with the most reviews in descending order:__
````sql
    SELECT city,
      SUM(review_count) AS review_count
    FROM business
    GROUP BY city
    ORDER BY review_count DESC
````
city            | review_count 
--------------- | ------------:
Las Vegas       |        82854
Phoenix         |        34503
Toronto         |        24113
Scottsdale      |        20614
Charlotte       |        12523
Henderson       |        10871
Tempe           |        10504
Pittsburgh      |         9798
Montréal        |         9448
Chandler        |         8112
Mesa            |         6875
Gilbert         |         6380
Cleveland       |         5593
Madison         |         5265
Glendale        |         4406
Mississauga     |         3814
Edinburgh       |         2792
Peoria          |         2624
North Las Vegas |         2438
Markham         |         2352
Champaign       |         2029
Stuttgart       |         1849
Surprise        |         1520
Lakewood        |         1465
Goodyear        |         1155

<sup>*(Output limit exceeded, 25 of 362 total rows shown)*</sup>

__6. Find the distribution of star ratings to the business in the following cities:__
* Avon
````sql
    SELECT stars,
      COUNT (review_count) as reviews
    FROM business
    WHERE city = 'Avon'
    GROUP BY stars
````
stars | reviews 
:----: | -----:
1.5    |      1 
2.5    |      2 
3.5    |      3 
4.0    |      2 
4.5    |      1 
5.0    |      1 

* Beachwood
````sql
    SELECT stars,
      COUNT (review_count) as reviews
    FROM business
    WHERE city = 'Beachwood'
    GROUP BY stars
````
stars | reviews 
:----: | -----:
2.0    |      1
2.5    |      1
3.0    |      2
3.5    |      2
4.0    |      1
4.5    |      2
5.0    |      5

__7. Find the top 3 users based on their total number of reviews:__
````sql
    SELECT name,
      review_count
    FROM user
    ORDER BY review_count DESC
    LIMIT 3
````
name   | review_count 
------ | -----------:
Gerald |         2000 
Sara   |         1629 
Yuri   |         1339 

__8. Does posting more reviews correlate with more fans?__
````sql
    SELECT name,
      review_count AS most_reviews,
      fans
    FROM user
    ORDER BY review_count DESC
    LIMIT 5
````
name    | most_reviews | fans 
------- | -----------: | ---:
Gerald  |         2000 |  253
Sara    |         1629 |   50
Yuri    |         1339 |   76
.Hon    |         1246 |  101
William |         1215 |  126
````sql
    SELECT name,
      review_count,
      fans AS most_fans
    FROM user
    ORDER BY fans DESC
    LIMIT 5
````
name      | review_count | most_fans
--------- | -----------: | ---------:
Amy       |          609 |       503
Mimi      |          968 |       497
Harald    |         1153 |       311
Gerald    |         2000 |       253
Christine |          930 |       173

* Interpretation of results:
  * Posting more reviews does not, alone, correlate to more fans. For example, Gerald has posted the most reviews (2000) and has 253 fans, while Amy has only posted 609 reviews but has 503 fans, which is the most number of fans among all users. In fact, 4 of the 5 users with the most fans are not among the top 5 users with the most reviews.

__9. Are there more reviews with the word "love" or with the word "hate" in them?__
````sql
    SELECT 
      SUM(CASE WHEN text LIKE '%love%' THEN 1 ELSE 0 END) AS love_reviews,--counts reviews with love in text
      SUM(CASE WHEN text LIKE '%hate%' THEN 1 ELSE 0 END) AS hate_reviews--counts reviews with hate in text
    FROM review
````
love_reviews | hate_reviews 
-----------: | -----------:
1780         |          232

* Interpretation of results:
   * There are more reviews with the word "love" than reviews with the word "hate".
      * While this basic query does answer the question posed, such a comparison seems inadvisable as a basis for inferring postive or negative sentiment in a review. Case in point, the following code can be used to query reviews that have the word "love" in them yet received only a 1 or 2 star review. Below the code are a few examples of 1-star reviews included in the dataset to show why this type of comparison should not be used to analyze the actual sentiment of reviews. Instead, a proper sentiment analysis using more advanced computational methods than those available in SQLite is recommended to draw accurate linguistic-based conclusions from review text.
   ````sql
          SELECT
            stars,
            CASE WHEN text LIKE '%love%' THEN text ELSE 0 END AS love_reviews
          FROM review
          WHERE stars IN ('1', '2')
            AND love_reviews IS NOT 0
          ORDER BY stars
   ````
   * Some particularly egregious examples:
      1. "I __love__ supporting small business, but this is not one I would recommend giving your money to."
      2. "Seriously the worst experience in Vegas! I've been to vegas a ton and I've never had to make the decision to leave a resort. It smells like sewer, customer service is non-existent, bathrooms not restocked, broken refrigerator, terrible food and that's just the start. Elvis must be turning in his grave to know what is happening to his __beloved__ Las Vegas Hilton."
      3. "Went there for lunch yesterday. Discusting stench odor inside. Cooks cooking w/out __gloves__, and picking food off floor. Rice was greasy and over salted. NEVER AGAIN!" 
         * *Note that this example is returned because of the word gloves which does, technically speaking, meet the wildcard criteria of %love%.*

__10. Find the top 10 users with the most fans:__
````sql
    SELECT name,
      fans
    FROM user
    ORDER BY fans DESC
    LIMIT 10
````
name      | fans 
--------- | ----:
Amy       |  503
Mimi      |  497
Harald    |  311
Gerald    |  253
Christine |  173
Lisa      |  159
Cat       |  133
William   |  126
Fran      |  124
Lissa     |  120
