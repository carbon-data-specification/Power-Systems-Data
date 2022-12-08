# Data types and specifications

## Entities

We propose to define data types for three different kinds of entities. These entities would all be defined as nodes in the [topology graph](topology.md). The three kinds of entities are:

* System. A system is the lowest granularity level in the topology graph. It is a collection of other entities. A system can for example be a single synchronous grid, or a collection of synchronous grids.
* Market. A market is a collection of nodes that are connected to each other through a single market operator. A market can for example be a single balancing area, or a collection of balancing areas.
* Unit. A unit is a single physical entity. A unit can for example be a single power plant, or a single generation unit within that plant.

### System

| Data Type                           | Data definition                                                          | Unit / Data Format | Temporal Granularity          | Secondary Information                        | Locational Granularity | Time horizon        | Latency   | Maximum Latency | Requirement level |
|-------------------------------------|--------------------------------------------------------------------------|--------------------|-------------------------------|----------------------------------------------|------------------------|---------------------|-----------|-----------------|-------------------|
| Generation                          | Net generation output per time unit                                      | MW (Float)         | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time | Day +7          | Must-have         |
| Demand                              | Total system demand                                                      | MW (Float)         | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time | Day +7          | Must-have         |
| Total Emissions (by pollutant type) | Total operating (direct) emissions over time period                      | t (metric) (Float) | Market (hourly or sub hourly) | Locational Granularity, Pollutant type       | Balancing Region       | Real-time           | Real-time | Day +7          | Should-have       |
| Imports                             | Imports by interconnect                                                  | MW (Float)         | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time | Day +7          | Must-have         |
| Exports                             | Exports by interconnect                                                  | MW (Float)         | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time | Day +7          | Must-have         |
| Renewable generation curtailment    | Curtailment of renewable generation                                      | MW (Float)         | Market (hourly or sub hourly) | Locational Granularity, fuel type (optional) | Balancing Region       | Real-time           | Real-time | Day +7          | Should-have       |
| Generation by fuel type             | The full breakdown of generation per fuel type (e.g. wind, solar)        | MW (Float)         | Market (hourly or sub hourly) | Fuel Type List                               | Balancing Region       | Real-time           | Real-time | Day +7          | Must-have         |
| Demand Forecast                     | Forecast of system demand over the next day                              | MW (Float)         | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Short-term forecast | Day-ahead | N/A             | Should-have       |
| Generation Forecast                 | Forecast of system generation by fuel type over the next day             | MW (Float)         | Market (hourly or sub hourly) | Fuel Type List, generation technology        | Balancing Region       | Short-term forecast | Day-ahead | N/A             | Should-have       |
| Curtailment Forecast                | Forecast of renewable generation curtailment                             | MW (Float)         | Market (hourly or sub hourly) | Fuel Type List, generation technology        | Balancing Region       | Short-term forecast | Day-ahead | N/A             | May-have          |
| Installed Capacity                  | Max output of technology type                                            | MW (Float)         | Annual                        | Fuel Type List                               | Balancing Region       | Historical          | Day +30   | Day +90         | Must-have         |
| Generation type emissions intensity | Direct Emission factors per production mode level                        | tCO2/MWh (Float)   | Annual                        | Fuel Type List                               | Balancing Region       | Historical          | Day +30   | Day +90         | Should-have       |
| Fuel emissions rates                | Direct Emissions associated with a fixed volume of consumption of a fuel | tCO2/BTU (Float)   | Annual                        | Fuel Type List                               | Balancing Region       | Historical          | Day +30   | Day +90         | Should-have       |

### Market

| Data Type                                                    | Data definition                                                                    | Unit / Data Format   | Temporal Granularity          | Secondary Information  | Locational Granularity | Time horizon | Latency   | Maximum Latency | Requirement level |
|--------------------------------------------------------------|------------------------------------------------------------------------------------|----------------------|-------------------------------|------------------------|------------------------|--------------|-----------|-----------------|-------------------|
| REAL TIME market price Locational Marginal Prices/ Balancing | LMP set by system operator at each node on the grid (assuming nodal market design) | Currency/MWh (Float) | Market (hourly or sub hourly) | Locational Granularity | Zonal / nodal          | Real-time    | Real-time | N/A             | Should-have       |
| Locational Marginal Congestion Prices                        | The impact of congestion on LMP                                                    | Currency/MWh (Float) | Market (hourly or sub hourly) | Locational Granularity | Zonal / nodal          | Historical   | Day +?    | N/A             | Should-have       |
| Day ahead price                                              | Day ahead prices set by market operator per market zone, by hour                   | Currency/MWh         | Market (hourly or sub hourly) | Locational granularity | Zonal / nodal          | Day ahead    |           |                 | Must-have         |

### Unit

| Data Type                          | Data definition                                                                                                                    | Unit / Data Format                       | Temporal Granularity          | Secondary Information   | Locational Granularity | Time horizon | Latency | Maximum Latency | Requirement level |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|-------------------------------|-------------------------|------------------------|--------------|---------|-----------------|-------------------|
| Unit Generation                    | Generation for each unit in each time interval                                                                                     | MWh (Float)                              | Market (hourly or sub hourly) | Unit ID                 | unit                   | Historical   | Day +30 | Day + 90        | Should-have       |
| Unit Emissions (by pollutant type) | Direct emissions from combustion of fuel                                                                                           | t (Float)                                | Market (hourly or sub hourly) | Unit ID, Pollutant type | unit                   | Historical   | Day +30 | Day + 90        | Should-have       |
| Installed Capacity                 | Max output of plant                                                                                                                | MW (Float)                               | Monthly                       | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | Must-have         |
| Unit Heat Rates                    | The amount of energy used by an electrical generator/power plant to generate one kWh of electricity                                | J/kWh (Float)                            | Monthly                       | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | Should-have       |
| Heat Rates Curves                  | The amount of energy used by an electrical generator/power plant to generate one kWh of electricity at different generation levels | J/kWh (Float)                            | Monthly                       | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | May-have          |
| Unit fuel types                    | Fuel consumed by each unit                                                                                                         | List of types (List[String])             | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | Must-have         |
| Unit Location                      |                                                                                                                                    | Lat,Long (Cf Google Maps) (Tuple[Float]) | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | Should-have       |
| Unit Type                          |                                                                                                                                    | Generation Technology (String)           | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | Should-have       |
| Unit market                        | Energy market of the unit                                                                                                          | (Integer)                                | Monthly                       | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | May-have          |
| Unit owner information             |                                                                                                                                    | (String)                                 | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | May-have          |
| Unit ID number                     |                                                                                                                                    | (Integer)                                | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | Should-have       |
| Unit name                          |                                                                                                                                    | (String)                                 | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | May-have          |
| Unit start year                    |                                                                                                                                    | (Integer)                                | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | Should-have       |
| Co-generation Heat Output          | Total Heat produced in the cogeneration heation                                                                                    | J (Float)                                | Annual                        | Unit ID                 | unit                   | Historical   | Annual  | Year + 1        | May-have          |

## Variables

Some data types are defined for multiple variables. For example, emissions can be reported for multiple pollutants, or production can be reported for multiple fuel types.

In these cases, the data type is defined once, and we specify henceforth that the data type is a variable. The variables are defined as follows:

### Fuel Types

| Fuel type                   |
|-----------------------------|
| Brown coal/Lignite          |
| Hard coal                   |
| Coal-derived gas            |
| Other coal                  |
| Natural gas                 |
| Landfill gas                |
| Other gas                   |
| Wood / Wood waste           |
| Municipal waste             |
| Propane oil                 |
| Shale oil                   |
| Distillate oil              |
| Other oil                   |
| Peat                        |
| Uranium, Thorium, Plutonium |
| Solar                       |
| Wind                        |
| Geothermal                  |
| Water                       |

### Generation Technology

| Generation technology                        |
|----------------------------------------------|
| Energy Storage, Battery                      |
| Energy Storage, Compressed Air               |
| Energy Storage, Concentrated Solar Power     |
| Energy Storage, Flywheel                     |
| Energy Storage, Reversible Hydraulic Turbine |
| Energy Storage, Other                        |
| Fuel Cell                                    |
| Turbine, Binary Cycle                        |
| Turbine, Combined-Cycle                      |
| Turbine, Steam                               |
| Turbine, Gas Combustion                      |
| Hydrokinetic, Axial Flow Turbine             |
| Hydrokinetic, Wave Buoy                      |
| Hydrokinectic, Other                         |
| Hydraulic Turbine                            |
| Internal Combustion                          |
| Other                                        |
| Photovoltaic                                 |
| Wind Turbine, Onshore                        |
| Wind Turbine, Offshore                       |

### Generator Aggregation

| Generator Aggregation |
|-----------------------|
| Generator             |
| Plant                 |
| Unit                  |
| Boilers               |

### Pollutant Types

| Pollutant type |
|----------------|
| NOx            |
| SOx            |
| CO2            |
| Particulates   |
