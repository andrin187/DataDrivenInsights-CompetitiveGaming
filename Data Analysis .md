# 🎮 DataDrivenInsights-CompetitiveGaming
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

Plugging the following values into the formula we get the following Pearson Correlation Coefficient, *r* = 0.4814065.  Thus, there is a moderate positive relationship (0.3< *r* <0.5) between pick rates and ban rates; champions with high pick rates tend to have higher ban rates.

A useful way to visualize this relationship can be done using R statistics:

```{r}
library(ggplot2)

ggplot(leaguedata, aes(x = Pick.., y = Ban..)) +
  geom_point(color = "orange") +
  geom_smooth(method = "lm", color = "red") +
  ggtitle("Correlation Between Win % VS Ban %") +
  xlab("Win %") +
  ylab("Ban %")
```


