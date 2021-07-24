# Energon

## Installation

Python version 3.7

Library dependencies are sepecified in requirements.txt

## Introduction of Energon and EnergonQL

**EnergonQL** is a query language to extract data for building analytics. **Energon** is a data analytic platform based on EnergonQL and contributes for the entire process of building analytics.

### The format of EnergonQL

The Energon query language, **EnergonQL**, following the concept of object database, can be defined as ***select-from-where*** expressions. The syntax of is as follows:

**SELECT** *feature representations* \
**FROM** *list of buildings* \
**WHERE** *predicate expressions* \
**FILTER** *predicate expressions* \
**LABEL** *list of labels*

The **SELECT** expression decides which features of the building to extract. It consists of two types of elements: logic view and operator. You can combine any logic view and operator and use them to represent the features you want to extract.

The **FROM** expression states the objects of buildings, following the concept of object database.

The **WHERE** expression decides which building is the target to extract data from.

The **FILTER** expression decides the range of extracted data.

The **LABLE** expression decides the labels of data analytics to be used.

### Two logic views of EnergonQL (subsystem and functionality)

In Energon, there are two logic views: subsystem and functionality. Each logic view represents a part of data of the building. The subsystem view represents the data from a system of the building. e.g., **Chiller** represents the data of chiller systems. The functionality view represents a specific type of function of data. e.g., **Temperature** represents the temperature data of all the entire building.

### Operators of EnergonQL

Four operators can be used to compose the final representation of features: **Union(+)**, **Intersection(\*)**, **Difference(-)**, and **Join(%)**, following the concept of set operation.

For example, ***Chiller \* Temperature*** means intersects the data of chiller systems and temperature functions. Thus, the temperature data of chiller systems is extracted.

## An Entire Example

Let's see an entire example that tracing the daily mean power of chillers how is implemented in Energon. Now that there is a building named **\'EMSD\'**, whose ***power data*** from ***Jun. 1 to 30, 2020*** need to be extacted to track changes in daily mean power of chillers.

### Energon query

`SELECT Chiller(B) * Power(B)` \
`FROM Building B` \
`WHERE B.BuildingID = 'EMSD' ` \
`FILTER B.TIMESTAMP > '202o0101' AND B.TIMESTAMP < '20200130'` 

### Sample code


```python
from engine.engineQL import *
import pandas as pd

# define the Energon query to decide which features to extract
chiller_power_query = """
SELECT Chiller(B) * Power(B)
FROM Building B
WHERE B.BuildingID = 'EMSD'
FILTER B.TIMESTAMP > '20200101' AND B.TIMESTAMP < '20200130'    
"""

# input the query to Energon and extract data
chiller_power_data = energon(chiller_power_query)

# resample the chiller power data in days to calculate the mean daily value
chiller_power_data.set_index('Time_Stamp')
mean_chiller_power_data = chiller_power_data.resample('D', how='mean')

# display the results
print(mean_chiller_power_data)
```

