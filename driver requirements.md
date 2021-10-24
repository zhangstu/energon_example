# Data-driven Building Analytics
## Procedure for building analytics

## Two real-world building datasets
We firstly show two real-world building datasets. One is the **EMSD** dataset which consists of serveral tables (stored as multiple .csv files) and each table stores the data collected from an independent sensor of the building DP. Another is the **DP** dataset which is a single super table (stored as a single .csv file) and all the data collected by different sensors of building EMSD are concated by raws together. Both of them can be potentially used for the building analytic **Energy Consumption Predicetion (ECP)**.

![image](https://github.com/fangger4396/energon_example/blob/main/img/data_tables.png)

In fact, we can never expect that all the real-world datasets to be construted by the above methods. Without constraints, the organization of building datasets can be arbitrary. For example, the datasets can also be a single table and all the data from different sensors are concated by columns as follows. Thus, it comes more difficult when the number of buildings becomes multiple.

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
## Metadata-scheme based way

## Deficiencies when it comes to multiple buildings

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
