
# Project 4

## Part 1 - Selection (30 points)


Identify and describe your dataset, its source, and what appeals to you about it.
Acquire the data and perform an initial exploration to determine which themes
you wish to explore. Describe the questions you want to be able to answer with
the data, any concerns you have about the data, and any challenges you expect
to have to overcome.

**Answer:**

Recently, the tax reduction arose lots of attentions; we are pretty interested in the spending as well as revenue of the US government, normally regarded as one of the governments worldwide which financially press their citizens a lot. As a result, we picked up a dataset, provided by San Francisco Controller's Office, reporting the financial status of a typical US citywide government.

Firstly, we get the dataset from 'DataSF', a website freely providing hundreds of datasets from the City and County of San Francisco, and download the csv file by using the codes shown as follow.

Considering the convenience for typing, we renamed the file as 'spending_and_revenue_sf.csv'.


```python
!wget -O spending_and_revenue_sf.csv "https://data.sfgov.org/api/views/bpnb-jwfb/rows.csv?accessType=DOWNLOAD"
```

    --2017-12-08 16:43:11--  https://data.sfgov.org/api/views/bpnb-jwfb/rows.csv?accessType=DOWNLOAD
    Resolving data.sfgov.org (data.sfgov.org)... 52.206.140.199
    Connecting to data.sfgov.org (data.sfgov.org)|52.206.140.199|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: unspecified [text/csv]
    Saving to: ‘spending_and_revenue_sf.csv’
    
    spending_and_revenu     [ <=>                ] 133.42M  5.38MB/s    in 27s     
    
    Last-modified header invalid -- time-stamp ignored.
    2017-12-08 16:43:38 (4.91 MB/s) - ‘spending_and_revenue_sf.csv’ saved [139903971]
    


We can see that the data size is above 100m, and we want to make sure the dataset is neither too large nor too small to explore further by checking the number of the rows.


```python
!wc -l spending_and_revenue_sf.csv
```

    534310 spending_and_revenue_sf.csv


We see that the original dataset contains around 530k rows, which is appropriate for researching in this project. And we expected that there might be some data issues, e.g., null values, to be discussed and dealt with later.

To initially explore the dataset, we decided to firstly take a look at the column names and see what kinds of information the original data can tell.


```python
!csvcut -n spending_and_revenue_sf.csv
```

      1: Fiscal Year
      2: Revenue or Spending
      3: Related Gov’t Units
      4: Organization Group Code
      5: Organization Group
      6: Department Code
      7: Department
      8: Program Code
      9: Program
     10: Character Code
     11: Character
     12: Object Code
     13: Object
     14: Sub-object Code
     15: Sub-object
     16: Fund Type Code
     17: Fund Type
     18: Fund Code
     19: Fund
     20: Fund Category Code
     21: Fund Category
     22: Amount


Most of the column names clearly indicate the columns' information, while some of them are quite confusing; for instance, so far we have no idea that what the differences are between Fund Type and Fund Category. Accordingly, it is necessary to double check the database website for clarifying the information provided, and the followings are the exact decriptions of each column.

**1. Fiscal Year**

An accounting period of 12 months. The City and County of San Francisco operates on a fiscal year that begins on July 1 and ends on June 30 the following year. The Fiscal Year ending June 30, 2012 is represented as FY2011-2012.


**2. Revenue or Spending**

Revenue or Spending for this record.


**3. Related Gov’t Units**

In SFOpenBook, these are fiduciary funds and component units that are included in the City’s financial statements but are different in nature from the other funds and organizations in City government. Fiduciary Funds include all Trust and Agency Funds that account for assets held by the City as a trustee or as an agency for individuals or other governmental units.


**4. Organization Group Code**

Org Group is a group of Departments. For example, the Public Protection Org Group includes departments such as the Police, Fire, Adult Probation, District Attorney, and Sheriff.


**5. Organization Group**

Org Group is a group of Departments. For example, the Public Protection Org Group includes departments such as the Police, Fire, Adult Probation, District Attorney, and Sheriff.


**6. Department Code**

Departments are the primary organizational unit used by the City and County of San Francisco. Examples include Recreation and Parks, Public Works, and the Police Department.


**7. Department**

Departments are the primary organizational unit used by the City and County of San Francisco. Examples include Recreation and Parks, Public Works, and the Police Department.


**8. Program Code**

A program identifies the services a department provides. For example, the Police Department has programs for Patrol, Investigations, and Administration.


**9. Program**

A program identifies the services a department provides. For example, the Police Department has programs for Patrol, Investigations, and Administration.


**10. Character Code**

In the type hierarchy, Character is the highest level. For example, salaries, benefits, contractual services, and materials & supplies are recorded as different Characters.


**11. Character**

In the type hierarchy, Character is the highest level. For example, salaries, benefits, contractual services, and materials & supplies are recorded as different Characters.


**12. Object Code**

In the type hierarchy, Object is the middle level. For example, within the Salaries Character, Objects differentiate between Permanent Salaries, Temporary Salaries, and Overtime pay.


**13. Object**

In the type hierarchy, Object is the middle level. For example, within the Salaries Character, Objects differentiate between Permanent Salaries, Temporary Salaries, and Overtime pay.


**14. Sub-object Code**

In the type hierarchy, Sub-object is the lowest level of detail. For instance, within the Overtime Object, Sub-object segregates overtime for nurses from overtime for police officers and fire fighters (known as uniformed staff).


**15. Sub-object**

In the type hierarchy, Sub-object is the lowest level of detail. For instance, within the Overtime Object, Sub-object segregates overtime for nurses from overtime for police officers and fire fighters (known as uniformed staff).


**16. Fund Type Code**

In the Fund hierarchy, Fund Type is the highest level, and is used to group Funds according to governmental accounting standards.


**17. Fund Type**

In the Fund hierarchy, Fund Type is the highest level, and is used to group Funds according to governmental accounting standards.


**18. Fund Code**

In the Fund hierarchy, Fund is the middle level. For example, within the Special Revenue Fund Type, you can find the Children’s Fund and the Open Space & Park Fund.


**19. Fund**

In the Fund hierarchy, Fund is the middle level. For example, within the Special Revenue Fund Type, you can find the Children’s Fund and the Open Space & Park Fund.


**20. Fund Category Code**

In the Fund hierarchy, Fund Category is the lowest level. Within Fund, Fund Categories group activity by their characteristics: Operating, Annual Projects, Continuing Projects, Grants, Interdepartmental/Work Order.


**21. Fund Category**

In the Fund hierarchy, Fund Category is the lowest level. Within Fund, Fund Categories group activity by their characteristics: Operating, Annual Projects, Continuing Projects, Grants, Interdepartmental/Work Order.


**22. Amount**

The amount earned (Revenue) or spent (Spending) by the City and County of San Francisco.

Briefly, from the dataset we have all the spending and revenue records of the San Francisco during years, and the records provide information on which fund is involved, why it is used/collected, which department manage it, and what the amount is. Recall that our objective is to figure out how many funds are collected for what kinds of purposes and how the funds are actually used by the governments; under the circumstance, the dataset is supposed to be sufficient for achieveing our research goal.

To explore the data itself further, we use csvstat and focus on the first 10,000 rows.


```python
!head -n 10000 spending_and_revenue_sf.csv | csvstat
```

      1. Fiscal Year
    	<class 'int'>
    	Nulls: False
    	Values: 1999
      2. Revenue or Spending
    	<class 'str'>
    	Nulls: False
    	Values: Spending, Revenue
      3. Related Gov’t Units
    	<class 'bool'>
    	Nulls: False
    	Unique values: 2
    	5 most frequent values:
    		False:	9915
    		True:	84
      4. Organization Group Code
    	<class 'int'>
    	Nulls: False
    	Min: 1
    	Max: 7
    	Sum: 22857
    	Mean: 2.285928592859286
    	Median: 2
    	Standard Deviation: 1.6235198172370684
    	Unique values: 7
    	5 most frequent values:
    		2:	4211
    		1:	3696
    		6:	954
    		4:	481
    		5:	272
      5. Organization Group
    	<class 'str'>
    	Nulls: False
    	Unique values: 7
    	5 most frequent values:
    		Public Works, Transportation & Commerce:	4211
    		Public Protection:	3696
    		General Administration & Finance:	954
    		Community Health:	481
    		Culture & Recreation:	272
    	Max length: 40
      6. Department Code
    	<class 'str'>
    	Nulls: False
    	Unique values: 51
    	5 most frequent values:
    		DPW:	2350
    		POL:	789
    		AIR:	707
    		SHF:	650
    		FIR:	550
    	Max length: 3
      7. Department
    	<class 'str'>
    	Nulls: False
    	Unique values: 50
    	5 most frequent values:
    		DPW GSA - Public Works:	2350
    		POL Police:	789
    		AIR Airport Commission:	707
    		SHF Sheriff:	650
    		FIR Fire Department:	550
    	Max length: 30
      8. Program Code
    	<class 'str'>
    	Nulls: False
    	Unique values: 319
    	5 most frequent values:
    		BAW:	461
    		XXX:	302
    		BAR:	292
    		BGH:	278
    		BAX:	266
    	Max length: 4
      9. Program
    	<class 'str'>
    	Nulls: False
    	Unique values: 299
    	5 most frequent values:
    		City Capital Projects:	461
    		No Program Defined:	402
    		Building Repair And Maintenance:	292
    		Facilities Maintenance,Construction:	278
    		Wastewater Operations:	266
    	Max length: 40
     10. Character Code
    	<class 'str'>
    	Nulls: False
    	Unique values: 33
    	5 most frequent values:
    		521:	1803
    		513:	1559
    		865:	1103
    		501:	1003
    		540:	959
    	Max length: 3
     11. Character
    	<class 'str'>
    	Nulls: False
    	Unique values: 33
    	5 most frequent values:
    		Non Personnel Services:	1803
    		Mandatory Fringe Benefits:	1559
    		Expenditure Recovery:	1103
    		Salaries:	1003
    		Materials & Supplies:	959
    	Max length: 30
     12. Object Code
    	<class 'str'>
    	Nulls: False
    	Unique values: 105
    	5 most frequent values:
    		865:	998
    		581:	644
    		501:	609
    		535:	435
    		515:	395
    	Max length: 3
     13. Object
    	<class 'str'>
    	Nulls: False
    	Unique values: 209
    	5 most frequent values:
    		Recovery for Svcs to AAO Funds:	998
    		Svcs Of Other Depts -AAO Funds:	644
    		Permanent Salaries-Misc:	609
    		Other Current Expenses:	435
    		Health Service:	395
    	Max length: 40
     14. Sub-object Code
    	<class 'str'>
    	Nulls: False
    	Unique values: 1190
    	5 most frequent values:
    		ELIMSC:	199
    		517010:	185
    		501010:	183
    		514020:	181
    		513710:	176
    	Max length: 6
     15. Sub-object
    	<class 'str'>
    	Nulls: False
    	Unique values: 1185
    	5 most frequent values:
    		Transfer Adjustments-Sources (Citywide):	199
    		Unemployment Insurance:	185
    		Perm Salaries-Misc-Regular:	183
    		Social Sec-Medicare(HI Only):	181
    		Dental Coverage:	176
    	Max length: 40
     16. Fund Type Code
    	<class 'str'>
    	Nulls: False
    	Unique values: 19
    	5 most frequent values:
    		1G:	4570
    		2S:	2452
    		5A:	755
    		3C:	546
    		6I:	357
    	Max length: 2
     17. Fund Type
    	<class 'str'>
    	Nulls: False
    	Unique values: 19
    	5 most frequent values:
    		General Fund:	4570
    		Special Revenue Funds:	2452
    		SF International Airport Funds:	755
    		Capital Projects Funds:	546
    		Internal Service Funds:	357
    	Max length: 37
     18. Fund Code
    	<class 'str'>
    	Nulls: False
    	Unique values: 90
    	5 most frequent values:
    		1GAGF:	4554
    		2SPWF:	765
    		2SPPF:	635
    		5AAAA:	633
    		2SBIF:	271
    	Max length: 5
     19. Fund
    	<class 'str'>
    	Nulls: False
    	Unique values: 89
    	5 most frequent values:
    		General Fund:	4554
    		Public Works, Transp. & Commerce Fund:	765
    		Public Protection Fund:	635
    		Airport Operating Fund:	633
    		Building Inspection Fund:	271
    	Max length: 40
     20. Fund Category Code
    	<class 'int'>
    	Nulls: False
    	Values: 1, 2, 3, 4, 5
     21. Fund Category
    	<class 'str'>
    	Nulls: False
    	Values: Work Orders/Overhead, Annual Projects, Operating, Grants, Continuing Projects
     22. Amount
    	<class 'float'>
    	Nulls: False
    	Min: -320300433.26
    	Max: 655301545.96
    	Sum: 7971768132.799989
    	Mean: 797256.5389338923
    	Median: 10192.62
    	Standard Deviation: 13593296.056233019
    	Unique values: 9317
    	5 most frequent values:
    		2000.0:	17
    		25000.0:	16
    		50000.0:	15
    		20000.0:	11
    		150.0:	11
    
    Row count: 9999


**Concerns:**

As shown is above, there are null values in several columns, and we will have to deal with those later. Besides, as mentioned before, for each hierarchy, there are more than one columns providing details; thus we should decide properly whether drop some and keep others for convenience, which is the main challenge facing us.

**Potential Questions:**

There are several questions we will answer based on the dataset:
1. What are the total amount of revenue/spending for each year? What is the trend across 10 years for revenue/spending?
2. Is there a surplus or defict for each year?
3. Which organizations manage the largest amounts of funds?
4. what types of fund are involved in transaction the most?


## Part 2 - Wrangling (35 points)


Based on what you found above, wrangle the data into a format suitable for
analysis. This may involve cleaning, filtering, merging, and modeling steps,
any and all of which are valid for this project. Describe your process as you
proceed, and document any scripts, databases, or other models you develop. Be
specific about any key decisions to modify or remove data, how you overcame
any challenges, and all assumptions you make about the meaning of variables
and their values.

Verify that your wrangling steps have succeeded (for example, if you loaded
the data into a dimensional model, ensure that the fact table contains the right
number of records).

**Answer:**

There are two major parts in the wrangling section: cleaning dataset and building database.

**1. Cleaning dataset **

Our initial plan was to use Trifacta to wrangle data. However, since the dataset is over 100 MB, we were not able to import dataset to Trifacta. We then decision to use pandas for several data cleaning steps. 

We first load the dataset.Check the number of rows.


```python
import pandas as pd
df=pd.read_csv("spending_and_revenue_sf.csv",low_memory=False)
len(df.index)
```




    534309



Considering overall goal of our work, we just want to keep some of the columns, and the followings are main reasons for droping each of the others.

**Related Gov't Units:** According to its introduction, we can see that the information provided by this column is unnecessary for our research.

**Department:** 'Organization Group' and 'Department' both indicate the organizational units, while the former is imformative enough to explore futher with the dataset and the abbreviations in 'Department' column are confusing. As a result, we decide to just keep the 'Organization Group' column.

**Object & Sub-object:** 'Character', 'Object' and 'Sub-object' are all in the type hierarchy with different levels; in other words, they all tell the objective of funds used/collected. However, the dimentions in 'Character' columns are enough to let us analyze and there is no need to keep other two. For example, for the 14th record in the original dataset, it was revenue from Expenditure Recovery indicated by 'Character' column, while it is unnecessary and confusing when we take a look at 'Object' and 'Sub-object' columns, which are telling 'Recovery for Svcs to AAO Funds' and 'Exp Rec Fr Juvenile Court (AAO)', respectively.

**Fund Type:** Like type hierarchy, there are also three columns with different level in the fund hierarchy: 'Fund Type', 'Fund' and 'Fund Category'. Unlike the highest level in the type hierarchy, 'Fund Type' here, the highest level in fund hierarchy, is too general to see in the name of which fund on earth is involved in each spending/revenue record. For instance, for 'Special Revenue Funds' in this column, it infers to 'Public Protection Fund', 'Community Health Services Fund', 'Courts' Fund' or others indicated by 'Fund' column, and the latter is obviously more helpful and easier to understand. As supplementary, 'Fund Category' clearly group activity by their characteristics: Operating, Annual Projects, Continuing Projects, etc.

Besides, we drop all of the code columns cause in some cases the codes get pretty weird and we can replace them with numeric IDs for convenience.

As a result, we extract the columns we need to 'df1' dataframe.We also drop all the rows that contain NA values. Since we have a fairly large dataset, and there are only small amount of rows containing NA, dropping these rows won't have significant effect on the analyses.


```python
df1 = df[['Fiscal Year','Revenue or Spending','Organization Group','Program','Character','Fund','Fund Category','Amount']]
df2 = df1.dropna(how='any',axis=0) 
len(df2.index)
```




    530789



To take a look at the total rows for ensuring that the new dataframe contains the right number of records.

Since we only study data that was recorded from 2008 to 2017, we subset all the rows that recorded spending and revenue in these 10 years.


```python
df3=df2.loc[df['Fiscal Year'].isin(['2008','2009','2010','2011','2012','2013','2014','2015','2016','2017'])]
len(df3.index)
```




    289429



The final dataset we are going to use to build the database contains approximate 290k rows, which is satisfying for us to do some analyses in this project.

To write the dataframe to csv file for submission purpose.


```python
df3.to_csv('newdataset.csv')
```

**2.Building database **

To connect to a new database: 'proj4'.


```python
!dropdb -U student proj4
```


```python
!createdb -U student proj4
```


```python
%load_ext sql
```


```python
%sql postgresql://student@/proj4
```




    'Connected: student@proj4'



We design the following schema for the dataset. It contains one fact table and five dimension tables. Here are the demostration of the six tables. 


```python
from IPython.display import Image
Image(url='https://s3.amazonaws.com/dmfa-2017fall/Star-Schema.png')
```




<img src="https://s3.amazonaws.com/dmfa-2017fall/Star-Schema.png"/>



We start with populating a temporary fact table.

To create a temporary fact table to load all the data.


```python
%%sql

DROP TABLE IF EXISTS fact;

CREATE TABLE fact (
    fact_id INTEGER,
    fiscal_year INTEGER,
    revenue_spending VARCHAR(10),
    organization_group VARCHAR(50),
    program VARCHAR(50),
    character VARCHAR(50),
    fund VARCHAR(50),
    fund_category VARCHAR(50),
    amount NUMERIC(20,2)    
);
```

    Done.
    Done.





    []



To populate the temorary fact table, we use the absolute path here.


```python
!pwd
```

    /home/ubuntu



```python
%%sql
COPY fact FROM '/home/ubuntu/newdataset.csv'
CSV
HEADER;
```

    289429 rows affected.





    []



To verify data was loaded successfully to the temporary fact table.


```python
%%sql
SELECT *
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fact_id</th>
        <th>fiscal_year</th>
        <th>revenue_spending</th>
        <th>organization_group</th>
        <th>program</th>
        <th>character</th>
        <th>fund</th>
        <th>fund_category</th>
        <th>amount</th>
    </tr>
    <tr>
        <td>205140</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Water Operating Fund</td>
        <td>Continuing Projects</td>
        <td>171.26</td>
    </tr>
    <tr>
        <td>205141</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Annual Projects</td>
        <td>-45627.76</td>
    </tr>
    <tr>
        <td>205142</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Continuing Projects</td>
        <td>-50385.50</td>
    </tr>
    <tr>
        <td>205143</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Parking &amp; Traffic Gasoline Tax Fund</td>
        <td>Continuing Projects</td>
        <td>225822.47</td>
    </tr>
    <tr>
        <td>205144</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Public Works, Transp. &amp; Commerce Fund</td>
        <td>Continuing Projects</td>
        <td>1672.97</td>
    </tr>
    <tr>
        <td>205145</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Public Works, Transp. &amp; Commerce Fund</td>
        <td>Work Orders/Overhead</td>
        <td>17844.66</td>
    </tr>
    <tr>
        <td>205146</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Earthquake Safety Improvements Fund</td>
        <td>Continuing Projects</td>
        <td>10081.92</td>
    </tr>
    <tr>
        <td>205147</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Fire Protection Systems Impvt. Fund</td>
        <td>Continuing Projects</td>
        <td>583.72</td>
    </tr>
    <tr>
        <td>205148</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Recreation &amp; Park Capital Impvts Fund</td>
        <td>Continuing Projects</td>
        <td>-1628.71</td>
    </tr>
    <tr>
        <td>205149</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Recreation &amp; Park Capital Impvts Fund</td>
        <td>Grants</td>
        <td>55651.28</td>
    </tr>
</table>



#### Next, we populate the `RS` table.

To extract RS-related dimension detail.


```python
%%sql
SELECT DISTINCT revenue_spending
FROM fact;
```

    2 rows affected.





<table>
    <tr>
        <th>revenue_spending</th>
    </tr>
    <tr>
        <td>Spending</td>
    </tr>
    <tr>
        <td>Revenue</td>
    </tr>
</table>



To create `RS` table.


```python
%%sql

DROP TABLE IF EXISTS RS;

CREATE TABLE RS (
    RS_id SERIAL PRIMARY KEY,
    type VARCHAR(10) NOT NULL
);
```

    Done.
    Done.





    []



To populate the dimension table with unique values of these dimensions from the dataset.


```python
%%sql
INSERT INTO RS (type)
SELECT DISTINCT revenue_spending
FROM fact;
```

    2 rows affected.





    []



We add a foreign key column to the fact table that references `RS` dimension table.


```python
%%sql
ALTER TABLE fact
ADD COLUMN RS_id INTEGER,
ADD CONSTRAINT fk_RS_id
    FOREIGN KEY (RS_id)
    REFERENCES RS (RS_id);
```

    Done.





    []



To create an index on all columns in `RS` table.


```python
%%sql
DROP INDEX IF EXISTS idx_RS;

CREATE INDEX idx_RS ON RS (type);
```

    Done.
    Done.





    []




```python
%%sql
UPDATE fact
SET RS_id = RS.RS_id
FROM RS
WHERE fact.revenue_spending = RS.type;
```

    289429 rows affected.





    []



To see if we successfully populate the foreign key column in the fact table.


```python
%%sql
SELECT *
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fact_id</th>
        <th>fiscal_year</th>
        <th>revenue_spending</th>
        <th>organization_group</th>
        <th>program</th>
        <th>character</th>
        <th>fund</th>
        <th>fund_category</th>
        <th>amount</th>
        <th>rs_id</th>
    </tr>
    <tr>
        <td>217214</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Non Personnel Services</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>882.50</td>
        <td>1</td>
    </tr>
    <tr>
        <td>229911</td>
        <td>2008</td>
        <td>Revenue</td>
        <td>Public Protection</td>
        <td>Fire Nert</td>
        <td>Other Revenues</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>9975.00</td>
        <td>2</td>
    </tr>
    <tr>
        <td>232203</td>
        <td>2008</td>
        <td>Revenue</td>
        <td>Culture &amp; Recreation</td>
        <td>Branch Program</td>
        <td>Other Revenues</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>4170.00</td>
        <td>2</td>
    </tr>
    <tr>
        <td>263962</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Security Services</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>366.85</td>
        <td>1</td>
    </tr>
    <tr>
        <td>285803</td>
        <td>2010</td>
        <td>Revenue</td>
        <td>Public Protection</td>
        <td>Grant Services</td>
        <td>Other Revenues</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>3666.00</td>
        <td>2</td>
    </tr>
    <tr>
        <td>290596</td>
        <td>2010</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Emergency Services</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>-1365.00</td>
        <td>1</td>
    </tr>
    <tr>
        <td>291312</td>
        <td>2010</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Log Cabin Ranch</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>1140649.97</td>
        <td>1</td>
    </tr>
    <tr>
        <td>308471</td>
        <td>2010</td>
        <td>Spending</td>
        <td>Culture &amp; Recreation</td>
        <td>Golden Gate Park</td>
        <td>Salaries</td>
        <td>Bequests Fund</td>
        <td>Grants</td>
        <td>12626.00</td>
        <td>1</td>
    </tr>
    <tr>
        <td>335828</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Culture &amp; Recreation</td>
        <td>Public Art</td>
        <td>Salaries</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>830.99</td>
        <td>1</td>
    </tr>
    <tr>
        <td>342642</td>
        <td>2012</td>
        <td>Revenue</td>
        <td>Public Protection</td>
        <td>Fire General</td>
        <td>Other Revenues</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>5380.53</td>
        <td>2</td>
    </tr>
</table>



End of populating `RS` table.

#### Next, we populate the `fund` table.

To extract fund-related dimension details.


```python
%%sql
SELECT DISTINCT Fund, Fund_Category
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fund</th>
        <th>fund_category</th>
    </tr>
    <tr>
        <td>Other Agency Fund</td>
        <td>Operating</td>
    </tr>
    <tr>
        <td>Courts&#x27; Fund</td>
        <td>Annual Projects</td>
    </tr>
    <tr>
        <td>Police Dept. Facilities Impvt. Fund</td>
        <td>Grants</td>
    </tr>
    <tr>
        <td>Real Property Fund</td>
        <td>Continuing Projects</td>
    </tr>
    <tr>
        <td>School District Agency Fund</td>
        <td>Operating</td>
    </tr>
    <tr>
        <td>Parking Off Street Parking Oper Fund</td>
        <td>Operating</td>
    </tr>
    <tr>
        <td>SFCTA Capital Project Fund</td>
        <td>Continuing Projects</td>
    </tr>
    <tr>
        <td>San Francisco Finance Corporation</td>
        <td>Continuing Projects</td>
    </tr>
    <tr>
        <td>Transportation &amp; Commerce Fund</td>
        <td>Grants</td>
    </tr>
    <tr>
        <td>Port Capital Projects Fund</td>
        <td>Continuing Projects</td>
    </tr>
</table>



To create `fund` table.


```python
%%sql

DROP TABLE IF EXISTS fund;

CREATE TABLE fund (
    fund_id SERIAL PRIMARY KEY,
    fund VARCHAR(50) NOT NULL,
    fund_category VARCHAR(50) NOT NULL
);
```

    Done.
    Done.





    []



To populate the dimension table with unique values of these dimensions from the dataset.


```python
%%sql
INSERT INTO fund (fund,fund_category)
SELECT DISTINCT Fund, Fund_Category
FROM fact;
```

    246 rows affected.





    []



We add a foreign key column to the fact table that references `fund` dimension table.


```python
%%sql
ALTER TABLE fact
ADD COLUMN fund_id INTEGER,
ADD CONSTRAINT fk_fund_id
    FOREIGN KEY (fund_id)
    REFERENCES fund (fund_id);
```

    Done.





    []




```python
%%sql
SELECT fund_id FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fund_id</th>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
    <tr>
        <td>None</td>
    </tr>
</table>



To create an index on all columns in `fund` table.


```python
%%sql
DROP INDEX IF EXISTS idx_fund;

CREATE INDEX idx_fund ON fund (fund,fund_category);
```

    Done.
    Done.





    []




```python
%%sql
UPDATE fact
SET fund_id = fund.fund_id
FROM fund
WHERE fact.Fund = fund.fund
    AND fact.Fund_Category = fund.fund_category;
```

    289429 rows affected.





    []



To see if we successfully populate the foreign key column in `fact` table.


```python
%%sql
SELECT *
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fact_id</th>
        <th>fiscal_year</th>
        <th>revenue_spending</th>
        <th>organization_group</th>
        <th>program</th>
        <th>character</th>
        <th>fund</th>
        <th>fund_category</th>
        <th>amount</th>
        <th>rs_id</th>
        <th>fund_id</th>
    </tr>
    <tr>
        <td>407341</td>
        <td>2014</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Sheriff Programs</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>625.00</td>
        <td>1</td>
        <td>161</td>
    </tr>
    <tr>
        <td>423881</td>
        <td>2014</td>
        <td>Spending</td>
        <td>Culture &amp; Recreation</td>
        <td>Golden Gate Park</td>
        <td>Salaries</td>
        <td>Bequests Fund</td>
        <td>Grants</td>
        <td>2469.74</td>
        <td>1</td>
        <td>154</td>
    </tr>
    <tr>
        <td>424113</td>
        <td>2014</td>
        <td>Spending</td>
        <td>Culture &amp; Recreation</td>
        <td>Parks</td>
        <td>Non Personnel Services</td>
        <td>Golf Fund</td>
        <td>Operating</td>
        <td>2360.00</td>
        <td>1</td>
        <td>86</td>
    </tr>
    <tr>
        <td>424999</td>
        <td>2014</td>
        <td>Spending</td>
        <td>Culture &amp; Recreation</td>
        <td>Rec &amp; Park Administration</td>
        <td>Materials &amp; Supplies</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>209.98</td>
        <td>1</td>
        <td>230</td>
    </tr>
    <tr>
        <td>425105</td>
        <td>2014</td>
        <td>Spending</td>
        <td>Culture &amp; Recreation</td>
        <td>Academy Of Sciences</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>806707.73</td>
        <td>1</td>
        <td>161</td>
    </tr>
    <tr>
        <td>426881</td>
        <td>2014</td>
        <td>Spending</td>
        <td>General Administration &amp; Finance</td>
        <td>Legal Service</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>1693730.87</td>
        <td>1</td>
        <td>161</td>
    </tr>
    <tr>
        <td>427442</td>
        <td>2014</td>
        <td>Spending</td>
        <td>General Administration &amp; Finance</td>
        <td>Current Planning</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>4093224.97</td>
        <td>1</td>
        <td>161</td>
    </tr>
    <tr>
        <td>433234</td>
        <td>2015</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Log Cabin Ranch</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>54009.88</td>
        <td>1</td>
        <td>161</td>
    </tr>
    <tr>
        <td>467196</td>
        <td>2016</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Security Services</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>3699.80</td>
        <td>1</td>
        <td>161</td>
    </tr>
    <tr>
        <td>468130</td>
        <td>2017</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Non Personnel Services</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>150.00</td>
        <td>1</td>
        <td>230</td>
    </tr>
</table>



#### End of populating `fund` table.

#### Next, we populate `character` table.

To extract character-related dimension details.


```python
%%sql
SELECT DISTINCT character
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>character</th>
    </tr>
    <tr>
        <td>Licenses; Permits &amp; Franchises</td>
    </tr>
    <tr>
        <td>Expenditure Recovery</td>
    </tr>
    <tr>
        <td>Other Local Taxes</td>
    </tr>
    <tr>
        <td>Other Support&amp;Care Of Persons</td>
    </tr>
    <tr>
        <td>Transfer Adjustments-Sources</td>
    </tr>
    <tr>
        <td>Business Taxes</td>
    </tr>
    <tr>
        <td>Intrafund Transfers Out</td>
    </tr>
    <tr>
        <td>Other Expenses-Non Expend Type</td>
    </tr>
    <tr>
        <td>Aid Assistance</td>
    </tr>
    <tr>
        <td>Other Financing Sources</td>
    </tr>
</table>



To create `character` table.


```python
%%sql

DROP TABLE IF EXISTS character;

CREATE TABLE character (
    character_id SERIAL PRIMARY KEY,
    character VARCHAR(50) NOT NULL
);
```

    Done.
    Done.





    []



To populate the dimension table with unique values of dimension from the dataset.


```python
%%sql
INSERT INTO character (character)
SELECT DISTINCT character
FROM fact;
```

    46 rows affected.





    []



We add a foreign key column to the fact table that references character dimension table.


```python
%%sql
ALTER TABLE fact
ADD COLUMN character_id INTEGER,
ADD CONSTRAINT fk_character_id
    FOREIGN KEY (character_id)
    REFERENCES character (character_id);
```

    Done.





    []



To create an index on all columns in `character` table.


```python
%%sql
DROP INDEX IF EXISTS idx_character;

CREATE INDEX idx_character ON character (character);
```

    Done.
    Done.





    []




```python
%%sql
UPDATE fact
SET character_id = character.character_id
FROM character
WHERE fact.character = character.character;
```

    289429 rows affected.





    []



To see if we successfully populate the foreign key column in fact table.


```python
%%sql
SELECT *
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fact_id</th>
        <th>fiscal_year</th>
        <th>revenue_spending</th>
        <th>organization_group</th>
        <th>program</th>
        <th>character</th>
        <th>fund</th>
        <th>fund_category</th>
        <th>amount</th>
        <th>rs_id</th>
        <th>fund_id</th>
        <th>character_id</th>
    </tr>
    <tr>
        <td>468130</td>
        <td>2017</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Non Personnel Services</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>150.00</td>
        <td>1</td>
        <td>230</td>
        <td>42</td>
    </tr>
    <tr>
        <td>481547</td>
        <td>2016</td>
        <td>Spending</td>
        <td>Community Health</td>
        <td>Sfhn-Managed Care</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>6714.40</td>
        <td>1</td>
        <td>161</td>
        <td>14</td>
    </tr>
    <tr>
        <td>235567</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Grant Services</td>
        <td>Salaries</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>2466.08</td>
        <td>1</td>
        <td>230</td>
        <td>14</td>
    </tr>
    <tr>
        <td>235568</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Grant Services</td>
        <td>Salaries</td>
        <td>Public Protection Fund</td>
        <td>Grants</td>
        <td>4099.39</td>
        <td>1</td>
        <td>105</td>
        <td>14</td>
    </tr>
    <tr>
        <td>515919</td>
        <td>2017</td>
        <td>Spending</td>
        <td>Culture &amp; Recreation</td>
        <td>Law Library</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>246239.09</td>
        <td>1</td>
        <td>161</td>
        <td>14</td>
    </tr>
    <tr>
        <td>235569</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Grant Services</td>
        <td>Salaries</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>2385.02</td>
        <td>1</td>
        <td>230</td>
        <td>14</td>
    </tr>
    <tr>
        <td>235570</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Grant Services</td>
        <td>Salaries</td>
        <td>Public Protection Fund</td>
        <td>Grants</td>
        <td>3209.80</td>
        <td>1</td>
        <td>105</td>
        <td>14</td>
    </tr>
    <tr>
        <td>235571</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Grant Services</td>
        <td>Salaries</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>3340.50</td>
        <td>1</td>
        <td>230</td>
        <td>14</td>
    </tr>
    <tr>
        <td>235572</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Grant Services</td>
        <td>Salaries</td>
        <td>Public Protection Fund</td>
        <td>Grants</td>
        <td>625.00</td>
        <td>1</td>
        <td>105</td>
        <td>14</td>
    </tr>
    <tr>
        <td>229662</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Community Health</td>
        <td>Health At Home</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>277858.70</td>
        <td>1</td>
        <td>161</td>
        <td>14</td>
    </tr>
</table>



#### End of populating `character` table.

#### Next, we populate `organization` table.

To extract organization-related dimension details.


```python
%%sql
SELECT DISTINCT organization_group
FROM fact;
```

    7 rows affected.





<table>
    <tr>
        <th>organization_group</th>
    </tr>
    <tr>
        <td>Human Welfare &amp; Neighborhood Development</td>
    </tr>
    <tr>
        <td>Community Health</td>
    </tr>
    <tr>
        <td>General City Responsibilities</td>
    </tr>
    <tr>
        <td>Public Protection</td>
    </tr>
    <tr>
        <td>Culture &amp; Recreation</td>
    </tr>
    <tr>
        <td>Public Works, Transportation &amp; Commerce</td>
    </tr>
    <tr>
        <td>General Administration &amp; Finance</td>
    </tr>
</table>



To create `organization` table. 


```python
%%sql

DROP TABLE IF EXISTS organization;

CREATE TABLE organization (
    organization_id SERIAL PRIMARY KEY,
    organization VARCHAR(50) NOT NULL
);
```

    Done.
    Done.





    []



To populate the dimension table with unique values of dimension from the dataset.


```python
%%sql
INSERT INTO organization (organization)
SELECT DISTINCT organization_group
FROM fact;
```

    7 rows affected.





    []



We add a foreign key column to the fact table that references organization dimension table.


```python
%%sql
ALTER TABLE fact
ADD COLUMN organization_id INTEGER,
ADD CONSTRAINT fk_organization_id
    FOREIGN KEY (organization_id)
    REFERENCES organization (organization_id);
```

    Done.





    []



To create an index on all columns in `organization` table.


```python
%%sql
DROP INDEX IF EXISTS idx_organization;

CREATE INDEX idx_organization ON organization (organization);
```

    Done.
    Done.





    []




```python
%%sql
UPDATE fact
SET organization_id = organization.organization_id
FROM organization
WHERE fact.organization_group = organization.organization;
```

    289429 rows affected.





    []



To see if we successfully populate the foreign key column in fact table.


```python
%%sql
SELECT *
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fact_id</th>
        <th>fiscal_year</th>
        <th>revenue_spending</th>
        <th>organization_group</th>
        <th>program</th>
        <th>character</th>
        <th>fund</th>
        <th>fund_category</th>
        <th>amount</th>
        <th>rs_id</th>
        <th>fund_id</th>
        <th>character_id</th>
        <th>organization_id</th>
    </tr>
    <tr>
        <td>229662</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Community Health</td>
        <td>Health At Home</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>277858.70</td>
        <td>1</td>
        <td>161</td>
        <td>14</td>
        <td>2</td>
    </tr>
    <tr>
        <td>229947</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Community Health</td>
        <td>Health At Home</td>
        <td>Salaries</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>346845.67</td>
        <td>1</td>
        <td>161</td>
        <td>14</td>
        <td>2</td>
    </tr>
    <tr>
        <td>235745</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Investigations</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>2202.00</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
    </tr>
    <tr>
        <td>236070</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>324.00</td>
        <td>1</td>
        <td>161</td>
        <td>22</td>
        <td>4</td>
    </tr>
    <tr>
        <td>236071</td>
        <td>2008</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>15701.53</td>
        <td>1</td>
        <td>161</td>
        <td>22</td>
        <td>4</td>
    </tr>
    <tr>
        <td>263649</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Non Personnel Services</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>339501.39</td>
        <td>1</td>
        <td>161</td>
        <td>42</td>
        <td>4</td>
    </tr>
    <tr>
        <td>263650</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Non Personnel Services</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>688103.90</td>
        <td>1</td>
        <td>161</td>
        <td>42</td>
        <td>4</td>
    </tr>
    <tr>
        <td>263651</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>City Grant Programs</td>
        <td>Public Protection Fund</td>
        <td>Grants</td>
        <td>40000.00</td>
        <td>1</td>
        <td>105</td>
        <td>26</td>
        <td>4</td>
    </tr>
    <tr>
        <td>263652</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>12173.44</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
    </tr>
    <tr>
        <td>263653</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>3345.05</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
    </tr>
</table>



#### End of populating `organization` table.

#### Next, we populate `program` table.

To extract program-related dimension details.


```python
%%sql
SELECT DISTINCT program
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>program</th>
    </tr>
    <tr>
        <td>Health At Home</td>
    </tr>
    <tr>
        <td>Fire Nert</td>
    </tr>
    <tr>
        <td>Non-Grant Construction Projects</td>
    </tr>
    <tr>
        <td>Fire Prevention</td>
    </tr>
    <tr>
        <td>Refugee Resettlement Program</td>
    </tr>
    <tr>
        <td>Laguna Honda Hosp - Acute Care</td>
    </tr>
    <tr>
        <td>Medi-Cal</td>
    </tr>
    <tr>
        <td>Muni-General Management</td>
    </tr>
    <tr>
        <td>Departmental Administration</td>
    </tr>
    <tr>
        <td>Information Technology</td>
    </tr>
</table>



To create `program` table.


```python
%%sql

DROP TABLE IF EXISTS program;

CREATE TABLE program (
    program_id SERIAL PRIMARY KEY,
    program VARCHAR(50)
);
```

    Done.
    Done.





    []



To populate the dimension table with unique values of dimension from the dataset.


```python
%%sql
INSERT INTO program (program)
SELECT DISTINCT program 
FROM fact;
```

    443 rows affected.





    []



We add a foreign key column to the fact table that references program dimension table.


```python
%%sql
ALTER TABLE fact
ADD COLUMN program_id INTEGER,
ADD CONSTRAINT fk_program_id
    FOREIGN KEY (program_id)
    REFERENCES program (program_id);
```

    Done.





    []



To create an index on all columns in `program` table.


```python
%%sql
DROP INDEX IF EXISTS idx_program;

CREATE INDEX idx_program ON program (program);
```

    Done.
    Done.





    []




```python
%%sql
UPDATE fact
SET program_id = program.program_id
FROM program
WHERE fact.program = program.program;
```

    289429 rows affected.





    []



To see if we successfully populate the foreign key column in fact table.


```python
%%sql
SELECT *
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fact_id</th>
        <th>fiscal_year</th>
        <th>revenue_spending</th>
        <th>organization_group</th>
        <th>program</th>
        <th>character</th>
        <th>fund</th>
        <th>fund_category</th>
        <th>amount</th>
        <th>rs_id</th>
        <th>fund_id</th>
        <th>character_id</th>
        <th>organization_id</th>
        <th>program_id</th>
    </tr>
    <tr>
        <td>263653</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>3345.05</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>263654</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>62.28</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>263655</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>138.52</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>263656</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>40622.33</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>263657</td>
        <td>2009</td>
        <td>Spending</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Materials &amp; Supplies</td>
        <td>General Fund</td>
        <td>Operating</td>
        <td>161.11</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>205140</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Water Operating Fund</td>
        <td>Continuing Projects</td>
        <td>171.26</td>
        <td>1</td>
        <td>14</td>
        <td>22</td>
        <td>6</td>
        <td>91</td>
    </tr>
    <tr>
        <td>205141</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Annual Projects</td>
        <td>-45627.76</td>
        <td>1</td>
        <td>175</td>
        <td>22</td>
        <td>6</td>
        <td>91</td>
    </tr>
    <tr>
        <td>205142</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Continuing Projects</td>
        <td>-50385.50</td>
        <td>1</td>
        <td>223</td>
        <td>22</td>
        <td>6</td>
        <td>91</td>
    </tr>
    <tr>
        <td>205147</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Fire Protection Systems Impvt. Fund</td>
        <td>Continuing Projects</td>
        <td>583.72</td>
        <td>1</td>
        <td>132</td>
        <td>22</td>
        <td>6</td>
        <td>91</td>
    </tr>
    <tr>
        <td>230025</td>
        <td>2008</td>
        <td>Revenue</td>
        <td>Public Protection</td>
        <td>Patrol</td>
        <td>Other Revenues</td>
        <td>Gift Fund</td>
        <td>Grants</td>
        <td>5845.00</td>
        <td>2</td>
        <td>230</td>
        <td>37</td>
        <td>4</td>
        <td>319</td>
    </tr>
</table>



#### End of populating `program` table.

We can now remove all columns in fact table that are not facts nor foreign keys of dimension tables.


```python
%%sql
ALTER TABLE fact
DROP COLUMN fact_id,
DROP COLUMN revenue_spending,
DROP COLUMN organization_group,
DROP COLUMN program,
DROP COLUMN character,
DROP COLUMN fund,
DROP COLUMN fund_category;
```

    Done.





    []




```python
%%sql
SELECT *
FROM fact
LIMIT 10;
```

    10 rows affected.





<table>
    <tr>
        <th>fiscal_year</th>
        <th>amount</th>
        <th>rs_id</th>
        <th>fund_id</th>
        <th>character_id</th>
        <th>organization_id</th>
        <th>program_id</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>5845.00</td>
        <td>2</td>
        <td>230</td>
        <td>37</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>2008</td>
        <td>864203.78</td>
        <td>1</td>
        <td>161</td>
        <td>14</td>
        <td>2</td>
        <td>271</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>34257.11</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>4</td>
        <td>69</td>
    </tr>
    <tr>
        <td>2008</td>
        <td>3850.00</td>
        <td>1</td>
        <td>193</td>
        <td>44</td>
        <td>5</td>
        <td>32</td>
    </tr>
    <tr>
        <td>2008</td>
        <td>8817.17</td>
        <td>1</td>
        <td>161</td>
        <td>44</td>
        <td>5</td>
        <td>137</td>
    </tr>
    <tr>
        <td>2008</td>
        <td>905.92</td>
        <td>1</td>
        <td>161</td>
        <td>42</td>
        <td>7</td>
        <td>69</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>114539.16</td>
        <td>1</td>
        <td>105</td>
        <td>14</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>33690.67</td>
        <td>1</td>
        <td>53</td>
        <td>14</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>14807598.40</td>
        <td>1</td>
        <td>161</td>
        <td>14</td>
        <td>4</td>
        <td>319</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>201492.70</td>
        <td>2</td>
        <td>161</td>
        <td>2</td>
        <td>4</td>
        <td>230</td>
    </tr>
</table>



To check the number of row matches that of the wrangled dataframe. 


```python
%%sql
SELECT COUNT(*) 
FROM fact;
```

    1 rows affected.





<table>
    <tr>
        <th>count</th>
    </tr>
    <tr>
        <td>289429</td>
    </tr>
</table>



Thus we are sure that we have the correct fact table.

## Part 3 - Analysis (35 points)

Explore and analyze your data in its wrangled form. Follow through on the
themes you identified in Part 1 with queries or scripts that answer the questions
you had in mind. Be clear about the answers you discover, discussing them and
whether the results match your expectations. Include charts or other visuals
that support your analysis. You may use Tableau, ggplot, or other tools we
have not covered in class for visualization, but be sure to export images and to
include them properly in your writeup.


**Answer:**

We firstly import some necessary Python modules.


```python
import numpy as np
import matplotlib
import matplotlib.pyplot as plt
%matplotlib inline
```

To see the total amount of revenue each year.


```python
%%sql
SELECT fact.fiscal_year, sum(fact.amount) as revenue_amount
FROM fact
JOIN RS
ON fact.rs_id=RS.rs_id
WHERE RS.type='Revenue'
GROUP BY fact.fiscal_year
ORDER BY fact.fiscal_year;
```

    10 rows affected.





<table>
    <tr>
        <th>fiscal_year</th>
        <th>revenue_amount</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>8332812399.38</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>9113646360.95</td>
    </tr>
    <tr>
        <td>2010</td>
        <td>11041315212.53</td>
    </tr>
    <tr>
        <td>2011</td>
        <td>10559553014.76</td>
    </tr>
    <tr>
        <td>2012</td>
        <td>11955234445.83</td>
    </tr>
    <tr>
        <td>2013</td>
        <td>12272215852.43</td>
    </tr>
    <tr>
        <td>2014</td>
        <td>12103140489.91</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>12926845611.19</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>13790489689.38</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>13929932358.14</td>
    </tr>
</table>




```python
df=_.DataFrame()
```


```python
df=df.astype(float)
df.plot(x='fiscal_year', y='revenue_amount', style='o',linestyle='-')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fbcc0ba0908>




![png](output_137_1.png)


We can clearly see that the revenue underwent an steady increase in last ten years, despite several drops. And the revenue in 2017 (FY2016-2017) is twice as much as that in 2008 (FY2007-2008).

Similarly, we want to see the trend of the annual amounts of spending during last ten years.


```python
%%sql
SELECT fact.fiscal_year, sum(fact.amount) as spend_amount
FROM fact
JOIN RS
ON fact.rs_id=RS.rs_id
WHERE RS.type='Spending'
GROUP BY fact.fiscal_year
ORDER BY fact.fiscal_year;
```

    10 rows affected.





<table>
    <tr>
        <th>fiscal_year</th>
        <th>spend_amount</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>8320554906.14</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>8616502486.82</td>
    </tr>
    <tr>
        <td>2010</td>
        <td>9185963929.98</td>
    </tr>
    <tr>
        <td>2011</td>
        <td>9738243905.15</td>
    </tr>
    <tr>
        <td>2012</td>
        <td>10465877639.99</td>
    </tr>
    <tr>
        <td>2013</td>
        <td>11015995873.07</td>
    </tr>
    <tr>
        <td>2014</td>
        <td>11712598141.50</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>11610478035.03</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>12622077979.58</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>12617090257.71</td>
    </tr>
</table>




```python
df=_.DataFrame()
```


```python
df=df.astype(float)
df.plot(x='fiscal_year', y='spend_amount', style='o',linestyle='-')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fbcbead2c50>




![png](output_142_1.png)


Clearly, amounts of spending these years share similar upward trend with that of revenue. It is reasonable that with more revenue, the government then has a looser budget. Also, the amount of spending increases fairly constantly each year. 

Now, it is time to check the financial status, i.e., surplus or deficit, of the government each year.


```python
%%sql
SELECT Revenue.fiscal_year, (Revenue.revenue_amount - Spending.spend_amount) as net_revenue
FROM (SELECT fact.fiscal_year as fiscal_year, sum(fact.amount) as revenue_amount
      FROM fact
      JOIN RS
      ON fact.rs_id=RS.rs_id
      WHERE RS.type='Revenue'
      GROUP BY fact.fiscal_year
      ORDER BY fact.fiscal_year) as Revenue
JOIN (SELECT fact.fiscal_year as fiscal_year, sum(fact.amount) as spend_amount
      FROM fact
      JOIN RS
      ON fact.rs_id=RS.rs_id
      WHERE RS.type='Spending'
      GROUP BY fact.fiscal_year
      ORDER BY fact.fiscal_year) as Spending
ON Revenue.fiscal_year=Spending.fiscal_year;
```

    10 rows affected.





<table>
    <tr>
        <th>fiscal_year</th>
        <th>net_revenue</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>12257493.24</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>497143874.13</td>
    </tr>
    <tr>
        <td>2010</td>
        <td>1855351282.55</td>
    </tr>
    <tr>
        <td>2011</td>
        <td>821309109.61</td>
    </tr>
    <tr>
        <td>2012</td>
        <td>1489356805.84</td>
    </tr>
    <tr>
        <td>2013</td>
        <td>1256219979.36</td>
    </tr>
    <tr>
        <td>2014</td>
        <td>390542348.41</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>1316367576.16</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>1168411709.80</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>1312842100.43</td>
    </tr>
</table>




```python
df=_.DataFrame()
```


```python
df=df.astype(float)
df.plot(x='fiscal_year', y='net_revenue', style='o',linestyle='-')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fbcbeab71d0>




![png](output_147_1.png)


From the query result and the plot, we see that in the past ten years, the annual net revenues of the government are all greater than zero, which means there does exist surplus each year in San Francisco governmental financial system.

**Next, we want to figure out which organizations manage the largest amounts of funds. Here we focus on the data in the last three years.**


```python
%%sql
WITH orgrev_2015 AS (
SELECT fact.fiscal_year,organization.organization,sum(fact.amount) as revenue_total
FROM fact 
JOIN organization ON organization.organization_id=fact.organization_id
JOIN RS ON fact.rs_id=RS.rs_id
WHERE RS.type='Revenue' and fact.fiscal_year='2015'
GROUP BY fact.fiscal_year,organization.organization
ORDER BY revenue_total DESC),
orgrev_2016 AS (
SELECT fact.fiscal_year,organization.organization,sum(fact.amount) as revenue_total
FROM fact 
JOIN organization ON organization.organization_id=fact.organization_id
JOIN RS ON fact.rs_id=RS.rs_id
WHERE RS.type='Revenue' and fact.fiscal_year='2016'
GROUP BY fact.fiscal_year,organization.organization
ORDER BY revenue_total DESC),
orgrev_2017 AS (
SELECT fact.fiscal_year,organization.organization,sum(fact.amount) as revenue_total
FROM fact 
JOIN organization ON organization.organization_id=fact.organization_id
JOIN RS ON fact.rs_id=RS.rs_id
WHERE RS.type='Revenue' and fact.fiscal_year='2017'
GROUP BY fact.fiscal_year,organization.organization
ORDER BY revenue_total DESC
), total_orgrev AS(
SELECT*FROM orgrev_2015
UNION
SELECT*FROM orgrev_2016
UNION
SELECT*FROM orgrev_2017)
SELECT*FROM total_orgrev
ORDER BY fiscal_year;
```

    21 rows affected.





<table>
    <tr>
        <th>fiscal_year</th>
        <th>organization</th>
        <th>revenue_total</th>
    </tr>
    <tr>
        <td>2015</td>
        <td>Culture &amp; Recreation</td>
        <td>344343515.77</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Human Welfare &amp; Neighborhood Development</td>
        <td>1406510829.14</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>3868374791.90</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Community Health</td>
        <td>1596564113.56</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>General Administration &amp; Finance</td>
        <td>2788413937.99</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Public Protection</td>
        <td>297863581.19</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>General City Responsibilities</td>
        <td>2624774841.64</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>General City Responsibilities</td>
        <td>2680604132.05</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Public Protection</td>
        <td>326281272.97</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Human Welfare &amp; Neighborhood Development</td>
        <td>1619388921.67</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Culture &amp; Recreation</td>
        <td>435425405.62</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>General Administration &amp; Finance</td>
        <td>2707016995.96</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Community Health</td>
        <td>1343545810.47</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>4678227150.64</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>General City Responsibilities</td>
        <td>3125981922.13</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Culture &amp; Recreation</td>
        <td>288539671.91</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Community Health</td>
        <td>1612080473.56</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Public Protection</td>
        <td>318032446.84</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Human Welfare &amp; Neighborhood Development</td>
        <td>1704999317.70</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>4170321886.52</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>General Administration &amp; Finance</td>
        <td>2709976639.48</td>
    </tr>
</table>



We try to demonstrate the results vividly in Tableau, as shown below.


```python
Image(url='https://s3.amazonaws.com/dmfa-2017fall/Pro4--rev.jpeg')
```




<img src="https://s3.amazonaws.com/dmfa-2017fall/Pro4--rev.jpeg"/>



Clearly, 'Public Works, Transportation & Commerce' Group has the largest revenue across these years, followed by 'General City Responsibilities' and 'General Administration & Finance'. The result makes sense to us since transportation and commerce usually generate many income for the government, so do the other two organizations. Besides, it is reasonable that the revenue from Public Works, Transportation & Commerce decreased after 2016 taking into consideration that 'Uber', 'lyft' and others became popular these year.


```python
%%sql
WITH orgspend_2015 AS (
SELECT fact.fiscal_year,organization.organization,sum(fact.amount) as spend_total
FROM fact 
JOIN organization ON organization.organization_id=fact.organization_id
JOIN RS ON fact.rs_id=RS.rs_id
WHERE RS.type='Spending' and fact.fiscal_year='2015'
GROUP BY fact.fiscal_year,organization.organization
ORDER BY spend_total DESC),
orgspend_2016 AS (
SELECT fact.fiscal_year,organization.organization,sum(fact.amount) as spend_total
FROM fact 
JOIN organization ON organization.organization_id=fact.organization_id
JOIN RS ON fact.rs_id=RS.rs_id
WHERE RS.type='Spending' and fact.fiscal_year='2016'
GROUP BY fact.fiscal_year,organization.organization
ORDER BY spend_total DESC),
orgspend_2017 AS (
SELECT fact.fiscal_year,organization.organization,sum(fact.amount) as spend_total
FROM fact 
JOIN organization ON organization.organization_id=fact.organization_id
JOIN RS ON fact.rs_id=RS.rs_id
WHERE RS.type='Spending' and fact.fiscal_year='2017'
GROUP BY fact.fiscal_year,organization.organization
ORDER BY spend_total DESC
), total_orgspend AS(
SELECT*FROM orgspend_2015
UNION
SELECT*FROM orgspend_2016
UNION
SELECT*FROM orgspend_2017)
SELECT*FROM total_orgspend
ORDER BY fiscal_year;
```

    21 rows affected.





<table>
    <tr>
        <th>fiscal_year</th>
        <th>organization</th>
        <th>spend_total</th>
    </tr>
    <tr>
        <td>2015</td>
        <td>General Administration &amp; Finance</td>
        <td>2394365501.18</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Community Health</td>
        <td>1794130104.53</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Human Welfare &amp; Neighborhood Development</td>
        <td>1202185527.41</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>General City Responsibilities</td>
        <td>391283695.69</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Public Protection</td>
        <td>1333975773.08</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Culture &amp; Recreation</td>
        <td>313605424.35</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>4180932008.79</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Public Protection</td>
        <td>1398936149.05</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Human Welfare &amp; Neighborhood Development</td>
        <td>1317163525.61</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Culture &amp; Recreation</td>
        <td>466050522.30</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>4433689710.24</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>General City Responsibilities</td>
        <td>414299839.67</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>Community Health</td>
        <td>1909322828.04</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>General Administration &amp; Finance</td>
        <td>2682615404.67</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Community Health</td>
        <td>1949161181.95</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Human Welfare &amp; Neighborhood Development</td>
        <td>1423495149.70</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>4118237607.92</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>General City Responsibilities</td>
        <td>428039305.04</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Public Protection</td>
        <td>1461444672.83</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>Culture &amp; Recreation</td>
        <td>360018275.92</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>General Administration &amp; Finance</td>
        <td>2876694064.35</td>
    </tr>
</table>




```python
Image(url='https://s3.amazonaws.com/dmfa-2017fall/Pro4--sp.jpeg')
```




<img src="https://s3.amazonaws.com/dmfa-2017fall/Pro4--sp.jpeg"/>



For governmental spending, we see that 'Public Works, Transportation & Commerce' group obviously takes up the most in the three years, followed by 'General Administration & Finance' and 'Community Health'. Compare to the revenue graph, we see that even though 'Public Works, Transportation & Commerce' generates the most revenue for the government, it is also the department that takes up the most spending. On the contrast, 'General City Responsibilities' generates the second most revenue for the government, but it is one of the lowest spending organizations among all these organizations. We also found that 'Culture & Recreation' generates the least revenue and also takes up least spending among all organizations.

**Next, it is also one of our aims to get some ideas which funds are involved most.**


```python
%%sql
SELECT DISTINCT fund.fund_category, sum(fact.amount) as revenue_amount
FROM fact,fund,RS
WHERE fact.fund_id = fund.fund_id and fact.RS_id=RS.RS_id and RS.type='Revenue'
GROUP BY fund.fund_category
ORDER BY revenue_amount DESC;
```

    5 rows affected.





<table>
    <tr>
        <th>fund_category</th>
        <th>revenue_amount</th>
    </tr>
    <tr>
        <td>Operating</td>
        <td>87976140752.29</td>
    </tr>
    <tr>
        <td>Continuing Projects</td>
        <td>18444365560.97</td>
    </tr>
    <tr>
        <td>Grants</td>
        <td>5470413115.17</td>
    </tr>
    <tr>
        <td>Annual Projects</td>
        <td>3937054372.14</td>
    </tr>
    <tr>
        <td>Work Orders/Overhead</td>
        <td>197211633.93</td>
    </tr>
</table>




```python
%%sql
SELECT DISTINCT fund.fund_category, sum(fact.amount) as spend_amount
FROM fact,fund,RS
WHERE fact.fund_id = fund.fund_id and fact.RS_id=RS.RS_id and RS.type='Spending'
GROUP BY fund.fund_category
ORDER BY spend_amount DESC;
```

    5 rows affected.





<table>
    <tr>
        <th>fund_category</th>
        <th>spend_amount</th>
    </tr>
    <tr>
        <td>Operating</td>
        <td>80670230679.69</td>
    </tr>
    <tr>
        <td>Continuing Projects</td>
        <td>15991726084.62</td>
    </tr>
    <tr>
        <td>Grants</td>
        <td>5464091733.00</td>
    </tr>
    <tr>
        <td>Annual Projects</td>
        <td>3582620814.46</td>
    </tr>
    <tr>
        <td>Work Orders/Overhead</td>
        <td>196713843.20</td>
    </tr>
</table>



Here we still use Tableau to achieve visualization, and this time we create trends in the past ten years.


```python
Image(url='https://s3.amazonaws.com/dmfa-2017fall/Project+4_trends.jpg')
```




<img src="https://s3.amazonaws.com/dmfa-2017fall/Project+4_trends.jpg"/>



From the bar charts above, 'Operating' fund is involved most of the times during past ten years and rises along with the total funds' increases. It is reasonable since huge amount of fund is needed to keep the hold government running. The next largest fund is located to 'Continuing Projects'. The government usually launches huge project that continue for several years and takes up huge amount of money. It makes sense that this type of fund cost the government a lot of money. At the same time, many of such project also generate a mass of return for the goverment. From this graph, we barely see 'Work Orders/Overhead' fund get involved. Since we are not familiar with this fund type, our guess is that it is not a major fund for the government. 

**Finally, we want to know the source and usage of the funds.**


```python
%%sql
SELECT DISTINCT program.program, sum(fact.amount) as revenue_amount
FROM fact,program,RS
WHERE fact.program_id = program.program_id and fact.RS_id=RS.RS_id and RS.type='Revenue'
GROUP BY program.program
ORDER BY revenue_amount DESC
limit 10;
```

    10 rows affected.





<table>
    <tr>
        <th>program</th>
        <th>revenue_amount</th>
    </tr>
    <tr>
        <td>General Fund Unallocated</td>
        <td>27134439883.05</td>
    </tr>
    <tr>
        <td>No Program Defined</td>
        <td>17187977134.48</td>
    </tr>
    <tr>
        <td>Administration</td>
        <td>16515350758.04</td>
    </tr>
    <tr>
        <td>Revenue, Transfers &amp; Reserves</td>
        <td>7697988486.55</td>
    </tr>
    <tr>
        <td>SFGH - Acute Care - Hospital</td>
        <td>7304818043.88</td>
    </tr>
    <tr>
        <td>Administration, Business</td>
        <td>4365958181.91</td>
    </tr>
    <tr>
        <td>Community Health/Support Service</td>
        <td>4291756893.49</td>
    </tr>
    <tr>
        <td>Administrative Support</td>
        <td>3813664939.26</td>
    </tr>
    <tr>
        <td>Health Service System</td>
        <td>2911649762.99</td>
    </tr>
    <tr>
        <td>Capital Programs &amp; Construction</td>
        <td>2608338169.04</td>
    </tr>
</table>




```python
Image(url='https://s3.amazonaws.com/dmfa-2017fall/Pro4--revbypro.jpeg')
```




<img src="https://s3.amazonaws.com/dmfa-2017fall/Pro4--revbypro.jpeg"/>




```python
%%sql
SELECT distinct program.program, sum(fact.amount) as spend_amount
FROM fact,program,RS
WHERE fact.program_id = program.program_id and fact.RS_id=RS.RS_id and RS.type='Spending'
GROUP BY program.program
ORDER BY spend_amount DESC
limit 10;
```

    10 rows affected.





<table>
    <tr>
        <th>program</th>
        <th>spend_amount</th>
    </tr>
    <tr>
        <td>Administration</td>
        <td>11867953878.03</td>
    </tr>
    <tr>
        <td>SFGH - Acute Care - Hospital</td>
        <td>6442300076.07</td>
    </tr>
    <tr>
        <td>Rail &amp; Bus Services</td>
        <td>4895100802.07</td>
    </tr>
    <tr>
        <td>Community Health/Support Service</td>
        <td>4290946903.43</td>
    </tr>
    <tr>
        <td>Water Capital Projects</td>
        <td>4204991019.87</td>
    </tr>
    <tr>
        <td>Business &amp; Finance</td>
        <td>4063922421.37</td>
    </tr>
    <tr>
        <td>General City Responsibilities</td>
        <td>3208804722.58</td>
    </tr>
    <tr>
        <td>Health Service System</td>
        <td>2871055497.96</td>
    </tr>
    <tr>
        <td>Patrol</td>
        <td>2812324407.38</td>
    </tr>
    <tr>
        <td>Fire Suppression</td>
        <td>2511187318.54</td>
    </tr>
</table>




```python
Image(url='https://s3.amazonaws.com/dmfa-2017fall/Pro4--spbypro.jpeg')
```




<img src="https://s3.amazonaws.com/dmfa-2017fall/Pro4--spbypro.jpeg"/>



From the results, we see that most of the revenue were from 'General Fund Unallocated', i.e. retained money, which means instead of using the funds for some programs, the government chose to keep them, indicating that the efficiency of fund using was a bit low - these retained money could be used for making better life of citizens. For the spending, it is not surprising that 'Administration' took up the most amount, taking into account in 'Spending Trends in Fund Categories' we saw that the operating costs was the largest expenses across years. Besides, we are pleased to see 'Acute Care', 'Rail & Bus Services', and 'Community Health/Support Service' programs were also the focuses of the government, which means the tax we paid was worthy.

## Bonus - Augment (10 points)

Sometimes the most value can be gained from one dataset when it is studied
alongside data drawn from other sources. Identify at least one additional data
source that can complement your analysis. Pull this additional data into your
chosen environment and explore at least one more theme you are able to further
analyze that depends upon a combination of data from both sources.

**Answer:**

San Francisco is the most innovative place in the world. The city government plays a critical role in distributing budget into different areas and collecting taxes. Our analysis aim to reflect the trend of economic performances in the last ten years by comparing the tax revenue with key economic indexes of the city. 


```python
import pandas as pd
newdata = pd.read_csv('newdataset.csv')
```

For convenience, we upload these two csv files to s3 in advance.


```python
!wget https://s3.amazonaws.com/dmfa-2017fall/SFecon-Annual.csv
!wget https://s3.amazonaws.com/dmfa-2017fall/SFecon-Monthly.csv
```

    --2017-12-08 16:44:18--  https://s3.amazonaws.com/dmfa-2017fall/SFecon-Annual.csv
    Resolving s3.amazonaws.com (s3.amazonaws.com)... 52.216.230.133
    Connecting to s3.amazonaws.com (s3.amazonaws.com)|52.216.230.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 234 [text/csv]
    Saving to: ‘SFecon-Annual.csv.5’
    
    SFecon-Annual.csv.5 100%[===================>]     234  --.-KB/s    in 0s      
    
    2017-12-08 16:44:18 (12.0 MB/s) - ‘SFecon-Annual.csv.5’ saved [234/234]
    
    --2017-12-08 16:44:18--  https://s3.amazonaws.com/dmfa-2017fall/SFecon-Monthly.csv
    Resolving s3.amazonaws.com (s3.amazonaws.com)... 52.216.230.133
    Connecting to s3.amazonaws.com (s3.amazonaws.com)|52.216.230.133|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 4753 (4.6K) [text/csv]
    Saving to: ‘SFecon-Monthly.csv.4’
    
    SFecon-Monthly.csv. 100%[===================>]   4.64K  --.-KB/s    in 0s      
    
    2017-12-08 16:44:18 (338 MB/s) - ‘SFecon-Monthly.csv.4’ saved [4753/4753]
    



```python
gdp = pd.read_csv('SFecon-Annual.csv')
other = pd.read_csv('SFecon-Monthly.csv')
```

We first take a look of the columns in two csv files.


```python
!csvcut -n SFecon-Annual.csv
```

      1: ﻿DATE
      2: GDP
      3: Year



```python
!csvcut -n SFecon-Monthly.csv
```

      1: ﻿DATE
      2: Unemployment_rate
      3: CPI
      4: ECON_CONDITION_INDEX
      5: HPI
      6: Year



```python
!csvcut -n newdataset.csv
```

      1: 
      2: Fiscal Year
      3: Revenue or Spending
      4: Organization Group
      5: Program
      6: Character
      7: Fund
      8: Fund Category
      9: Amount


We then create three new tables: newdata, gdp and other.


```python
%%sql
DROP TABLE IF EXISTS newdata;

CREATE TABLE newdata 
(
    ID    TEXT,
    Year     TEXT,
    Revenue_spending TEXT,
    Org   TEXT,
    Prog     TEXT,
    Chara TEXT,
    Fund   TEXT,
    Fund_cat  TEXT,
    Amount   NUMERIC
);
```

    Done.
    Done.





    []



To populate the tables, we again use absolute path.


```python
%%sql
COPY newdata FROM '/home/ubuntu/newdataset.csv'
CSV
HEADER;
```

    289429 rows affected.





    []




```python
%%sql
DROP TABLE IF EXISTS gdp;

CREATE TABLE gdp 
(
    DATE    TEXT,
    Amount   NUMERIC,
    Year     TEXT
);
```

    Done.
    Done.





    []




```python
%%sql
COPY gdp FROM '/home/ubuntu/SFecon-Annual.csv'
CSV
HEADER;
```

    9 rows affected.





    []




```python
%%sql
DROP TABLE IF EXISTS other;

CREATE TABLE other
(
    DATE    TEXT,
    Unemployment_rate   NUMERIC,
    CPI   NUMERIC,
    ECON_CONDITION_INDEX    NUMERIC,
    HPI NUMERIC,
    Year     TEXT
);
```

    Done.
    Done.





    []




```python
%%sql
COPY other FROM '/home/ubuntu/SFecon-Monthly.csv'
CSV
HEADER;
```

    114 rows affected.





    []



Check the three tables.


```python
%%sql
SELECT *
FROM newdata
LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>id</th>
        <th>year</th>
        <th>revenue_spending</th>
        <th>org</th>
        <th>prog</th>
        <th>chara</th>
        <th>fund</th>
        <th>fund_cat</th>
        <th>amount</th>
    </tr>
    <tr>
        <td>205140</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Water Operating Fund</td>
        <td>Continuing Projects</td>
        <td>171.26</td>
    </tr>
    <tr>
        <td>205141</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Annual Projects</td>
        <td>-45627.76</td>
    </tr>
    <tr>
        <td>205142</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>General Fund</td>
        <td>Continuing Projects</td>
        <td>-50385.5</td>
    </tr>
    <tr>
        <td>205143</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Parking &amp; Traffic Gasoline Tax Fund</td>
        <td>Continuing Projects</td>
        <td>225822.47</td>
    </tr>
    <tr>
        <td>205144</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Public Works, Transp. &amp; Commerce Fund</td>
        <td>Continuing Projects</td>
        <td>1672.97</td>
    </tr>
    <tr>
        <td>205145</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Public Works, Transp. &amp; Commerce Fund</td>
        <td>Work Orders/Overhead</td>
        <td>17844.66</td>
    </tr>
    <tr>
        <td>205146</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Earthquake Safety Improvements Fund</td>
        <td>Continuing Projects</td>
        <td>10081.92</td>
    </tr>
    <tr>
        <td>205147</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Fire Protection Systems Impvt. Fund</td>
        <td>Continuing Projects</td>
        <td>583.72</td>
    </tr>
    <tr>
        <td>205148</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Recreation &amp; Park Capital Impvts Fund</td>
        <td>Continuing Projects</td>
        <td>-1628.71</td>
    </tr>
    <tr>
        <td>205149</td>
        <td>2011</td>
        <td>Spending</td>
        <td>Public Works, Transportation &amp; Commerce</td>
        <td>City Capital Projects</td>
        <td>Mandatory Fringe Benefits</td>
        <td>Recreation &amp; Park Capital Impvts Fund</td>
        <td>Grants</td>
        <td>55651.28</td>
    </tr>
</table>




```python
%%sql
SELECT *
FROM gdp
LIMIT 10
```

    9 rows affected.





<table>
    <tr>
        <th>date</th>
        <th>amount</th>
        <th>year</th>
    </tr>
    <tr>
        <td>2008-01-01</td>
        <td>353339</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2009-01-01</td>
        <td>331326</td>
        <td>2009</td>
    </tr>
    <tr>
        <td>2010-01-01</td>
        <td>330154</td>
        <td>2010</td>
    </tr>
    <tr>
        <td>2011-01-01</td>
        <td>340757</td>
        <td>2011</td>
    </tr>
    <tr>
        <td>2012-01-01</td>
        <td>366459</td>
        <td>2012</td>
    </tr>
    <tr>
        <td>2013-01-01</td>
        <td>385843</td>
        <td>2013</td>
    </tr>
    <tr>
        <td>2014-01-01</td>
        <td>412423</td>
        <td>2014</td>
    </tr>
    <tr>
        <td>2015-01-01</td>
        <td>440246</td>
        <td>2015</td>
    </tr>
    <tr>
        <td>2016-01-01</td>
        <td>470529</td>
        <td>2016</td>
    </tr>
</table>




```python
%%sql
SELECT *
FROM other
LIMIT 10
```

    10 rows affected.





<table>
    <tr>
        <th>date</th>
        <th>unemployment_rate</th>
        <th>cpi</th>
        <th>econ_condition_index</th>
        <th>hpi</th>
        <th>year</th>
    </tr>
    <tr>
        <td>2008-01-01</td>
        <td>4.6</td>
        <td>219.612</td>
        <td>2.10</td>
        <td>179.22</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-02-01</td>
        <td>4.3</td>
        <td>219.612</td>
        <td>1.67</td>
        <td>176.68</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-03-01</td>
        <td>4.6</td>
        <td>219.612</td>
        <td>0.37</td>
        <td>175.14</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-04-01</td>
        <td>4.3</td>
        <td>222.074</td>
        <td>-0.26</td>
        <td>171.98</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-05-01</td>
        <td>4.7</td>
        <td>222.074</td>
        <td>-1.11</td>
        <td>171.13</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-06-01</td>
        <td>5.3</td>
        <td>225.181</td>
        <td>-1.31</td>
        <td>167.77</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-07-01</td>
        <td>5.6</td>
        <td>225.181</td>
        <td>-1.59</td>
        <td>165.74</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-08-01</td>
        <td>5.8</td>
        <td>225.411</td>
        <td>-2.42</td>
        <td>162.33</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-09-01</td>
        <td>5.5</td>
        <td>225.411</td>
        <td>-3.19</td>
        <td>160.79</td>
        <td>2008</td>
    </tr>
    <tr>
        <td>2008-10-01</td>
        <td>5.8</td>
        <td>225.824</td>
        <td>-4.57</td>
        <td>157.22</td>
        <td>2008</td>
    </tr>
</table>



Below two queries aim to sum all tax revenues of the city government from newdata and select GDP of corresponding years from gdp file.


```python
%%sql
SELECT newdata.year, round(sum(newdata.amount/100000),3) as tax_revenue, (gdp.amount) as gdp from newdata
LEFT JOIN gdp 
ON newdata.year=gdp.year
WHERE (newdata.Chara not like '%Taxi%') and (newdata.Chara like '%Tax%') and (newdata.Revenue_spending='Revenue' )
GROUP BY gdp.amount, newdata.year
ORDER BY newdata.year;
```

    10 rows affected.





<table>
    <tr>
        <th>year</th>
        <th>tax_revenue</th>
        <th>gdp</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>25203.470</td>
        <td>353339</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>26798.099</td>
        <td>331326</td>
    </tr>
    <tr>
        <td>2010</td>
        <td>27817.620</td>
        <td>330154</td>
    </tr>
    <tr>
        <td>2011</td>
        <td>28876.065</td>
        <td>340757</td>
    </tr>
    <tr>
        <td>2012</td>
        <td>30827.858</td>
        <td>366459</td>
    </tr>
    <tr>
        <td>2013</td>
        <td>33380.300</td>
        <td>385843</td>
    </tr>
    <tr>
        <td>2014</td>
        <td>36128.418</td>
        <td>412423</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>39583.721</td>
        <td>440246</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>42804.773</td>
        <td>470529</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>46798.096</td>
        <td>None</td>
    </tr>
</table>




```python
%%sql
SELECT newdata.year, round((sum(newdata.amount/100000)/gdp.amount),3) as tax_GDP_ratio
FROM newdata
LEFT JOIN gdp 
ON newdata.year=gdp.year
WHERE (newdata.Chara not like '%Taxi%') and (newdata.Chara like '%Tax%') and (newdata.Revenue_spending='Revenue' )
GROUP BY gdp.amount, newdata.year
ORDER BY newdata.year;
```

    10 rows affected.





<table>
    <tr>
        <th>year</th>
        <th>tax_gdp_ratio</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>0.071</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>0.081</td>
    </tr>
    <tr>
        <td>2010</td>
        <td>0.084</td>
    </tr>
    <tr>
        <td>2011</td>
        <td>0.085</td>
    </tr>
    <tr>
        <td>2012</td>
        <td>0.084</td>
    </tr>
    <tr>
        <td>2013</td>
        <td>0.087</td>
    </tr>
    <tr>
        <td>2014</td>
        <td>0.088</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>0.090</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>0.091</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>None</td>
    </tr>
</table>



By calculating the Tax_to_GDP ratio of SF in the past nine years, we found that the ratio gradually increased from 7% to 9%. Even though these numbers were below the national average, around 25%, we can raise a question for further investigation: Is SF's growth slowing down? Or tax is growing faster than economic growth?


```python
%%sql
SELECT round(avg(cpi),2) 
FROM other;
```

    1 rows affected.





<table>
    <tr>
        <th>round</th>
    </tr>
    <tr>
        <td>242.53</td>
    </tr>
</table>




```python
%%sql
SELECT other.Year, round(avg(Unemployment_rate),3) as unemployment_rate, round(avg(CPI),3) as CPI, round(avg(ECON_CONDITION_INDEX),3) as ECON_CONDITION_INDEX, round(avg(HPI),3) as HPI, round(sum(newdata.amount/100000),3) as tax_revenue
FROM other
LEFT JOIN newdata 
ON newdata.Year=other.Year
GROUP BY other.Year
ORDER BY other.Year;
```

    10 rows affected.





<table>
    <tr>
        <th>year</th>
        <th>unemployment_rate</th>
        <th>cpi</th>
        <th>econ_condition_index</th>
        <th>hpi</th>
        <th>tax_revenue</th>
    </tr>
    <tr>
        <td>2008</td>
        <td>5.250</td>
        <td>222.862</td>
        <td>-1.917</td>
        <td>166.246</td>
        <td>1998404.077</td>
    </tr>
    <tr>
        <td>2009</td>
        <td>8.683</td>
        <td>224.158</td>
        <td>-3.793</td>
        <td>141.590</td>
        <td>2127617.862</td>
    </tr>
    <tr>
        <td>2010</td>
        <td>8.917</td>
        <td>227.327</td>
        <td>2.207</td>
        <td>146.273</td>
        <td>2427273.497</td>
    </tr>
    <tr>
        <td>2011</td>
        <td>8.050</td>
        <td>233.113</td>
        <td>4.911</td>
        <td>142.383</td>
        <td>2435735.630</td>
    </tr>
    <tr>
        <td>2012</td>
        <td>6.783</td>
        <td>239.434</td>
        <td>5.970</td>
        <td>147.026</td>
        <td>2690533.450</td>
    </tr>
    <tr>
        <td>2013</td>
        <td>5.458</td>
        <td>244.766</td>
        <td>5.278</td>
        <td>171.758</td>
        <td>2794585.407</td>
    </tr>
    <tr>
        <td>2014</td>
        <td>4.367</td>
        <td>251.713</td>
        <td>5.483</td>
        <td>194.928</td>
        <td>2857888.636</td>
    </tr>
    <tr>
        <td>2015</td>
        <td>3.608</td>
        <td>258.144</td>
        <td>5.838</td>
        <td>213.964</td>
        <td>2944478.838</td>
    </tr>
    <tr>
        <td>2016</td>
        <td>3.250</td>
        <td>266.042</td>
        <td>4.442</td>
        <td>226.326</td>
        <td>3169508.120</td>
    </tr>
    <tr>
        <td>2017</td>
        <td>2.967</td>
        <td>272.870</td>
        <td>4.088</td>
        <td>233.733</td>
        <td>1592821.357</td>
    </tr>
</table>



Covert the output into Tableau visualizations.



```python
Image(url='https://s3.amazonaws.com/dmfa-2017fall/proj4-month.jpg')
```




<img src="https://s3.amazonaws.com/dmfa-2017fall/proj4-month.jpg"/>



Based on data in the past ten years, tax revenue of San Francisco city government (excluding incomplete data of 2017) increases with CPI, HPI and ECI. Only the unemployment rate decreases through the years. These trends reveals that the overall economic environment in San Francisco has been positive. That is, increasing CPI represents inflation, which matches with the increasing real estate prices (HPI). Normally, booming economic units like SF bring more tax revenue to the government.

* HPI reflects the changes of real estate prices
* ECI is a key index to show the economic performance 
