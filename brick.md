# Brick

## Introduction of Brick

The **Brick schema**, also referred to as the **Brick ontology**, is a formal data model for describing data sources and their context in a building. Refer to the [Brick Ontology documentation page][brick] for details on the contents of the Brick ontology.

In Brick, the building data model which reveals the entities and the relationship among them in the building can be represented as **RDF triples**. For example, <brick:Chiller, brickframe:hasPart, brick:Pump> represents that a pump is a component of a chiller.

To retrieve a certain data point (e.g. the power of a pump) in a building, one needs to know the specific RDF triples about this data point, and write **SPARQL** queries.

## Introduction of Mortar

**Mortar** is a platform that can retrieve building data built in **Brick schema**. A standard workflow consists of qualify, fetch and aggregation is used by Mortar to conduct data extraction.

## An Entire Example

Let's see an entire example that tracing the daily mean power of chillers how is implemented in Energon. Now that there is a building named **\'EMSD\'**, whose ***power data*** from ***Jun. 1 to 30, 2020*** need to be extacted to track changes in daily mean power of chillers.

### SPARQL query

`SELECT ?chp ?cpp ?pp WHERE {`\
`  ?chiller a brick:Chiller .`\
`  ?chp a brick:Power_Sensor .`\
`  ?chiller brick:hasPoint ?chp .`
  
`  ?compressor a brick:Compressor .`\
`  ?chiller brick:hasPart ?compressor .`\
`  ?cpp a brick:Power_Sensor .`\
`  ?compressor brick:hasPoint ?cpp .`
  
`  ?pump a brick:Pump .`\
`  ?chiller brick:hasPart ?pump .`\
`  ?pp a brick:Power_Semsor .`\
`  ?pump brick:hasPoint ?pp .`\
`}`

### Sample code


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
[brick]:https://brickschema.org/ontology/
