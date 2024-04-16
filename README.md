# Apple-app-store-insights

## Table of content

- [Project overview](#project-overview)

- [Data sources](#data-sources)

- [Tools](#tools)

- [Data Cleaning and Preparation](#data-cleaning-and-preparation)

- [Exploratory Data Analysis](#exploratory-data-analysis)

- [Results and Findings](#results-and-findings)

- [Recommendations](#recommendations)

- [Limitations](#limitations)

- [References](#references)


## Project overview
This project delves into the Apple App Store dataset to extract insights guiding app developers in strategic decision-making. Using SQL queries, we aim to uncover trends, correlations, and patterns within the data to inform app development choices.

## Data sources

## Tools


## Data Cleaning and Preparation



```sql
-- Combine tables
SELECT * FROM appleStore_description1
UNION ALL
SELECT * FROM appleStore_description2
UNION ALL
SELECT * FROM appleStore_description3
UNION ALL
SELECT * FROM appleStore_description4;

-- Check the number of unique apps in both tables
SELECT COUNT(DISTINCT id) AS UniqueAppIDs FROM AppleStore;

SELECT COUNT(DISTINCT id) AS UniqueAppIDs FROM appleStore_description__combined;

-- Check for missing values in key fields
SELECT COUNT(*) AS missingvalues 
FROM AppleStore
WHERE track_name IS NULL OR user_rating IS NULL OR prime_genre IS NULL;

SELECT COUNT(*) AS missingvalues 
FROM appleStore_description__combined
WHERE app_desc IS NULL;
```



## Exploratory Data Analysis



```sql
-- Find out the number of apps per genre
SELECT prime_genre, COUNT(*) AS NumApps
FROM AppleStore
GROUP BY prime_genre
ORDER BY NumApps DESC;

-- Get an overview of the apps' ratings
SELECT MIN(user_rating) AS MinRating,
       MAX(user_rating) AS MaxRating,
       AVG(user_rating) AS AVGRating
FROM AppleStore;

-- Get the distribution of app prices
SELECT (price / 2) * 2 AS priceBinStart,
       ((price / 2) * 2) + 2 AS priceBinEnd,
       COUNT(*) AS NumApps
FROM AppleStore
GROUP BY priceBinStart
ORDER BY priceBinStart;

-- Data Analysis

-- Determine whether Paid apps have higher ratings than free apps
SELECT CASE 
         WHEN price > 0 THEN 'Paid'
         ELSE 'Free'
       END AS App_type,
       AVG(user_rating) AS AVG_Rating
FROM AppleStore
GROUP BY App_Type;

-- Check if apps with more supported languages have higher ratings
SELECT CASE 
         WHEN lang_num < 10 THEN '<10 languages'
         WHEN lang_num BETWEEN 10 AND 30 THEN '10-30 languages'
         ELSE '>30 languages'
       END AS language_bucket,
       AVG(user_rating) AS Avg_Rating
FROM AppleStore
GROUP BY language_bucket
ORDER BY Avg_Rating DESC;

-- Check genres with low ratings
SELECT prime_genre,
       AVG(user_rating) AS Avg_rating
FROM AppleStore
GROUP BY prime_genre
ORDER BY Avg_rating ASC
LIMIT 10;

-- Check if there is correlation between the length of the app description and the user rating
SELECT CASE 
         WHEN LENGTH(b.app_desc) < 500 THEN 'Short'
         WHEN LENGTH(b.app_desc) BETWEEN 500 AND 1000 THEN 'Medium'
         ELSE 'Long'
       END AS description_length_bucket,
       AVG(a.user_rating) AS average_rating
FROM AppleStore AS a
JOIN appleStore_description__combined AS b ON a.id = b.id
GROUP BY description_length_bucket
ORDER BY average_rating DESC;

-- Check the top-rated apps for each genre
SELECT prime_genre,
       track_name,
       user_rating
FROM (
       SELECT
         prime_genre,
         track_name,
         user_rating,
         RANK() OVER(PARTITION BY prime_genre ORDER BY user_rating DESC, rating_count_tot DESC) AS rank
       FROM AppleStore
     ) AS a
WHERE a.rank = 1;
```





## Results and Findings

 1. paid apps have better ratings 

 2. apps supporting between 10 and 30 languages have better ratings 

 3. finance and book apps have low ratings 
 
 4. apps with longer descriptions have better ratings 
 
 5. a new app should aim for a rating of atleast 3.5 and above
 
 6. games and entertainment apps have a higher competition
  



## Recommendations

Certainly! Here are the recommendations in UK English:

1. **Prioritise Developing Paid Apps**: Given that paid apps tend to garner better ratings, consider focusing on the development of paid applications. Ensure that the paid apps offer unique value propositions or enhanced features to justify the cost to users.

2. **Target Multilingual Support**: Aim to develop apps that support between 10 and 30 languages, as these apps tend to receive higher ratings. Providing multilingual support can enhance the accessibility and appeal of your app to a broader audience.

3. **Exercise Caution with Finance and Book Apps**: Finance and book apps have been observed to have lower ratings. Before investing heavily in these categories, conduct thorough market research to identify pain points and opportunities for improvement. Consider innovative features or unique approaches to differentiate your app from competitors.

4. **Invest in Detailed Descriptions**: Apps with longer descriptions tend to receive better ratings. Prioritise crafting comprehensive and engaging app descriptions that effectively communicate the value proposition and features of your app. Highlighting key functionalities and benefits can positively influence user perceptions and ratings.

5. **Set a Rating Benchmark**: Aim for a minimum rating of at least 3.5 and above for your app. This benchmark ensures a competitive edge in the market and reflects positively on the quality and user experience of your app. Continuously monitor user feedback and ratings to identify areas for improvement and maintain high user satisfaction levels.

6. **Consider Market Competition in Games and Entertainment**: Recognise that the games and entertainment categories have higher competition levels. While these categories offer significant potential for success, be prepared to differentiate your app through unique gameplay mechanics, compelling content, or innovative features to stand out in the crowded market.

By incorporating these recommendations into your app development strategy, you can increase the likelihood of developing successful and well-received apps that resonate with users and achieve higher ratings in the competitive app market.
## Limitations

## References
