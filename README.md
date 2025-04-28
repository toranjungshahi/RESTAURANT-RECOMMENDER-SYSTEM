## RESTAURANT RECOMMENDER

In this project we have attempted to build a robust restaurant recommendation system that provides 5 personalised restaurant recommendations to the users located in Philadelphia city, USA. As the maximum number of reviews and restaurants in the dataset we obtained was in Philadelphia, we decided to build the prototype based on dataset for Philadelphia and scale it to other cities/locations. The whole system is divided in two stages namely cold stage and warm stage. Location based system is similar to other currently existing restaurant recommender system. It is used to solve the cold start problem and thus handles new users, for which the system does not have enough data to provide personalised recommendations. Therefore, cold stage of the system consists of location-based recommender system. Whereas in warm stage hybrid system comprising of content based followed by collaborative filtering is used to handle existing users. Since the existing users has enough data to create their user profile, this user profile is used to provide personalized recommendations using the hybrid system. Below  is the general structure of the system developed.

![System Structure](https://github.com/user-attachments/assets/4be92325-68cc-4691-ba22-cc750d643e12)

## Data and Data Exploration

We used business dataset from Yelp public dataset for Philadelphia city. 
- [Yelp dataset](https://www.yelp.com/dataset) (including 40000 restaurants in different cities)
- [Yelp dataset description](https://www.yelp.com/dataset/documentation/main) 


**Data Structure and Preprocessing**

The business dataset contains details for 5853 restaurants and has attributes : ['business_id', 'name', 'address', 'city', 'state', 'postal_code', 'latitude', 'longitude', 'stars', 'review_count', 'is_open', 'attributes', 'categories', 'hours']. The ‘attributes’ of a restaurant inside dataset attributes specifies the characteristics of a restaurant such as, ‘Outdoor seating’, ‘Parking available’, ‘Noise level’ etc. The dataset also contains ‘is_open’ which shows that not all the restaurants are still operating. Therefore, we delete the restaurants that are no longer operating leaving 3526 total restaurants.  Furthermore, the dataset contains ‘review_count’ that shows the number of reviews for each restaurant.
Below is basic statistics and distribution of review_count in the dataset: 

- Minimum number of review counts = 5
- Maximum number of review counts = 5721
- Mean number of review counts = 140 
- Tenth percentile = 8, 95th percentile = 564
 
Since ten percentiles of the restaurants only have maximum 8 reviews, we dropped 
the restaurants that have less than 8 reviews. This prevents recommending 
restaurants that have 5-star rating but only rated by one or two people. The final 
number of restaurants that the system can recommend from is 3202. 

## System Specifications

### User profile

- A star rating to every restaurant reviewed by this user.
- A bag of keywords which represent the preference of this user. The keywords are  generated from tags, which belong to the restaurants recently reviewed or clicked by  this user.

### Method

- Cold stage (with insufficient user data)
  - Location-based recommender system
- Warm stage (with sufficient user data)
  - A hybrid recommender system
    - Content-based method
    - Collaborative filtering method

### Location-based recommender system

- Input
  - Geographical location of the user.
  - Restaurant information in a certain range (e.g. in 5km), maybe from [Google map API](https://developers.google.com/maps)?
- Output
  - Top-N (Top 5) restaurants according to their reputation (e.g. stars)

### Hybrid recommender system

**Content-based**
  - Input
    - User keyword (e.g. Chinese food, burger)
    - Restaurant Keyword (e.g. Chinese food, burger)
  - Output
    - Top-N (Top 5) restaurants according to the similarity between user keyword and restaurant keyword.
  - Implement
    - Preprocess these restaurant tags and the keywords from the user, and convert  them to tf-idf vectors using tfidfVecorizer function in sklearn library.
    - Calculate the similarity between user’s keyword vector and every restaurant’s tag  vector.
    - Recommend Top-N (Top 5) restaurants with the highest similarity.
**Collaborative filtering**
  - Input
    - Reviews from the user
  - Output
    - Top-N (Top 5) restaurants
  - Implement
    - Analyze the sentiment of the reviews:
      - Positive (like this restaurant)
      - Negative (don't like this restaurant)
    - Create a rating matrix for each user and each restaurant, the value in the rating  matrix is depending on user’s review of the restaurant. (e.g. positive is 1, negative is 0, or some value between 0 to 1)
    - Make a prediction of restaurants that the user has not been to yet. 

## Usage 

*location_based system.ipynb* contains location based recommender system. 
Whereas, *Collaborative filtering and Hybrid System.ipynb* is a hybrid system as described above.
Final deployment of the system can be done by combining both systems.
