# Effects-of-Covid-19-in-Europe (An Azure Data Engineering project on UDEMY)
This project looks at datasets to study the overall effect of Covid-19 in Europe in 2020.

## Goal of the project
The goal is to visualize the death count, admission count as well as the vaccination count per country in Europe. The project also attempts to find the link between confirmed cases
and Covid-19 vaccines.

![image](https://github.com/user-attachments/assets/618d7296-7e72-402e-ac96-52183cbae643)


## Dataset
Data is ingested from 2 sources:
1. ECDC website to extract: Confirmed cases, Mortality, Hospitalization/ICU cases, Testing numbers
2. Eurostat website to extract Population by age

We started out with the intention of reading the data from the website itself. However, due to the new permission barrier, the course instructor created a gitbub repo for the 
datasets: https://github.com/cloudboxacademy/covid19.

## Solution Architecture
![image](https://github.com/user-attachments/assets/9c7a7905-b1b7-4c06-877e-f4fe2a88cee6)

## Environment Set-up
 AZURE Subscription <br>
 Data Factory <br>
 Blob Storage <br>
 Data Lake Storage GEN 2 <br>
 AZURE SQL Database <br>
 AZURE databricks <br>
 HD Insight cluster <br>


## Ingestion  
### Ingesting the Population data
The population data is ingested as a .tsv.gz file to a blob storage. 

![image](https://github.com/user-attachments/assets/97027d6b-668b-4a86-986d-0999c7d72e36)

### Solution flow for Population data
![image](https://github.com/user-attachments/assets/3d766c74-3527-4fc5-bd21-a797b5d301e5)

Steps:

Create a Linked Service To Azure Blob Storage <br>
Create a Source Data Set <br>
Create a Linked Service To Azure Data Lake storage (GEN2) <br>
Create a Sink Data set <br>
Create a Pipeline: <br>
Execute Copy activity when the file becomes available<br>
Check metadata counts before loading the data using the IF Condition <br>
Load Data into our destination <br>
ScheduleTrigger <br>

### Ingesting ECDC Data
The confirmed cases, Mortality, Hospitalization/ICU cases, Testing numbers data are read from the github repo.

The files ingested are as follows:

Case & Deaths Data.csv <br>
Hospital Admission Data.csv <br>
testing.csv <br>
country_response.csv <br>

![image](https://github.com/user-attachments/assets/ebfce600-7bd7-4c22-9f6a-1773b66004bf)

### Solution flow for the ECDC Data
![image](https://github.com/user-attachments/assets/feed5b9f-4964-4801-b200-f1c43db5c61f)

Steps:

Create a Linked Service using an HTTP connector <br>
Create a Source Data Set <br>
Create a Linked Service To Azure Data Lake storage (GEN2) <br>
Create a Sink Data set <br>
Create a Pipeline With Parameters & Variables <br>
Lookup to get all the parameters from json file, then pass it to ForEach ECDC DATA as shown below <br>
Schedule Trigger <br>

### The json file:
![image](https://github.com/user-attachments/assets/951e6585-0d65-4baf-9338-3c38d787acd9)


## Transformation
To transform the Cases and Deaths data as well as the hospital admissions data, ADF dataflows were used.
To transform the Population data, Databricks was used.

The Data Flows transformations used are:

- Select transformation
- Lookup transformation
- Filter transformation
- Join transformation
- Sort transformation
- Conditional split transformation
- Derived columns transformation
- Sink transformation

### Cases and Deaths transformation
![image](https://github.com/user-attachments/assets/6638af9e-eab3-43e0-a2d1-06d266cdd0c7)

<strong> Steps </strong>:

Cases And Deaths Source (Azure Data Lake Storage Gen2 ) <br>
Filter Europe-Only Data <br>
Select only the required columns <br>
PivotCounts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count <br>
Lookup Country to get country_code_2_digit,country_code_3_digit columns <br>
Select Only the required columns for the Sink <br>
Create a Sink dataset (Azure Data Lake Storage Gen2) <br>
Used Schedule Trigger <br>

 # Insert the pipeline picture here

 ### Hospital Admissions transformation
 ![image](https://github.com/user-attachments/assets/8f066a71-c3ce-455d-9015-f3e7e786a0b4)

 <strong> Steps </strong>
1. Hospital Admissions Source (Azure Data Lake Storage Gen2 ) <br>
2. Select only the required columns<br>
3. Lookup Country to get country_code_2_digit,country_code_3_digit columns<br>
4. Select only the required columns<br>
5. Condition Split Weekly, Daily Split condition<br>
- indicator=='Weekly new hospital admissions per 100k' || indicator=='Weekly new ICU admissions per 100k'
- indicator== "Daily hospital occupancy" || indicator=="Daily ICU occupancy"
6. For Weekly Path <br>
- Join with Date to get ecdc_Year_week, week_start_date, week_End_date
- Pivot Counts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
- Sort data using reported_year_week ASC and Country DESC
- Select only required columns for sink
- Create a sink dataset (Azure Data Lake Storage Gen2)
- Schedule Trigger
7. For Daily Path <br>
- Pivot Counts using indicator Columns(confirmed cases, deaths) and get the sum of daily cases count
- Sort Data using reported_year_week ASC and Country DESC
- Select only required columns for sink
- Create a sink dataset (Azure Data Lake Storage Gen2)
- Used Schedule Trigger

### Population data transformation
![image](https://github.com/user-attachments/assets/4500ca70-de07-42f7-a2a3-11d7d2cb304c)


## Visualization

The link to the dashboard can be found here: https://app.powerbi.com/view?r=eyJrIjoiYjBhYWU0NTItMmVhOS00MGM5LTk1ZGEtMTQxZTdmZDUxMWUwIiwidCI6ImUwYjlhZTFlLWViMjYtNDZhOC1hZGYyLWQ3ZGJjZjIzNDBhOSJ9


## Resources
https://www.udemy.com/course/learn-azure-data-factory-from-scratch/learn/lecture/23983360#announcements <br>
https://github.com/hbuddana/Azure_Data_Factory_COVID-19_Reporting?tab=readme-ov-file#azure-data-factory-covid-19-reporting










