# Data-driven Building Analytics
## Procedure for building analytics

## Two real-world building datasets
We firstly show two real-world building datasets. One is the **DP** dataset which consists of serveral tables (stored as multiple .csv files) and each table stores the data collected from an independent sensor of the building DP. Another is the **EMSD** dataset which is a single super table (stored as a single .csv file) and all the data collected by different sensors of building EMSD are concated by raws together. Both of them can be potentially used for the building analytic **Energy Consumption Predicetion (ECP)**.

**a figure to show the different data files**

In fact, we can never expect that all the real-world datasets to be construted by the above methods. Without constraints, the organization of building datasets can be arbitrary. For example, the datasets can also be a single table and all the data from different sensors are concated by columns as follows. Thus, it comes more difficult when the number of buildings becomes multiple.

# Methods for Building Data Accessing
## Manual way
For the **DP** dataset, we try to extract the data in a manual way. With the help of data processing tools (e.g. Pandas in python), we can read the tables and extract the data (e.g. electricity meter readings) we need for ECP, as showed in the following codes:
```python
import pandas as pd
for file in dic
...
```
Note that this codes is hard to be reused for another building datas like EMSD. Because either 
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
