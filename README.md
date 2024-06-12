# TSA case study using SAS
## Prepare and analyze claims data from the Transportation Security Administration(TSA)


## Problem Statement

The TSA is an agency of the US Department of Homeland Security that has authority over the security of the traveling public. Claims are filed if travelers are injured or their property is lost or damaged during the screening process at an airport. The case study data was created by concatenating individual TSA airport claims data, removing some extra columns, and then joining the concatenated TSA claims data with the FAA airport facilities data. The TSA Claims 2002 to 2017 CSV file has 14 columns and over 220,000 rows. Claim_Type has a type of the claim. There are 14 valid claim types. The Claim_Site column has where the claim occurred. There are 8 valid values for claims site. The Disposition column has a final settlement of the claim. And so on. All missing and ‘-‘ values in the columns Claim_Type, Claim_Site, and Disposition must be changed to Unknown. The table must include a new column named Date_Issues with a value of Needs Review to
indicate that a row has a date issue. Date issues a missing value for Incident_Date or Date_Received, received date before incident date, format change, etc. These issues needs to be addressed during the data exploration. 

It is necessary to clean the data, as there can be invalid claims, invalid sites,duplicate values, etc. The purpose of the analysis is to answer the business questions such as 
- How many claims per year of Incident_Date are in the overall data?
- What are the frequency values for Claim_Type for the selected state?
- What are the frequency values for Claim_Site for the selected state?
- What are the frequency values for Disposition for the selected state?
- What is the mean, minimum, maximum, and sum of Close_Amount for the selected state?

This will help understand improvement area,make the better policies for the claims settlement, reduce the injuries and losses and improve services. 


### Steps followed 

### Access Data
#### Import the TSAClaims2002_2017.csv file: 
- Create the macro variable 'path' that will point the folder were the required csv file is located. (To access the folder location: right-click on the folder, go to the properties and copy the location.)
- Use a libname statement to create a permanent library(TSA) referencewhose location will be data folder location.   
-  Use the 'options validvarname=v7' option, so that the imported columns will follow SAS naming conventions.
- Use proc import datafile= to import the file. Here the macro variable for the folder location was already created so &path will give req location followed by the name of the csv file. 
- After that use a DBMS= option to specify the type of file, use the out equals option, to determine the table name that we're going to create using the import procedure(TSA.ClaimsImport) followed by replace to overwrite if already exists.
- Use Guessing rows= max. As csv may have multiple missing values, this statement will make SAS to scan all the rows to determine the correct column type. 

Snap of the code to access data:
![Screenshot 2024-06-11 181754](https://github.com/PoojaParab-DA/Projects/assets/172165136/22de31a0-48ec-46bd-9f14-e26eb81f9e43)



### Explore Data

- As mentioned in the problem statement, it is necessary to explore columns Claim_Site, Disposition, Claim_Type, Date_Received, Incident_Date for the missing, - values and format. 
- Run print procedure to get quick preview. 
Table Preview: ![Table Preview](https://github.com/PoojaParab-DA/Projects/assets/172165136/824a2869-df4b-4368-9401-2893dc577c70)
Here we observe the different Date format.
- Run the Contents procedure to look at the descriptor portion of the table. 
Content Preview: ![Content Preview](https://github.com/PoojaParab-DA/Projects/assets/172165136/cd4d9896-2e79-4c6d-8184-cb1ce02082cc)
Here we observe BEST12. format for Date_Received and Incident_Date instead of DATE. Also Close_Amount should be in DOLLAR format.
- Use the Frequency procedure to explore them individual required column. Change the format of date columns. 
Example snap of proc freq table: ![proc freq table](https://github.com/PoojaParab-DA/Projects/assets/172165136/c8ed2e5f-263e-4140-815c-810b2e76dd83)
As we can see in Claim_Site column, 387 '-' and 735 missing values were found which needs to be replaced as 'unknown'. Similar issues were found in other columns as well, which needs to be addressed further during Data Prepare. In the date columns, there were dates before 2002 and after 2017 that needs to be addressed further.
- Run proc print to confirm if Date_Received is always be after the Incident_Date.
Date issues Snap: ![Date issues](https://github.com/PoojaParab-DA/Projects/assets/172165136/a704f3fa-ed7c-4b3d-9003-9ae5e906140f)
An Incident_Date is the date of an incident and the Date_Received is the date the claim was received for that incident. Thus, Date_Received should always be after the Incident_Date. We can see we have an Incident_Date of December 26, 2006, but the Date_Received is February 8, 2006. This is due to the Date issue, that will address further. 

Snap of Code to Explore the data:![Explore Data](https://github.com/PoojaParab-DA/Projects/assets/172165136/bc624ca2-cb24-46e7-8a47-c47b8c9dbc4a)



### Prepare Data
- Remove duplicate rows and Sort the data by ascending Incident_Date.
Code: ![Code to remove duplicates and sort the data](https://github.com/PoojaParab-DA/Projects/assets/172165136/d707a613-25f3-443c-84a5-ccfaa6b872b4)
Here, the NODUPRECS option will remove adjacent rows that are entirely duplicated.and the all keyword will sort the rows by all columns so entirely duplicated rows are adjacent. Total 5 duplicate rows are removed. 

- Clean the Claim_Site, Disposition and Claim_Type column. Convert all State values to uppercase and all StateName values to proper case.
Code: ![Data cleaning 1](https://github.com/PoojaParab-DA/Projects/assets/172165136/6c8c6632-53c4-4001-b29b-891b0b94854c)
Here, the issues such as spelling mistakes are cleared using if else statements. 

- Create a new column to indicate date issues, Add permanent labels and formats, Exclude County and City from the output table.
Code: ![Data cleaning 2](https://github.com/PoojaParab-DA/Projects/assets/172165136/dfd2969a-ee12-4248-bde3-6f56ab7d228d)
Here, Date issues such as dates before and after of 2002 and 2017 resp is addressed along with blanks. Date format is standardized. 

- Perform proc freq to recheck the data cleaning and preparation. 
Code: ![proc freq for cleaned data](https://github.com/PoojaParab-DA/Projects/assets/172165136/28f9985b-2ff2-4764-ae9c-fd51380f3f1b)



### Analyze and Export Data
- Analyze the overall data to answer the business questions. Be sure to add appropriate titles.
- Analyze the state-level data to answer the business questions. Be sure to add appropriate titles.
- Export the end results into a single PDF named ClaimReports that has a style of your choice.
![analyse data part 1](https://github.com/PoojaParab-DA/Projects/assets/172165136/72aeddc7-235c-466b-b10f-030a8412cfaa)
![analys data part 2](https://github.com/PoojaParab-DA/Projects/assets/172165136/fd4ad46e-1e24-4912-8487-cde321b31dc0)
Here, data was analysed to answer the questions and it is exported in pdf file. 

The obtained results are as follows: 
![Analyse result 1](https://github.com/PoojaParab-DA/Projects/assets/172165136/a51ce29f-3b5c-44c9-afbe-dacb2cade6d8)
![Analyse result 2](https://github.com/PoojaParab-DA/Projects/assets/172165136/bda6935c-07a0-495c-960c-cd265e9d661f)
![Analyse result 3](https://github.com/PoojaParab-DA/Projects/assets/172165136/07499cff-d2c3-41d0-886e-59f63a8d81b0)
![Analyse result 4](https://github.com/PoojaParab-DA/Projects/assets/172165136/84577381-7c47-41ec-98ee-38dc31918303)
![Analyse result 5](https://github.com/PoojaParab-DA/Projects/assets/172165136/75ce7aa5-fe38-48cd-8c42-c4fc5443678d)
