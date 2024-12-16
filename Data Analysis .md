# 🎮 DataDrivenInsights-CompetitiveGaming
### Data-Driven Insights into Champion Pick, Ban, and Win Rates in Competitive Gaming
## 📚Table of Contents
- [Exploring Pick % and Ban % Correlation](#-exploring-pick--and-ban--correlation)
- [Exploring Win % and Ban % Correlation](#-exploring-win--and-ban--correlation)
- [Champion Scores Relationship with Win % ?](#-champion-scores-relationship-with-win--)

***
## 📌 Exploring `Pick %` and `Ban %` Correlation

To explore the correlation between these two variables the Pearson Correlation Coefficiant will be utilized. The coefficiant will be able to assess the degree of association between the two variables. Here is the formula that will be utilized: 
 
 <img width="581" alt="Screenshot 2024-12-15 at 9 51 42 PM" src="https://github.com/user-attachments/assets/cdf242e4-142c-4d0e-8171-8a9d45caf7af" /> <space>

To learn more about the Pearson Correlation Coefficiant: [link](https://www.investopedia.com/terms/c/correlationcoefficient.asp)

For this analysis here are the values of interest: 
- *X* = `Pick %`
- *Y* = `Ban %`
- *n* = `COUNT(*)` = 247

Here are the variance and standard deviation calculations: 

```sql
WITH correlation AS (
	SELECT `Pick %`,
    `Ban %`,
    (`Pick %` * `Ban %`) AS XY,
    (`Pick %` * `Pick %`) AS XSQUARED, 
    (`Ban %` * `Ban %`) AS YSQUARED
    FROM `league_of_legends_champion_stats_13_13`
    )
    
SELECT 
COUNT(*) AS n,
SUM(`Pick %`) AS sum_x,
SUM(`Ban %`) AS sum_y, 
SUM(XY) AS sum_xy,
SUM(YSQUARED) AS sum_ysquared,
SUM(XSQUARED) AS sum_xsquared
FROM correlation;
`````
Output:

<img width="614" alt="Screenshot 2024-12-15 at 10 08 47 PM" src="https://github.com/user-attachments/assets/b557b1ff-631f-419d-9798-dfdc60a13e2e" /> <space>


Plugging the following values into the formula we get the following Pearson Correlation Coefficient, *r* = 0.4814065.  Thus, there is a moderate positive relationship (0.3< *r* <0.5) between pick rates and ban rates; champions with high pick rates tend to have higher ban rates.

A useful way to visualize this relationship can be done using R Statistics. Assuming the CSV is named under "leaguedata":

```{r}
library(ggplot2)

ggplot(leaguedata, aes(x = Pick.., y = Ban..)) +
  geom_point(color = "orange") +
  geom_smooth(method = "lm", color = "red") +
  ggtitle("Correlation Between Pick % VS Ban %") +
  xlab("Pick %") +
  ylab("Ban %") +
  theme_minimal()
```
<sub><sup> ❗When importing the CSV into R Statistics utilize the function read.csv2(...) rather than read.csv(...) for proper formating.
</sub></sup><line>
<sub><sup> Data cleaning will need to be done and variables will need to be properly converted before plotting. </sub></sup>

Output:

<img width="700" alt="Screenshot 2024-12-15 at 10 29 27 PM" src="https://github.com/user-attachments/assets/96087ba9-86da-4cdc-8971-d106ea9eb5fa" /> <space>

As assumed, there is a moderate positive relationship visualized but not a strong one. There are a few visible outliers, however to reduce the risk of losing valuable data and information outliers will not be removed. 

***
## 📌 Exploring `Win %` and `Ban %` Correlation

A similar approach can be followed as the previous analysis. 

```sql
WITH correlation AS (
	SELECT 
    `Win %`,
    `Ban %`,
    (`Win %` * `Ban %`) AS XY,
    (`Win %` * `Win %`) AS XSQUARED, 
    (`Ban %` * `Ban %`) AS YSQUARED
    FROM `league_of_legends_champion_stats_13_13`
    )
    
SELECT 
COUNT(*) AS n,
SUM(`Win %`) AS sum_x,
SUM(`Ban %`) AS sum_y, 
SUM(XY) AS sum_xy,
SUM(YSQUARED) AS sum_ysquared,
SUM(XSQUARED) AS sum_xsquared
FROM correlation;
`````
Output:

<img width="554" alt="Screenshot 2024-12-15 at 10 20 17 PM" src="https://github.com/user-attachments/assets/fcb20c27-5883-48ce-958d-587f08426905" /> <space>

Alternatively, correlation coefficiants can also be calculated using the `COR(...)` function within R Statistics. 

```{r}
correlation <- cor(leaguedata$Win.., leaguedata$Ban.., method = "pearson")
```
The Pearson Coefficiant Correlation is *r* = 0.09024683. This indicates a really weak positive relationship between the `Win %` and `Ban %`, such that ban rates do not have a meaningful impact on win rates. Let's check the scatterplot. Modifying the R code from the previous analysis gets the following plot:

<img width="718" alt="Screenshot 2024-12-15 at 10 34 13 PM" src="https://github.com/user-attachments/assets/f8129816-9399-4c5e-bc5c-1ad80434c58b" /> <space>

The plot supports the findings. Since the correlation is positive, there is a slight upward trend but the line of best fit is relatively flat considering the correlation is weak, almost 0, with data points fairly dispersed. 

***
## 📌 Champion `Scores` Relationship with `Win %` ?

High `Tier` champions have higher `Scores` and are therefore expected to have higher `Win %`. Let's check if this is true:

```sql
SELECT 
    Tier,
    AVG(`Win %`) AS avg_win,
    AVG(`Pick %`) AS avg_pick,
    AVG(`Ban %`) AS avg_ban,
    AVG(KDA) AS avg_kda,
    AVG(Score) AS avg_score
FROM `league_of_legends_champion_stats_13_13`
GROUP BY Tier
ORDER BY avg_win DESC;
`````
Output:

<img width="623" alt="Screenshot 2024-12-15 at 10 56 18 PM" src="https://github.com/user-attachments/assets/3aa0bb65-05d9-4ba5-971f-74c593a469e9" /> <space>

Champions in A tier, and B tier have higher win averages despite God tier and S tier champions having the highest overall score. It can also be seen that God and S tier champs have the highest `Pick %` and at the same time the highest `Ban %`, supporting the findings from earlier. Let’s visualize the findings in R: 

<img width="698" alt="Screenshot 2024-12-15 at 11 00 33 PM" src="https://github.com/user-attachments/assets/d8ec3761-e8a4-4cec-a8d9-a3361f67f45f" /> <space>

Average `Score`, `Ban %`, `Pick %`, decrease as `Tiers` decrease; average `Win %` is not influenced from this decrease. Let's analyze further: 

```sql
SELECT 
    Tier, 
    Class, 
    Role, 
    AVG(`Win %`) AS avg_win
FROM `league_of_legends_champion_stats_13_13`
GROUP BY Tier, Class, Role
ORDER BY avg_win DESC;
`````
Output of the top data:

<img width="283" alt="Screenshot 2024-12-15 at 11 05 41 PM" src="https://github.com/user-attachments/assets/182e8708-d640-4673-aed4-781c0ea449eb" /> <space>

A tier ADC mages hold the highest average wins in ranked games in patch 13.3; this is an off-meta role for mages. Considering a marksman, fighter, and support champion hold the top 3 highest `Score` in patch 13.3, let's see if these classes have the highest average `Win %`. Using 51% as the threshold...

```sql
WITH class_data AS (
    SELECT 
        Tier, 
        Class, 
        Role, 
        AVG(`Win %`) AS avg_win
    FROM `league_of_legends_champion_stats_13_13`
    GROUP BY Tier, Class, Role
) 
SELECT 
    Class, 
    COUNT(*) AS class_count
FROM class_data
WHERE avg_win > 51
GROUP BY Class
ORDER BY class_count DESC;
`````

Output:

<img width="101" alt="Screenshot 2024-12-15 at 11 14 39 PM" src="https://github.com/user-attachments/assets/7be3ef03-2c0c-4c7c-bfd8-4b3414a95d48" /> <space>

Champions with the highest average `Win %` are mainly tanks and mages, with no assasin champions making the threshold; ADC role having the highest `Win %` average for mages, and the support role for tanks. Let's see who these champions are: 

```sql
SELECT *
FROM `league_of_legends_champion_stats_13_13`
WHERE Tier = 'A' AND
Role = 'ADC' AND
Class = 'Mage';
`````

Output:

<img width="477" alt="Screenshot 2024-12-15 at 11 26 45 PM" src="https://github.com/user-attachments/assets/1a8169be-c500-4fb1-aa96-033ccc8c1c4b" /> <space>

```sql
SELECT *
FROM `league_of_legends_champion_stats_13_13`
WHERE Tier = 'A' AND
Role = 'MID' AND
Class = 'Tank';
`````

Output:

<img width="460" alt="Screenshot 2024-12-15 at 11 28 10 PM" src="https://github.com/user-attachments/assets/30aacc56-281d-4241-adc6-365b3617caae" /> <space>

It can be seen that champions that hold the highest average `Win %` are champions in off-meta roles; champions that have high `Win %` despite having relatively low overall scores. 

***
Now that the analysis is complete, here are the conclusions that can be derived from the analysis: [Conclusions](https://github.com/andrin187/DataDrivenInsights-CompetitiveGaming/blob/main/Findings%20%26%20Conclusions%20.md)
***












