# Building Application Introduction
Building Applications are widely used in building energy tracing, fault detection and equipment control, which are always developed in data-driven manner today. Here, we firstly introduce the application named **Energy Consumption Predition (ECP)**, and then show how two different building datasets can be used for this application.
## Energy Consumption Prediction
The Energy Consumption Prdiction can be used to predict the next timestamp energy consumption of a group of building systems by training a ML model with histrical electricity data of the related systems. Here, a general ML model construction process can be followed to get this model, which consists of data access, data processing, model training, model evaluation orderly.
## Building Datasets Introduction
Besides the EMSD data, we also provide an open-source dataset named **Genome**. Both of them can be download at **xxx**.
## Model Introduction
### Model 1
### Model 2

# Examples
## Example 1: Using Genome Data for with Brick
By writing **SPARQL** queries, **Brick** can assist us to access the **Genome** data. For example:

`SELECT ?ps ?ots WHERE{`\
`?equip a brick:Equipment .`\
`?ps a brick:Power_sensor .`\
`?equip brick:hasPoint ?ps .`\
`?wea a brick:Weather .`\
`?ops a brick:Temperature_sensor .`\
`?wea brick:hasPoint ?ops .}`

This query extracts the following list of features in **Genome**:

+ **Building Electricity**\
+ **Outdoor Temperature**\
** ... **

By using these features as input of model 1, the accuracy of the model 1 for **Genome** is xxx with the **Mean Square Error** as metric.
## Example 2: Using Genome Data with Energon
Similarly, **EnergonQL** queries can help us to access the **Genome** data by using Energon. For example:

`SELECT Power + Weather * Temperature`\
`FROM Building A`\
`WHERE A.id = 'Genome'`

This query extracts the same list of features. Thus it gives the same accuracy of model 1.
## Example 3: Using EMSD Data with Brick and Energon

