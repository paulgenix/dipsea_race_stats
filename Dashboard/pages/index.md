---
title: Dipsea Race Stats
description: Stats for the Dipsea Race

---

```sql everything1
  SELECT year, COUNT(*) AS finishers
FROM bigquery.years
GROUP BY year
ORDER BY year;
```

**The Dipsea Race**, founded in **1905**, is the oldest trail race in the U.S., held annually on the second Sunday of June in **Marin County, California**. The 7.4-mile course begins in Mill Valley, climbs 1,360 feet over Mount Tam via 600+ stairs, and descends into Stinson Beach. It runs through **Mount Tamalpais State Park, Muir Woods National Monument and the Golden Gate National Recreation Area.** 

## Number of finishers over the years
<Details title="The Dipsea Race has been held every year since 1905 except for 7 years:">
    

**1932-1933:**
Great Depression  
**1942-1945:**
World War II  
**2020:**
Covid-19 Pandemic

</Details>

<LineChart
    data={everything1}
    title="Finishers by Year"
    x="year"
    y="finishers"
    xBaseline=true    
    yAxisTitle=true
    downloadableData=false
    downloadableImage=false
    echartsOptions={{
    dataZoom: [
        {
            type: 'inside',
            start: 0,
            end: 100,
        },
    ],
}}
    >
  
    <ReferenceArea xMin="1942" xMax="1945" label="WWII" labelPosition=belowCenter color =negative stroke=warning />
    <ReferenceArea xMin="1932" xMax="1933" color =negative label="Great Depression" />
    <ReferenceArea xMin="2019" xMax="2021" color =negative label="Covid" />

</LineChart>
<Note>
Note: The race has a limit of 1,500 participants since 1977 due to safety and environmental concerns
</Note>  



```sql everything20241
SELECT 
  COUNT(CASE WHEN year = 2023 THEN 1 END) AS finishers_2023,
  COUNT(CASE WHEN year = 2024 THEN 1 END) AS finishers_2024,
FROM bigquery.years
WHERE year IN (2023, 2024);
```


```sql gender1
SELECT year, UPPER(sex) AS sex, COUNT(*) AS gender_count
FROM bigquery.V2
WHERE LOWER(sex) IN ('m', 'f')
GROUP BY year, UPPER(sex)
ORDER BY year, sex;
```



## Women Participation in the Dipsea
Some women appeared in results as early as 1951, but it wasn't until 1971 that all women were officially allowed to register and run. In 1973, Mary Boitano became the first woman to win the race. 

<Tabs background=true color=primary>
    <Tab label="Finishers by Gender">
        <LineChart
    data={gender1}
    title="Dipsea Finishers by Gender"
    x="year"
    y="gender_count"
    series="sex"
    colorPalette={['#305CDE', '#FF1DCE']}
    labelPosition=middle
    downloadableData=false
    downloadableImage=false
>
    <ReferenceLine 
        x="1971"
        labelPosition=aboveCenter
        fontSize=13
        labelSize=1
        labelColor=positive
        hideValue={false} 
    />
</LineChart>
    </Tab>
    <Tab label="Women in the Top 50">
        <LineChart
    data={gender2}
    title="Number of Women in the Top 50 Over the Years"
    x="year"
    y="num_women_top_50"
    lineColor='#FF1DCE'
    downloadableData=false
    downloadableImage=false
/>
    </Tab>
</Tabs>
<Note>
Note: Gender data may be incomplete for some years
</Note>



```sql avg1
SELECT year, avg_age as avg_age
from bigquery.age
```

## Ages in the Dipsea Race
The minimum age to participate in the Dipsea Race has been 7 years old since 2024 (previously 6). There is no maximum age limit, and the oldest finisher was 96 years old.<Info description="Jack Kirk in 2003" />

<LineChart
    data={avg}
    title="Age Statistics Over Years"
    x="year"
    y={['min_age','max_age', 'avg_age']} 
    colorPalette={['#305CDE', '#0B1F4B', 'positive']}
/>
<Note>
Note: Ages have only been recorded since 1980
</Note>

```sql avg
SELECT 
    year,
    MIN(TRY_CAST(age AS INTEGER)) AS min_age,
    MAX(TRY_CAST(age AS INTEGER)) AS max_age,
    AVG(TRY_CAST(age AS INTEGER)) AS avg_age
FROM 
    bigquery.v2
WHERE
    TRY_CAST(age AS INTEGER) >5
    AND TRY_CAST(age AS INTEGER) < 100
    AND year >= 1980
GROUP BY 
    year
ORDER BY 
    year;
```

```sql scatter
SELECT age, place_overall, year
FROM bigquery.years
WHERE year >=2021
ORDER BY age;
```


```sql scatter1
SELECT Age, Place
FROM bigquery.test
ORDER BY Age;
```

```sql gender2
SELECT COUNT(*) AS num_women_top_50, year
FROM bigquery.V2
WHERE sex = 'F'
    AND place_overall <= 50
    AND year >= 1930
GROUP BY year
ORDER BY year;
```




## How headstarts make it fair
The Dipsea Race is known for its headstarts and handicap system that ensures runners of all ages and genders have an equitable chance to win.
<Details title="How Headstarts work">

Runers are divided into 25–26 age and gender-based groups, starting one minute apart. The youngest and oldest runners get the biggest head starts—up to 25 minutes ahead of the fastest runners. Based on decades of data, the race’s handicapper updates the system yearly to keep it fair. Past winners receive a “penalty minute”. This approach ensures runners of all ages and genders have a real shot at winning.  
[2025 Headstart table](https://www.dipsea.org/handicap.php) 


#### Dipsea's Handicap History:
The system initially relied on subjective evaluations of entrants' abilities but transitioned in 1965 to an age-based model, granting the youngest and oldest participants significant advantages over "scratch" runners in their prime years (ages 19-30). Further refinements in 1971 incorporated gender and historical performance data, allowing winners to emerge from diverse demographics, including children as young as 8 and seniors in their 70s
</Details>  

```sql all
SELECT *
FROM bigquery.latestmerge
```
<Tabs background=true color=primary>
    <Tab label="Gender">
        <Grid cols={2}>
  <ScatterPlot    
    data={scatterall1} 
      x="actual_place" 
      y="age" 
      series="sex" 
      subtitle= "1 female runner made it to the top 50 (since 2021)"
      title="Actual Place (time-based)"
      colorPalette={['#305CDE', '#FF1DCE']}
      xMax=25 
      yMax=60 
      yMin=6 
      xMin=1>
    </ScatterPlot>
  
  <Group>
    <ScatterPlot 
    data={scatterall1} 
    x="place_overall" 
    y="age" 
    series="sex" 
    title="Place Overall (with headstarts)"
    subtitle= "22 female runners made it to the top 50 (since 2021)"
    seriesOrder={['M', 'F']}
    colorPalette={['#305CDE', '#FF1DCE']}
    yMax=60
    yMin=6
    xMax=25
    xMin=1>
    </ScatterPlot>
  </Group>
</Grid>
    </Tab>
    <Tab label="Age">
        <Grid cols=2>
    <ScatterPlot data={scatterall} x=actual_place (time-based) y=age title="Actual Place (time-based)"xMax=15 yMax=80 yMin=6 xMin=1 subtitle="2 runners >40 made it to the top 15 (since 2021)" >
             <ReferenceArea xMin=1 xMax=15 yMin=40 yMax=80 label="age> 40" labelPosition=topRight color=negative border=true/>
             
     </ScatterPlot>

   <Group>
    <ScatterPlot data={scatterall} 
    x=place_overall 
    y=age 
    title="Place Overall (with headstarts)"
    subtitle="27 runners >40 made it to the top 15 (since 2021)"
    xMax=15 
    yMax=80 
    yMin=6 
    xMin=1>
    <ReferenceArea xMin=1 xMax=15 yMin=40 yMax=80 label="age> 40" color=positive border=true labelPosition=topRight/>
     </ScatterPlot>

   </Group>
</Grid>
    </Tab>
</Tabs>


```sql scatterall
SELECT age, place_overall, year, sex, actual_place
FROM bigquery.latestmerge
WHERE year >=2021
ORDER BY age;
```

```sql scatterall1
SELECT age, place_overall, year, sex, actual_place
FROM bigquery.latestmerge
WHERE year >=2021
ORDER BY sex;
```


[References](/about)  
Contact: paul.genix@gmail.com


```sql scatterallage12
SELECT COUNT(*) AS runners_over_40
FROM bigquery.latestmerge
WHERE year >= 2021
  AND age > 40
    AND place_overall <= 15;
```


```sql scatterallage123
SELECT COUNT(*) AS runners_over_40
FROM bigquery.latestmerge
WHERE year >= 2021
  AND age > 40
    AND actual_place <= 15;
```