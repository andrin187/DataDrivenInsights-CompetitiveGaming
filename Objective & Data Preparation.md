# 🎮 DataDrivenInsights-CompetitiveGaming
### Data-Driven Insights into Champion Pick, Ban, and Win Rates in Competitive Gaming
## ❗Objective 

In this project I want to answer the three following questions: 

- What is the correlation between pick rates and ban rates in patch 13.3 ranked games? Do higher pick rates equal higher ban rates? 

- What is ban rates impact on win rates? Is there a correlation between the two variables? Do champions with higher ban rates have higher win rates?

- What is the relationship between a champions score with their win rate? Is the hypothesis that a higher score equals a higher win rate true?

***

## 🧰 Data Preparation

This project used the following dataset: 
https://www.kaggle.com/datasets/vivovinco/league-of-legends-stats-s13

The following dataset has 100% completeness, and 100% credibility. Data from patch 13.1 all the way to the last patch of the season, 13.3, is included. For this analysis, we will be using the data from the last and latest patch of season 13. 

The dataset contains champion statistics of ranked League of Legends games with the following variables: 

- Name : Name of the champion

- Class : Fighter, Assassin, Mage, Marksman, Support or Tank

- Role : Top, Mid, ADC, Support or Jungle

- Tier : God, S, A, B, C or D

- Score : Overall score of the champion

- Trend : Trend of the score

- Win % : Win rate of the champion
  
- Role % : Role rate played with the champion

- Pick % : Pick rate of the champion

- Ban % : Ban rate of the champion

- KDA : (Kill+Death)/Assist ratio of the champion

Here is a glance at the data:
````sql
SELECT * 
FROM `league_of_legends_champion_stats_13_13`
LIMIT 30
`````

<img width="518" alt="Screenshot 2024-12-16 at 1 05 53 AM" src="https://github.com/user-attachments/assets/fb56d985-184d-4b85-aa5c-6eb5fabbf52a" />


***
 


