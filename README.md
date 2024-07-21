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

# Top 10 Products contributing the most to carbon emissions by Industry
    SELECT 
      product_emissions.product_name AS product_name
      , industry_groups.industry_group AS industry_group
      , ROUND(AVG(product_emissions.carbon_footprint_pcf),2) AS avg_pcf
    FROM product_emissions
    LEFT JOIN industry_groups
    ON product_emissions.industry_group_id = industry_groups.id
    GROUP BY product_name
    ORDER BY avg_pcf DESC
    LIMIT 10;

| product_name                                                                                                                       | industry_group                     | avg_pcf    | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00  | 
| TCDE                                                                                                                               | Materials                          | 99075.00   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00   | 

# Top 10 industries with the highest contribution to carbon emissions
    SELECT 
      industry_groups.industry_group AS industry_group
      , ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS total_pcf
    FROM product_emissions
    LEFT JOIN industry_groups
    ON product_emissions.industry_group_id = industry_groups.id
    GROUP BY industry_group
    ORDER BY total_pcf DESC
    LIMIT 10;

| industry_group                                   | total_pcf  | 
| -----------------------------------------------: | ---------: | 
| Electrical Equipment and Machinery               | 9801558.00 | 
| Automobiles & Components                         | 2582264.00 | 
| Materials                                        | 577595.00  | 
| Technology Hardware & Equipment                  | 363776.00  | 
| Capital Goods                                    | 258712.00  | 
| "Food, Beverage & Tobacco"                       | 111131.00  | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.00   | 
| Chemicals                                        | 62369.00   | 
| Software & Services                              | 46544.00   | 
| Media                                            | 23017.00   | 

# Top 10 companies contributing the most to carbon emissions
    SELECT 
      companies.company_name AS company
      , ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS total_pcf
    FROM product_emissions
    LEFT JOIN companies
    ON product_emissions.company_id = companies.id
    GROUP BY company
    ORDER BY total_pcf DESC
    LIMIT 10;

| company                                 | total_pcf  | 
| --------------------------------------: | ---------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464.00 | 
| Daimler AG                              | 1594300.00 | 
| Volkswagen AG                           | 655960.00  | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016.00  | 
| "Hino Motors, Ltd."                     | 191687.00  | 
| Arcelor Mittal                          | 167007.00  | 
| Weg S/A                                 | 160655.00  | 
| General Motors Company                  | 137007.00  | 
| "Lexmark International, Inc."           | 132012.00  | 
| "Daikin Industries, Ltd."               | 105600.00  | 

# Top 10 countries contributing the most to carbon emissions
    SELECT 
      countries.country_name AS country
      , ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS total_pcf
    FROM product_emissions
    LEFT JOIN countries
    ON product_emissions.country_id = countries.id
    GROUP BY country
    ORDER BY total_pcf DESC
    LIMIT 10;

| country     | total_pcf  | 
| ----------: | ---------: | 
| Spain       | 9786130.00 | 
| Germany     | 2251225.00 | 
| Japan       | 653237.00  | 
| USA         | 518381.00  | 
| South Korea | 186965.00  | 
| Brazil      | 169337.00  | 
| Luxembourg  | 167007.00  | 
| Netherlands | 70417.00   | 
| Taiwan      | 62875.00   | 
| India       | 24574.00   | 

# Trend of carbon footprints (PCFs) over the years
    SELECT 
      year
      , ROUND(SUM(carbon_footprint_pcf),2) AS total_pcf
    FROM product_emissions
    GROUP BY year;

| year | total_pcf   | 
| ---: | ----------: | 
| 2013 | 503857.00   | 
| 2014 | 624226.00   | 
| 2015 | 10840415.00 | 
| 2016 | 1640182.00  | 
| 2017 | 340271.00   | 

# Industry groups have demonstrated the most notable decrease in carbon footprints (PCFs) over time
    SELECT
      product_emissions.year AS year
      ,industry_groups.industry_group AS industry_group
      , ROUND(SUM(product_emissions.carbon_footprint_pcf),2) AS total_pcf
    FROM product_emissions
    LEFT JOIN industry_groups
    ON product_emissions.industry_group_id = industry_groups.id
    GROUP BY 
      year
      ,industry_group
    ORDER BY 
      year ASC
      , industry_group ASC;

| year | industry_group                                                         | total_pcf  | 
| ---: | ---------------------------------------------------------------------: | ---------: | 
| 2013 | "Food, Beverage & Tobacco"                                             | 4995.00    | 
| 2013 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 32271.00   | 
| 2013 | Automobiles & Components                                               | 130189.00  | 
| 2013 | Capital Goods                                                          | 60190.00   | 
| 2013 | Commercial & Professional Services                                     | 1157.00    | 
| 2013 | Consumer Durables & Apparel                                            | 2867.00    | 
| 2013 | Energy                                                                 | 750.00     | 
| 2013 | Household & Personal Products                                          | 0.00       | 
| 2013 | Materials                                                              | 200513.00  | 
| 2013 | Media                                                                  | 9645.00    | 
| 2013 | Software & Services                                                    | 6.00       | 
| 2013 | Technology Hardware & Equipment                                        | 61100.00   | 
| 2013 | Telecommunication Services                                             | 52.00      | 
| 2013 | Utilities                                                              | 122.00     | 
| 2014 | "Food, Beverage & Tobacco"                                             | 2685.00    | 
| 2014 | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 40215.00   | 
| 2014 | Automobiles & Components                                               | 230015.00  | 
| 2014 | Capital Goods                                                          | 93699.00   | 
| 2014 | Commercial & Professional Services                                     | 477.00     | 
| 2014 | Consumer Durables & Apparel                                            | 3280.00    | 
| 2014 | Food & Staples Retailing                                               | 773.00     | 
| 2014 | Materials                                                              | 75678.00   | 
| 2014 | Media                                                                  | 9645.00    | 
| 2014 | Retailing                                                              | 19.00      | 
| 2014 | Semiconductors & Semiconductor Equipment                               | 50.00      | 
| 2014 | Software & Services                                                    | 146.00     | 
| 2014 | Technology Hardware & Equipment                                        | 167361.00  | 
| 2014 | Telecommunication Services                                             | 183.00     | 
| 2015 | "Consumer Durables, Household and Personal Products"                   | 931.00     | 
| 2015 | "Food, Beverage & Tobacco"                                             | 0.00       | 
| 2015 | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 8909.00    | 
| 2015 | "Mining - Iron, Aluminum, Other Metals"                                | 8181.00    | 
| 2015 | "Textiles, Apparel, Footwear and Luxury Goods"                         | 387.00     | 
| 2015 | Automobiles & Components                                               | 817227.00  | 
| 2015 | Capital Goods                                                          | 3505.00    | 
| 2015 | Chemicals                                                              | 62369.00   | 
| 2015 | Containers & Packaging                                                 | 2988.00    | 
| 2015 | Electrical Equipment and Machinery                                     | 9801558.00 | 
| 2015 | Food & Beverage Processing                                             | 141.00     | 
| 2015 | Food & Staples Retailing                                               | 706.00     | 
| 2015 | Gas Utilities                                                          | 122.00     | 
| 2015 | Media                                                                  | 1919.00    | 
| 2015 | Retailing                                                              | 11.00      | 
| 2015 | Semiconductors & Semiconductors Equipment                              | 3.00       | 
| 2015 | Software & Services                                                    | 22856.00   | 
| 2015 | Technology Hardware & Equipment                                        | 106157.00  | 
| 2015 | Telecommunication Services                                             | 183.00     | 
| 2015 | Tires                                                                  | 2022.00    | 
| 2015 | Tobacco                                                                | 1.00       | 
| 2015 | Trading Companies & Distributors and Commercial Services & Supplies    | 239.00     | 
| 2016 | "Food, Beverage & Tobacco"                                             | 100289.00  | 
| 2016 | Automobiles & Components                                               | 1404833.00 | 
| 2016 | Capital Goods                                                          | 6369.00    | 
| 2016 | Commercial & Professional Services                                     | 2890.00    | 
| 2016 | Consumer Durables & Apparel                                            | 1162.00    | 
| 2016 | Energy                                                                 | 10024.00   | 
| 2016 | Food & Staples Retailing                                               | 2.00       | 
| 2016 | Materials                                                              | 88267.00   | 
| 2016 | Media                                                                  | 1808.00    | 
| 2016 | Semiconductors & Semiconductor Equipment                               | 4.00       | 
| 2016 | Software & Services                                                    | 22846.00   | 
| 2016 | Technology Hardware & Equipment                                        | 1566.00    | 
| 2016 | Utilities                                                              | 122.00     | 
| 2017 | "Food, Beverage & Tobacco"                                             | 3162.00    | 
| 2017 | Capital Goods                                                          | 94949.00   | 
| 2017 | Commercial & Professional Services                                     | 741.00     | 
| 2017 | Materials                                                              | 213137.00  | 
| 2017 | Software & Services                                                    | 690.00     | 
| 2017 | Technology Hardware & Equipment                                        | 27592.00   | 

