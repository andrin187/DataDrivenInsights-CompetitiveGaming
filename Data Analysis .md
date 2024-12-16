# 🎮 DataDrivenInsights-CompetitiveGaming
## 📚Table of Contents
- [Exploring Pick % and Ban % Correlation](#-exploring-pick--and-ban--correlation)
- [Exploring Win % and Ban % Correlation](#-exploring-win--and-ban--correlation)
- [What Traits Distinguish Champion Win Rates?](#what-traits-distinguish-champion-win-rates)

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
SUM(YSQUARED) as sum_ysquared,
SUM(XSQUARED) as sum_xsquared
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
SUM(YSQUARED) as sum_ysquared,
SUM(XSQUARED) as sum_xsquared
FROM correlation
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
## 📌 What Traits Distinguish Champion Win Rates?






