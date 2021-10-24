# Data-driven Building Analytics
## Procedure for building analytics
![image](https://github.com/fangger4396/energon_example/blob/main/img/process.png){:height="50%" width="50%"}
## Two real-world building datasets
We firstly show two real-world building datasets. One is the **EMSD** dataset which consists of serveral tables (stored as multiple .csv files) and each table stores the data collected from an independent sensor of the building DP. Another is the **DP** dataset which is a single super table (stored as a single .csv file) and all the data collected by different sensors of building EMSD are concated by columns together. Both of them can be potentially used for the building analytic **Energy Consumption Predicetion (ECP)**.

![test image size](https://github.com/fangger4396/energon_example/blob/main/img/data_tables.png)

In fact, we can never expect that all the real-world datasets to be construted by the above methods. Without constraints, the organization of building datasets can be arbitrary. For example, the datasets can also be a single table and all the data from different sensors are concated by raws. Thus, it comes more difficult when the number of buildings becomes multiple.

# Methods for Building Data Accessing
## Manual way
For the **DP** dataset, we try to extract the data in a manual way. With the help of data processing tools (e.g. **Pandas** in python), we can read the table and extract the data (e.g. electricity meter readings) we need for ECP, as showed in the following codes:
```python
import pandas as pd

# 1. define the feature list to be used in  model training process, which is a subset of all the features of DP data.
feature_list = ['chiller_electricity', 'light_electricity', 'steam_electricity', 'solar_illuminance']

# 2. read the csv file and load required data
dataset = pd.read_csv('DP_file.csv')
required_data = dataset[feature_list]
...
```
Note that this codes is hard to be reused for another building datas like **EMSD**. Because the features of the list in the first step can only be added case by case. Besides, not all the data are stored in the same table. Thus, one have to rewrite codes for other buildings.
## Metadata-schema based way
For the **EMSD** dataset, fortunately, as it use the **Brick** metadata schema, we can use **Brick** metadata (represented as rdf triples and SPARQL can be used to query it) to specify the data we need in EMSD. Refer to the [Brick Ontology documentation page][brick] for details on the contents of **Brick**. 
For example, with the help of **Mortar** (a tool to extract data for **Brick**), the following code can be used to extract data in EMSD.

```python
import pymortar

URL = "http://mortar_mortar-server_1:5001"
# connect client to Mortar frontend server
c = pymortar.Client(URL)

chiller_power_query = """
SELECT ?chp ?cpp ?pp WHERE {
  ?chiller a brick:Chiller .
  ?chp a brick:Power_Sensor .
  ?chiller brick:hasPoint ?chp .
  
  ?compressor a brick:Compressor .
  ?chiller brick:hasPart ?compressor .
  ?cpp a brick:Power_Sensor .
  ?compressor brick:hasPoint ?cpp .
  
  ?pump a brick:Pump .
  ?chiller brick:hasPart ?pump .
  ?pp a brick:Power_Semsor .
  ?pump brick:hasPoint ?pp .
}
"""

# get a summary of which sites return results for the set of query
c.qualify([chiller_power_query])

# get SPARQL query results, stream metadata data tables as separate objects
chiller_power_data = c.data_sparql(chiller_power_query)

# resample the chiller power data in days to calculate the mean daily value
chiller_power_data.set_index('time')
mean_chiller_power_data = chiller_power_data.resample('D', how='mean')

# display the results
print(mean_chiller_power_data)
```
These codes are also hard to be reused when it comes to another building, even if this another building is supported by **Brick**. Firstly, the SPARQL query always needed to be rewrote, as the metadata differs. Secondly, one has to be an expert who has good understanding for both **Brick** metadata schema itself and the metadata of the specific building. Otherwise, one may not be able to construct precise SPARQL query.

## A promising way

# Two Cases of Real-world Buidling Analytics

## Unified model training process
```python
from torch.utils.data import Dataset, DataLoader

train_data = my_building_dataset()
dataloader = Dataloader(train_data, batch_size=4, shuffle=True)
num_epoches = 100

for epoch in range(num_epoches)
  for X,y in dataloader:
    ...
```
## Quality evaluation for models
[brick]:https://brickschema.org/ontology/
