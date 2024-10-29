# Session-3.5-Project-2---Carbon-Emission-Analysis
Swiss Code Course - Practice Session 3.5:

## A - Project's objective (3-5 lines)
During the past few decades, we as human being can forseee the significant effect of climate change. One of the reasons contriubting to the changes is carbon emission from human being's daily and industrial production activities. 
This project is acting as a small analization on carbon emissions which mainly examines the carbon footprint across various industries; and Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

How to do project : 
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

### Connection details for a database client 

Host/Server: 	112.213.86.31
Port: 	3360
Username:	marshmallow
Password:	N3unkNbXQYh33og
Database:	carbon_emissions

2. Mining (use queries)
2.1 data patterns
missing, dups?, value of group by
(null ,N/A)
CONSISTENCY of format
2.2 soulitons in case of missing, duplicates ...

3. findings
3.1
3.2
most contribution carbon emmissions products
avg. sum: carbon
product name

4. limitation (3-4 lines)
giới hạn về nghiên cứu như thế nào, data chỉ cso sample 1000 dòng và không đại diện cho tất cả các case trong thực tế 
future proposal (trong tương lai có thể improve như nào)

5. conclusion (5-6 lines của summary findings)

# Questions to research
1. Which products contribute the most to carbon emissions?

```SQL
SELECT
FROM
WHERE
```

### 2. What are the industry groups of these products?
   
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



### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
