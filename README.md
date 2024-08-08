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
The population data is ingested as a .csv file to a blob storage. It is then transfered to the ADLS Gen 2 using linked services.

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


## Transfromation

## Visualization














