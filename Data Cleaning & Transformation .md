# 🎮 DataDrivenInsights-CompetitiveGaming
## 🧰 Data Cleaning & Transformation 

Before cleaning the data, let's take a glance at the dataset...

```sql
SELECT * 
FROM `league_of_legends_champion_stats_13_13`
`````
<img width="518" alt="Screenshot 2024-12-15 at 9 09 48 PM" src="https://github.com/user-attachments/assets/b8c15f5d-dd95-4b39-8676-5c67645718ad" />

<sub><sup>❗This dataset contains 247 unique rows of champion statistics. The above image only contains a small percentage of the data within the dataset. </sub></sup>

The dataset looks complete containing information on champion win % and ban % which will be useful for the analysis. No null or duplicate values within the data. 

Important to note that some champions contain more than one statistic correlating to different roles due to champion flexibility or ‘off-meta’ gameplay. Off-meta gameplay can be influenced by champions performing better outside of their designated roles. Eg. Pantheon, a flexible fighter that is popularily played within these 3 different roles:


<img width="510" alt="Screenshot 2024-12-15 at 9 15 43 PM" src="https://github.com/user-attachments/assets/72b5f051-4941-46d8-ad52-4af6be3407ff" />  <space>


When exploring the dataset farther it can be seen that the variables `win %`, `pick %`, `ban %`, and `role %` are actually strings and not floats. This will make these values incompatable for future calculations and therefore must be transformed before further analysis. 
First, let’s update the dataset to remove the ‘%’ from the values and then modify the columns. 

```sql
 SET SQL_SAFE_UPDATES = 0; 
 UPDATE `league_of_legends_champion_stats_13_13`
 SET
    `Win %` = REPLACE(`Win %`, '%', ''),
    `Role %` = REPLACE(`Role %`, '%', ''),
    `Pick %` = REPLACE(`Pick %`, '%', ''),
    `Ban %` = REPLACE(`Ban %`, '%', '');
 SET SQL_SAFE_UPDATES = 1;
`````

Now that the '%' signs have been removed from the values, let's permanently modify the string values to floats (more specifically a double precision floating point data type). 

```sql
ALTER TABLE `league_of_legends_champion_stats_13_13`
MODIFY COLUMN `Win %` DOUBLE;
ALTER TABLE `league_of_legends_champion_stats_13_13`
MODIFY COLUMN `Role %` DOUBLE;
ALTER TABLE `league_of_legends_champion_stats_13_13`
MODIFY COLUMN `Pick %` DOUBLE;
ALTER TABLE `league_of_legends_champion_stats_13_13`
MODIFY COLUMN `Ban %` DOUBLE;

DESCRIBE `league_of_legends_champion_stats_13_13`
`````
Once the columns have been modified, the `DESCRIBE` function can be utilized to double check that the variables `win %`, `pick %`, `ban %`, and `role %` are now 'double' variables. At this point, the dataset should treat the modified variables as floats and can be tested by ordering `win %` in descending order to get the following top 3 results: 

<img width="520" alt="Screenshot 2024-12-15 at 9 37 35 PM" src="https://github.com/user-attachments/assets/a54a6fe7-6fb8-4637-bc1d-b7f21ce1c631" /> <space>


Notice that the '%' signs have also been removed. The dataset is now ready for further analysis. 
***
