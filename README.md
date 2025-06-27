# carbon-emission-analysis

![image](https://github.com/user-attachments/assets/3b28a916-b253-4287-ba8d-de1677f3a317)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## Data Source
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![image](https://github.com/user-attachments/assets/00011436-0c3e-4268-b89e-d069d3865766)

## Data Exploring
### Table 'Product_emissions'
```sql
SELECT *
FROM product_emissions
LIMIT 10;
```
| id            | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf                       | operations_percent_total_pcf                     | downstream_percent_total_pcf                     | 
| ------------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -----------------------------------------------: | -----------------------------------------------: | -----------------------------------------------: | 
| 10056-1-2014  | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 
| 10056-1-2015  | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 
| 10222-1-2013  | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                                            | 17.36                                            | 2.01                                             | 
| 10261-1-2017  | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                                            | 5.51                                             | 63.84                                            | 
| 10261-2-2017  | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                                            | 4.51                                             | 70.41                                            | 
| 10261-3-2017  | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 2274                 | 20.05                                            | 3.61                                             | 76.34                                            | 
| 10324-1-2016  | 15         | 16         | 19                | 2016 | KURALON  fiber                                                  | 1500      | 10000                | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 
| 10418-1-2013  | 84         | 9          | 19                | 2013 | Portland Cement                                                 | 1000      | 1102                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 
| 10661-10-2014 | 85         | 28         | 11                | 2014 | Regular Straight 505® Jeans – Steel (Water                      | 0.7665    | 15                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 
| 10661-10-2015 | 85         | 28         | 6                 | 2015 | Regular Straight 505® Jeans – Steel (Water                      | 0.7665    | 15                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 
### Table 'industry_groups'
```sql
SELECT *
FROM industry_groups
LIMIT 5;
```
| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 
### Table 'companies'
```sql
SELECT *
FROM companies
LIMIT 5;
```
| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 
### Table 'countries'
```sql
SELECT *
FROM countries
LIMIT 5;
```
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 

### Find duplicate
```sql
SELECT *, COUNT(*) AS count_duplicate
FROM product_emissions
GROUP BY
	id,
	company_id,
	country_id,
	industry_group_id,
	year,
	product_name,
	weight_kg,
	carbon_footprint_pcf,
	upstream_percent_total_pcf,
	operations_percent_total_pcf,
	downstream_percent_total_pcf
HAVING COUNT(*) > 1
LIMIT 10;
```
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf                       | operations_percent_total_pcf                     | downstream_percent_total_pcf                     | count_duplicate | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -----------------------------------------------: | -----------------------------------------------: | -----------------------------------------------: | --------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2               | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2               | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                                            | 17.36                                            | 2.01                                             | 2               | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                                            | 5.51                                             | 63.84                                            | 2               | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                                            | 4.51                                             | 70.41                                            | 2               | 
| 10261-3-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 2274                 | 20.05                                            | 3.61                                             | 76.34                                            | 2               | 
| 10324-1-2016 | 15         | 16         | 19                | 2016 | KURALON  fiber                                                  | 1500      | 10000                | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10418-1-2013 | 84         | 9          | 19                | 2013 | Portland Cement                                                 | 1000      | 1102                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10661-1-2014 | 85         | 28         | 11                | 2014 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10661-1-2015 | 85         | 28         | 6                 | 2015 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               |

There are 171 duplicate rows, they need to be removed before analysis.

### Which products contribute the most to carbon emissions?
```sql
SELECT product_name, ROUND (AVG(carbon_footprint_pcf),2) AS carbon_emissions
FROM product_emissions
GROUP BY product_name
ORDER BY carbon_emissions DESC
LIMIT 10;
```
| product_name                                                                                                                       | carbon_emissions | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00       | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00       | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00       | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00       | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00        | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00        | 
| TCDE                                                                                                                               | 99075.00         | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00         | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00         | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00         | 

There are 661 product names, with Wind Turbine, Land Cruiser, Mercedes-Benz contributing the most to carbon emissions.

### What are the industry groups of these products?
```sql
SELECT
	product_name,
	industry_groups.industry_group,
	ROUND(AVG(carbon_footprint_pcf),2) AS "carbon emissions"
FROM product_emissions pe
JOIN industry_groups ON pe.industry_group_id = industry_groups.id
GROUP BY (pe.product_name)
ORDER BY ROUND(AVG(carbon_footprint_pcf),2) DESC
LIMIT 10;
```
| product_name                                                                                                                       | industry_group                     | carbon emissions | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ---------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00       | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00       | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00       | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00       | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00        | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00        | 
| TCDE                                                                                                                               | Materials                          | 99075.00         | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00         | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00         | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00         | 

Electrical Equipment and Machinery, Automobiles & Components, and Materials are 3 industry groups of above products.

### What are the industries with the highest contribution to carbon emissions?
```sql
SELECT
	industry_groups.industry_group,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emissions"
FROM product_emissions pe
JOIN industry_groups ON pe.industry_group_id = industry_groups.id
GROUP BY (industry_groups.industry_group)
ORDER BY ROUND(SUM(carbon_footprint_pcf),2) DESC
LIMIT 10;
```
| industry_group                                   | carbon emissions | 
| -----------------------------------------------: | ---------------: | 
| Electrical Equipment and Machinery               | 9801558.00       | 
| Automobiles & Components                         | 2582264.00       | 
| Materials                                        | 577595.00        | 
| Technology Hardware & Equipment                  | 363776.00        | 
| Capital Goods                                    | 258712.00        | 
| "Food, Beverage & Tobacco"                       | 111131.00        | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.00         | 
| Chemicals                                        | 62369.00         | 
| Software & Services                              | 46544.00         | 
| Media                                            | 23017.00         | 

Electrical Equipment and Machinery, Automobiles & Components, and Materials are 3 industry groups with the highest contribution to carbon emissions

### What are the companies with the highest contribution to carbon emissions?
```sql
SELECT
	companies.company_name,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emissions"
FROM product_emissions pe
JOIN companies ON pe.company_id = companies.id
GROUP BY (companies.company_name)
ORDER BY ROUND(SUM(carbon_footprint_pcf),2) DESC
LIMIT 10;
```
| company_name                            | carbon emissions | 
| --------------------------------------: | ---------------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464.00       | 
| Daimler AG                              | 1594300.00       | 
| Volkswagen AG                           | 655960.00        | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016.00        | 
| "Hino Motors, Ltd."                     | 191687.00        | 
| Arcelor Mittal                          | 167007.00        | 
| Weg S/A                                 | 160655.00        | 
| General Motors Company                  | 137007.00        | 
| "Lexmark International, Inc."           | 132012.00        | 
| "Daikin Industries, Ltd."               | 105600.00        | 

"Gamesa Corporación Tecnológica, S.A.", Daimler AG, and Volkswagen AG are 3 companies with the highest contribution to carbon emissions

### What are the countries with the highest contribution to carbon emissions?
```sql
SELECT
	countries.country_name,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emissions"
FROM product_emissions pe
JOIN countries ON pe.country_id = countries.id
GROUP BY (countries.country_name)
ORDER BY ROUND(SUM(carbon_footprint_pcf),2) DESC
LIMIT 10;
```
| country_name | carbon emissions | 
| -----------: | ---------------: | 
| Spain        | 9786130.00       | 
| Germany      | 2251225.00       | 
| Japan        | 653237.00        | 
| USA          | 518381.00        | 
| South Korea  | 186965.00        | 
| Brazil       | 169337.00        | 
| Luxembourg   | 167007.00        | 
| Netherlands  | 70417.00         | 
| Taiwan       | 62875.00         | 
| India        | 24574.00         | 

Spain, Germany, and Japan are 3 countries with the highest contribution to carbon emissions

### What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT
	year,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emissions"
FROM product_emissions pe
GROUP BY year;
```
| year | carbon emissions | 
| ---: | ---------------: | 
| 2013 | 503857.00        | 
| 2014 | 624226.00        | 
| 2015 | 10840415.00      | 
| 2016 | 1640182.00       | 
| 2017 | 340271.00        | 

The carbon emissions increased from 2013 to 2016, then decreased from 2016 to 2017

### Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
```sql
SELECT
	industry_groups.industry_group,
	year,
	ROUND(SUM(carbon_footprint_pcf),2) AS "carbon emissions"
FROM product_emissions pe
JOIN industry_groups ON pe.industry_group_id = industry_groups.id
GROUP BY industry_groups.industry_group, year;
```
| industry_group                                                         | year | carbon emissions | 
| ---------------------------------------------------------------------: | ---: | ---------------: | 
| "Consumer Durables, Household and Personal Products"                   | 2015 | 931.00           | 
| "Food, Beverage & Tobacco"                                             | 2013 | 4995.00          | 
| "Food, Beverage & Tobacco"                                             | 2014 | 2685.00          | 
| "Food, Beverage & Tobacco"                                             | 2015 | 0.00             | 
| "Food, Beverage & Tobacco"                                             | 2016 | 100289.00        | 
| "Food, Beverage & Tobacco"                                             | 2017 | 3162.00          | 
| "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 2015 | 8909.00          | 
| "Mining - Iron, Aluminum, Other Metals"                                | 2015 | 8181.00          | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2013 | 32271.00         | 
| "Pharmaceuticals, Biotechnology & Life Sciences"                       | 2014 | 40215.00         | 
| "Textiles, Apparel, Footwear and Luxury Goods"                         | 2015 | 387.00           | 
| Automobiles & Components                                               | 2013 | 130189.00        | 
| Automobiles & Components                                               | 2014 | 230015.00        | 
| Automobiles & Components                                               | 2015 | 817227.00        | 
| Automobiles & Components                                               | 2016 | 1404833.00       | 
| Capital Goods                                                          | 2013 | 60190.00         | 
| Capital Goods                                                          | 2014 | 93699.00         | 
| Capital Goods                                                          | 2015 | 3505.00          | 
| Capital Goods                                                          | 2016 | 6369.00          | 
| Capital Goods                                                          | 2017 | 94949.00         | 
| Chemicals                                                              | 2015 | 62369.00         | 
| Commercial & Professional Services                                     | 2013 | 1157.00          | 
| Commercial & Professional Services                                     | 2014 | 477.00           | 
| Commercial & Professional Services                                     | 2016 | 2890.00          | 
| Commercial & Professional Services                                     | 2017 | 741.00           | 
| Consumer Durables & Apparel                                            | 2013 | 2867.00          | 
| Consumer Durables & Apparel                                            | 2014 | 3280.00          | 
| Consumer Durables & Apparel                                            | 2016 | 1162.00          | 
| Containers & Packaging                                                 | 2015 | 2988.00          | 
| Electrical Equipment and Machinery                                     | 2015 | 9801558.00       | 
| Energy                                                                 | 2013 | 750.00           | 
| Energy                                                                 | 2016 | 10024.00         | 
| Food & Beverage Processing                                             | 2015 | 141.00           | 
| Food & Staples Retailing                                               | 2014 | 773.00           | 
| Food & Staples Retailing                                               | 2015 | 706.00           | 
| Food & Staples Retailing                                               | 2016 | 2.00             | 
| Gas Utilities                                                          | 2015 | 122.00           | 
| Household & Personal Products                                          | 2013 | 0.00             | 
| Materials                                                              | 2013 | 200513.00        | 
| Materials                                                              | 2014 | 75678.00         | 
| Materials                                                              | 2016 | 88267.00         | 
| Materials                                                              | 2017 | 213137.00        | 
| Media                                                                  | 2013 | 9645.00          | 
| Media                                                                  | 2014 | 9645.00          | 
| Media                                                                  | 2015 | 1919.00          | 
| Media                                                                  | 2016 | 1808.00          | 
| Retailing                                                              | 2014 | 19.00            | 
| Retailing                                                              | 2015 | 11.00            | 
| Semiconductors & Semiconductor Equipment                               | 2014 | 50.00            | 
| Semiconductors & Semiconductor Equipment                               | 2016 | 4.00             | 
| Semiconductors & Semiconductors Equipment                              | 2015 | 3.00             | 
| Software & Services                                                    | 2013 | 6.00             | 
| Software & Services                                                    | 2014 | 146.00           | 
| Software & Services                                                    | 2015 | 22856.00         | 
| Software & Services                                                    | 2016 | 22846.00         | 
| Software & Services                                                    | 2017 | 690.00           | 
| Technology Hardware & Equipment                                        | 2013 | 61100.00         | 
| Technology Hardware & Equipment                                        | 2014 | 167361.00        | 
| Technology Hardware & Equipment                                        | 2015 | 106157.00        | 
| Technology Hardware & Equipment                                        | 2016 | 1566.00          | 
| Technology Hardware & Equipment                                        | 2017 | 27592.00         | 
| Telecommunication Services                                             | 2013 | 52.00            | 
| Telecommunication Services                                             | 2014 | 183.00           | 
| Telecommunication Services                                             | 2015 | 183.00           | 
| Tires                                                                  | 2015 | 2022.00          | 
| Tobacco                                                                | 2015 | 1.00             | 
| Trading Companies & Distributors and Commercial Services & Supplies    | 2015 | 239.00           | 
| Utilities                                                              | 2013 | 122.00           | 
| Utilities                                                              | 2016 | 122.00           | 

