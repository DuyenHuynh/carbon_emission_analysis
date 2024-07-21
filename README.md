# carbon_emission_analysis
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. The purpose is to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends. 

# Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

# Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

## 1. Table 'product_emissions'
    SELECT * FROM product_emissions LIMIT 5;

| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

## 2. Table 'industry_groups'
    SELECT * FROM industry_groups LIMIT 5;

| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 

## 3. Table 'companies'
    SELECT * FROM companies LIMIT 5;

| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 

## 4. Table 'countries'
    SELECT * FROM countries LIMIT 5;

| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 

# Top 10 Products contributing the most to carbon emissions
    SELECT 
      product_name
      , SUM(carbon_footprint_pcf) AS total_pcf
    FROM product_emissions
    GROUP BY product_name
    ORDER BY total_pcf DESC
    LIMIT 10;

| product_name                                                                                                                       | total_pcf | 
| ---------------------------------------------------------------------------------------------------------------------------------: | --------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044   | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187   | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608   | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625   | 
| TCDE                                                                                                                               | 198150    | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687    | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000    | 
| Electric Motor                                                                                                                     | 160655    | 
| Audi A6                                                                                                                            | 111282    | 
| Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | 100621    | 


