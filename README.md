# Session-3.5-Project-2---Carbon-Emission-Analysis
Swiss Code Course - Practice Session 3.5:

## A - Project's objective (3-5 lines)
During the past few decades, we as human being can forseee the significant effect of climate change. One of the reasons contriubting to the changes is carbon emission from human being's daily and industrial production activities. 
This project is acting as a small analization on carbon emissions which mainly examines the carbon footprint across various industries; and Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

How to do project : 
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

### Connection details for a database client 

|Host/Server: |112.213.86.31 |
| --- | --- |
|Port  |3360|
|Username  |marshmallow  |
|Password  |N3unkNbXQYh33og  |
|Database |carbon_emissions  |


## B - Mining (use queries)
Data is ready to be analyzed.

## C - Findings
From the database, some of the findings could be considered from answers for these questions: 

### 1. Which products contribute the most to carbon emissions?

```SQL
select id, product_name, weight_kg, carbon_footprint_pcf
from product_emissions
order by carbon_footprint_pcf desc
limit 5
```
#### Result:

| id           | product_name                                                       | weight_kg | carbon_footprint_pcf | 
| -----------: | -----------------------------------------------------------------: | --------: | -------------------: | 
| 22917-4-2015 | Wind Turbine G128 5 Megawats                                       | 600000    | 3718044              | 
| 22917-5-2015 | Wind Turbine G132 5 Megawats                                       | 600000    | 3276187              | 
| 22917-3-2015 | Wind Turbine G114 2 Megawats                                       | 400000    | 1532608              | 
| 22917-2-2015 | Wind Turbine G90 2 Megawats                                        | 361000    | 1251625              | 
| 8362-1-2016  | Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit. | 2272.33   | 191687               | 


### 2. What are the industry groups of these products?

Let's take a look into the top 10 products contributing the most to the carbon emmision and their industry groups.

```SQL
SELECT pe.id, pe.product_name, pe.industry_group_id, ig.industry_group, pe.weight_kg, pe.carbon_footprint_pcf
FROM product_emissions pe
LEFT JOIN industry_groups ig ON pe.industry_group_id = ig.id
ORDER BY pe.carbon_footprint_pcf DESC
LIMIT 10
```
#### Result:
| id           | product_name                                                                                                                       | industry_group_id | industry_group                     | weight_kg | carbon_footprint_pcf | 
| -----------: | ---------------------------------------------------------------------------------------------------------------------------------: | ----------------: | ---------------------------------: | --------: | -------------------: | 
| 22917-4-2015 | Wind Turbine G128 5 Megawats                                                                                                       | 13                | Electrical Equipment and Machinery | 600000    | 3718044              | 
| 22917-5-2015 | Wind Turbine G132 5 Megawats                                                                                                       | 13                | Electrical Equipment and Machinery | 600000    | 3276187              | 
| 22917-3-2015 | Wind Turbine G114 2 Megawats                                                                                                       | 13                | Electrical Equipment and Machinery | 400000    | 1532608              | 
| 22917-2-2015 | Wind Turbine G90 2 Megawats                                                                                                        | 13                | Electrical Equipment and Machinery | 361000    | 1251625              | 
| 8362-1-2016  | Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 7                 | Automobiles & Components           | 2272.33   | 191687               | 
| 904-2-2013   | Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 19                | Materials                          | 140000    | 167000               | 
| 12134-8-2017 | TCDE                                                                                                                               | 19                | Materials                          | 12000     | 99075                | 
| 12134-8-2017 | TCDE                                                                                                                               | 19                | Materials                          | 12000     | 99075                | 
| 4235-30-2016 | Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 7                 | Automobiles & Components           | 3500      | 91000                | 
| 20527-1-2014 | Electric Motor                                                                                                                     | 8                 | Capital Goods                      | 90        | 87589                | 

Base on the analytics, the top 10 industry groups that contributing to the emission the most shall be :

```SQL
/* Find 10 groups that have biggiest average carbon footprint */

SELECT  pe.product_name, pe.industry_group_id, ig.industry_group, pe.weight_kg, AVG(pe.carbon_footprint_pcf)
FROM product_emissions pe
LEFT JOIN industry_groups ig ON pe.industry_group_id = ig.id
GROUP BY ig.industry_group
ORDER BY 5 DESC
LIMIT 10
```

#### Result:

| product_name                                                                                                                                                                                                                                                                                                                                                                            | industry_group_id | industry_group                                   | weight_kg    | AVG(pe.carbon_footprint_pcf) | 
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | ----------------: | -----------------------------------------------: | -----------: | ---------------------------: | 
| ACTI9 IID K 2P 40A 30MA AC-TYPE RESIDUAL CURRENT CIRCUIT BREAKER                                                                                                                                                                                                                                                                                                                        | 13                | Electrical Equipment and Machinery               | 0.21         | 891050.7273                  | 
| VW Polo V 1.6 TDI BlueMotion Technology                                                                                                                                                                                                                                                                                                                                                 | 7                 | Automobiles & Components                         | 993          | 35373.4795                   | 
| Alliance HPLC (High Peformance Liquid Chromatography)  The Alliance is an HPLC that is unique in that it has a single set of electronic boards that control the functions for both the solvent delivery system and the autosampler in the liquid chromatograph.                                                                                                                         | 5                 | "Pharmaceuticals, Biotechnology & Life Sciences" | 59           | 24162.0000                   | 
| Office Chair                                                                                                                                                                                                                                                                                                                                                                            | 8                 | Capital Goods                                    | 20.68        | 7391.7714                    | 
| KURALON  fiber                                                                                                                                                                                                                                                                                                                                                                          | 19                | Materials                                        | 1500         | 3208.8611                    | 
| Hot Rolled Steel (HR)                                                                                                                                                                                                                                                                                                                                                                   | 4                 | "Mining - Iron, Aluminum, Other Metals"          | 1000         | 2727.0000                    | 
| Natural Gas                                                                                                                                                                                                                                                                                                                                                                             | 14                | Energy                                           | 0.0012701804 | 2154.8000                    | 
| Mobile Batteries                                                                                                                                                                                                                                                                                                                                                                        | 9                 | Chemicals                                        | 1000         | 1949.0313                    | 
| "Bloomberg's standard-issue flat panel configuration (prior to 2010) was two 19\" panels mounted on a metal stand. In early 2010 Bloomberg engaged in the WRI Product Life Cycle Roadtest for this functional unit (cradle-to-grave). The functional unit has a lifespan of 5 years, so the emissions indicated [in this report] are the full emissions associated with that lifespan." | 20                | Media                                            | 14.6         | 1534.4667                    | 
| USB software                                                                                                                                                                                                                                                                                                                                                                            | 24                | Software & Services                              | 0.0085       | 1368.9412                    | 
   
### 3. What are the industries with the highest contribution to carbon emissions?

```SQL
select industry_group, sum(carbon_footprint_pcf)
from product_emissions pe
left join industry_groups ig
	on pe.industry_group_id = ig.id
group by industry_group
order by sum(pe.carbon_footprint_pcf) desc
limit 10
```
#### Result:

| industry_group                     | sum(carbon_footprint_pcf) | 
| ---------------------------------: | ------------------------: | 
| Electrical Equipment and Machinery | 9801558                   |

   
### 4. What are the companies with the highest contribution to carbon emissions?

```SQL
select company_name, sum(carbon_footprint_pcf)
from product_emissions pe
left join companies c
	on pe.company_id = c.id
group by company_name
order by sum(carbon_footprint_pcf) desc
limit 10
```

#### Result:

| company_name                            | sum(carbon_footprint_pcf) | 
| --------------------------------------: | ------------------------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464                   | 
| Daimler AG                              | 1594300                   | 
| Volkswagen AG                           | 655960                    | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016                    | 
| "Hino Motors, Ltd."                     | 191687                    | 
| Arcelor Mittal                          | 167007                    | 
| Weg S/A                                 | 160655                    | 
| General Motors Company                  | 137007                    | 
| "Lexmark International, Inc."           | 132012                    | 
| "Daikin Industries, Ltd."               | 105600                    | 

### 5.What are the countries with the highest contribution to carbon emissions?
```SQL
select country_name, sum(carbon_footprint_pcf)
from product_emissions pe
left join countries c1
	on pe.country_id = c1.id
group by country_name
order by sum(carbon_footprint_pcf) desc
limit 10
```
#### Result: 

| country_name | sum(carbon_footprint_pcf) | 
| -----------: | ------------------------: | 
| Germany      | 9778464                   | 
| [NULL]       | 3408747                   | 
| Lithuania    | 212016                    | 
| Greece       | 191687                    | 
| Japan        | 132012                    | 
| Colombia     | 105600                    | 
| South Africa | 35505                     | 
| France       | 21364                     | 
| Italy        | 20000                     | 
| Ireland      | 11160                     | 

German seems to be the one with highest carbon emission level.

### 6.What is the trend of carbon footprints (PCFs) over the years?

```SQL
select year, sum(carbon_footprint_pcf)
from product_emissions
group by year
order by year asc
```
#### Result:

| year | sum(carbon_footprint_pcf) | 
| ---: | ------------------------: | 
| 2013 | 503857                    | 
| 2014 | 624226                    | 
| 2015 | 10840415                  | 
| 2016 | 1640182                   | 
| 2017 | 340271                    | 

On the otherhand, if we look into the avarage carbon footprints over the years, the trends seem to be a bit different:

| year | avg(carbon_footprint_pcf) | 
| ---: | ------------------------: | 
| 2013 | 2399.3190                 | 
| 2014 | 2457.5827                 | 
| 2015 | 43188.9044                | 
| 2016 | 6891.5210                 | 
| 2017 | 4050.8452                 | 

2015 faced the largest annual increase during the period from 2013 until 2017. Some of the reasons that leads to this result might be considered are : El Niño phenomina, high consumption of fossil fuels in industrial activities. El Niño often leads to an expansion of global drought area, an increase in tropical forest fires, and other landscape changes that boost atmospheric carbon dioxide levels. According to the research of scientists, one of the thinkable cause of carbon dixide increasement is the fourfold increase in human emissions from fossil fuel combustion and cement production. 

### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?

From the findings in questions 2 and analytics of this dataset, we can investigate the decrease in carbon footprint of top 10 groups over time as below:

```sql
SELECT year, industry_group, sum(carbon_footprint_pcf) as total_carbon
FROM product_emissions pe
	LEFT JOIN industry_groups ig
		ON pe.industry_group_id = ig.id
WHERE industry_group = "Electrical Equipment and Machinery"
GROUP BY year
ORDER BY year ASC
```

#### Result:

Electrical Equipment and Machinery:

| year | industry_group                     | total_carbon | 
| ---: | ---------------------------------: | -----------: | 
| 2015 | Electrical Equipment and Machinery | 9801558      | 

Automobiles & Components

| year | industry_group           | total_carbon | 
| ---: | -----------------------: | -----------: | 
| 2013 | Automobiles & Components | 130189       | 
| 2014 | Automobiles & Components | 230015       | 
| 2015 | Automobiles & Components | 817227       | 
| 2016 | Automobiles & Components | 1404833      | 

Pharmaceuticals, Biotechnology & Life Sciences:

```sql
SELECT year, industry_group, sum(carbon_footprint_pcf) as total_carbon
FROM product_emissions pe
	LEFT JOIN industry_groups ig
		ON pe.industry_group_id = ig.id
WHERE industry_group LIKE "%Pharmaceuticals%"
GROUP BY year
ORDER BY year ASC
```

| year | industry_group                                   | total_carbon | 
| ---: | -----------------------------------------------: | -----------: | 
| 2013 | "Pharmaceuticals, Biotechnology & Life Sciences" | 32271        | 
| 2014 | "Pharmaceuticals, Biotechnology & Life Sciences" | 40215        | 

Capital Goods:

| year | industry_group | total_carbon | 
| ---: | -------------: | -----------: | 
| 2013 | Capital Goods  | 60190        | 
| 2014 | Capital Goods  | 93699        | 
| 2015 | Capital Goods  | 3505         | 
| 2016 | Capital Goods  | 6369         | 
| 2017 | Capital Goods  | 94949        | 

Materials

| year | industry_group | total_carbon | 
| ---: | -------------: | -----------: | 
| 2013 | Materials      | 200513       | 
| 2014 | Materials      | 75678        | 
| 2016 | Materials      | 88267        | 
| 2017 | Materials      | 213137       | 

Mining - Iron, Aluminum, Other Metals :

```SQL
SELECT year, industry_group, sum(carbon_footprint_pcf) as total_carbon
FROM product_emissions pe
	LEFT JOIN industry_groups ig
		ON pe.industry_group_id = ig.id
WHERE industry_group LIKE "%Other Metals%"
GROUP BY year
ORDER BY year ASC
```

| year | industry_group                          | total_carbon | 
| ---: | --------------------------------------: | -----------: | 
| 2015 | "Mining - Iron, Aluminum, Other Metals" | 8181         | 


Energy:

| year | industry_group | total_carbon | 
| ---: | -------------: | -----------: | 
| 2013 | Energy         | 750          | 
| 2016 | Energy         | 10024        | 

Chemicals: 

| year | industry_group | total_carbon | 
| ---: | -------------: | -----------: | 
| 2015 | Chemicals      | 62369        | 

Media:

| year | industry_group | total_carbon | 
| ---: | -------------: | -----------: | 
| 2013 | Media          | 9645         | 
| 2014 | Media          | 9645         | 
| 2015 | Media          | 1919         | 
| 2016 | Media          | 1808         | 

Software & Services:

| year | industry_group      | total_carbon | 
| ---: | ------------------: | -----------: | 
| 2013 | Software & Services | 6            | 
| 2014 | Software & Services | 146          | 
| 2015 | Software & Services | 22856        | 
| 2016 | Software & Services | 22846        | 
| 2017 | Software & Services | 690          | 


# D - Limitation of the project analysis

All the insights from this project do have limitation due to the size of sample. the dataset contain information of 1000 records and might not large enough to represent all the case in actual. 
With the expension of the database, the findings for abovementioned questions might be changed and overall overview might need to be re-considered. 


# E - Conclusion (5-6 lines của summary findings)
From the project findings, we can recognize the most contribution of carbon emission from German, Lithuania and Greece. 

Among the records, Electrical Equipment and Machinery seems to be the one that has surpassing other industries in term of carbon footprint, clearly reflected through the carbon measure during the life cycle of Wind Turbine. However, this leads to another controversial dicussion on that people are still questioning about the pros and cons of using  Wind Turbines. The wuolde process of manufaturing and delivery of those turbines, in deed, lead to a majority of carbon poluttion but in return, we will have cleaner and more climate-friendly electricity grid, in stead of using electricy generated by fossil fuels. Hence, I don't think this should be taken into account. 

Automobiles & Components industry ranked at 2nd place among such industries with highest carbon emission. During the years, the sum of carbon footprint seems cotinually increased by the activities of manufaturing cars. 
The trend of increasing in carbon footprint happen in most of our industries. Luckily, among the top 10 group of industries that have highest carbon footprint level, we still have some industries that seem to be sucessfull in the attempt of decrease the carbon emissions such as Media and Software & Services.
