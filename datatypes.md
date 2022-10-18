# Data types and specifications

## Entities

We propose to define data types for three different kinds of entities. These entities would all be defined as nodes in the [topology graph](topology.md). The three kinds of entities are:

* System. A system is the lowest granularity level in the topology graph. It is a collection of other entities. A system can for example be a single synchronous grid, or a collection of synchronous grids.
* Market. A market is a collection of nodes that are connected to each other through a single market operator. A market can for example be a single balancing area, or a collection of balancing areas.
* Unit. A unit is a single physical entity. A unit can for example be a single power plant, or a single generation unit within that plant.

### System

| Data type                                | Definition                                                               | Unit     | Temporal granularity          | Secondary information                        | Locational granularity | Time horizon        | Latency      | Max latency |
|------------------------------------------|--------------------------------------------------------------------------|----------|-------------------------------|----------------------------------------------|------------------------|---------------------|--------------|-------------|
| Generation                               | Net generation output per time unit                                      | MWH      | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time    | n/a         |
| Demand                                   | Total system demand                                                      | Mwh      | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time    |             |
| Total Emissions (by pollutant type)      | Total operating (direct) emissions over time period                      | t        | Market (hourly or sub hourly) | Locational Granularity, Pollutant type       | Balancing Region       | Real-time           | Real-time    | Day +7      |
| Imports                                  | Imports by interconnect                                                  | MWH      | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time    | Day +7      |
| Exports                                  | Exports by interconnect                                                  | MWH      | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Real-time           | Real-time    | Day +7      |
| Renewable generation curtailment         | Curtailment of renewable generation                                      | MWH      | Market (hourly or sub hourly) | Locational Granularity, fuel type (optional) | Balancing Region       | Real-time           | Real-time    | Day +7      |
| Generation by fuel type                  | The full breakdown of generation per fuel type (e.g. wind, solar)        | MWH      | Market (hourly or sub hourly) | Fuel Type List                               | Balancing Region       | Real-time           | Real-time    | Day +7      |
| Demand Forecast                          | Forecast of system demand over the next day                              | MWH      | Market (hourly or sub hourly) | Locational Granularity                       | Balancing Region       | Short-term forecast | Day-ahead    |             |
| Generation Forecast                      | Forecast of system generation by fuel type over the next day             | MWH      | Market (hourly or sub hourly) | Fuel Type List, generation technology        | Balancing Region       | Short-term forecast | Day-ahead    |             |
| Curtailment Forecast                     | Forecast of renewable generation curtailment                             | MWH      |                               |                                              |                        |                     |              |             |
| Installed Capacity                       | Max output of technology type                                            | MW       |                               | Fuel Type List                               |                        |                     |              |             |
| Generation type emissions intensity      |  Direct Emission factors per production mode level                       | tCO2/MWh |                               | Fuel Type List                               |                        |                     |              |             |
| Fuel emissions rates                     | Direct Emissions associated with a fixed volume of consumption of a fuel | tCO2/BTU |                               | Fuel Type List                               |                        |                     |              |             |
| Activated reserved of ancillary services | Volume of activated ancillary services, per reserve type, per hour       | MW       | Market (hourly or sub hourly) | Locational granularity, reserve type         | Balancing Region       | Real-time           | Real-time -1 |             |

### Market

| Data type                                                    | Definition                                                                                                                            | Unit         | Temporal granularity          | Secondary information                | Locational granularity | Time horizon | Latency   | Max latency |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|--------------|-------------------------------|--------------------------------------|------------------------|--------------|-----------|-------------|
| REAL TIME market price Locational Marginal Prices/ Balancing | LMP set by system operator at each node on the grid (assuming nodal market design)                                                    | Currency/MWh | Market (hourly or sub hourly) | Locational Granularity               | Zonal / nodal          | Real-time    | Real-time | Day +?      |
| Locational Marginal Congestion Prices                        | The impact of congestion on LMP                                                                                                       | Currency/MWh | Market (hourly or sub hourly) | Locational Granularity               | Zonal / nodal          | Historical   | Day +?    |             |
| Marginal Loss Component                                      | The impact of losses (as applicable, based on market design) on LMP                                                                   | Currency/MWh | Market (hourly or sub hourly) | Locational Granularity               | Zonal / nodal          | Historical   | Day +?    |             |
| Binding Transmission Constraints and shadow prices           | List of which specific transmission elements are impending eocnomic dispatch and the marginal system costs associated with congestion |              |                               | Locational Granularity               |                        |              |           |             |
| Shift Factor Matrix                                          | Matrix describing the percentage of MWh injected at a node that flows over a transmission constraint.                                 |              |                               | Locational Granularity               |                        |              |           |             |
| Violated Transmission Constraints and shadow prices          | List of which specific transmission elements are above transmission limits and associated violation penalties                         |              |                               | Locational Granularity               |                        |              |           |             |
| Unit offer curves                                            | Monotonically increasing offer curve for each unit                                                                                    | Currency/MWh | Market (hourly or sub hourly) | Unit ID                              | Zonal / nodal          | Historical   | Day +?    |             |
| Day ahead price                                              | Day ahead prices set by market operator per market zone, by hour                                                                      | Currency/MWh | Market (hourly or sub hourly) | Locational granularity               | Zonal / nodal          | Day ahead    |           |             |
| Ancillary services prices                                    | Price of the ancillary services, per type, per hour                                                                                   | Currency/MW  | Market (hourly or sub hourly) | Locational granularity, reserve type | Zonal / nodal          | Real-time    |           |             |
| Contracted ancillary services                                | Contracted volume of ancillary services, per reserve type, per hour                                                                   | MW           | Market (hourly or sub hourly) | Locational granularity, reserve type | Balancing Region       | Real-time    | Real-time |             |

### Unit

| Data type                          | Definition                                                                                                                         | Unit                  | Temporal granularity          | Secondary information   | Locational granularity | Time horizon | Latency | Max latency |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|-----------------------|-------------------------------|-------------------------|------------------------|--------------|---------|-------------|
| Unit Generation                    | Generation for each unit in each time interval                                                                                     | MWH                   | Market (hourly or sub hourly) | Unit ID                 | unit                   | historical   | Day +30 |             |
| Unit Emissions (by pollutant type) | Direct emissions from combustion of fuel                                                                                           | t                     | Market (hourly or sub hourly) | Unit ID, Pollutant type | unit                   | historical   | Day +30 |             |
| Installed Capacity                 | Max output of plant                                                                                                                | MW                    | Monthly                       | Unit ID                 | unit                   | Historical   | Annual  |             |
| Unit Heat Rates                    | The amount of energy used by an electrical generator/power plant to generate one kWh of electricity                                | J/kWh                 | Monthly                       | Unit ID                 | unit                   | Historical   | Annual  |             |
| Heat Rates Curves                  | The amount of energy used by an electrical generator/power plant to generate one kWh of electricity at different generation levels | J/kWh                 | Monthly                       | Unit ID                 | unit                   | Historical   | Annual  |             |
| Unit fuel types                    | Fuel consumed by each unit                                                                                                         | list of types         | Annual                        | Unit ID                 | unit                   | historical   | Annual  |             |
| Unit Location                      |                                                                                                                                    | Lat Long (?)          | Annual                        | Unit ID                 |                        |              |         |             |
| Unit Type                          |                                                                                                                                    | Generation Technology | Annual                        | Unit ID                 |                        |              |         |             |
| Unit prime mover                   |                                                                                                                                    |                       | Annual                        |                         |                        |              |         |             |
| Unit market                        | Energy market of the unti                                                                                                          |                       |                               |                         |                        |              |         |             |
| Unit owner information             |                                                                                                                                    |                       |                               |                         |                        |              |         |             |
| Unit ID number                     |                                                                                                                                    | string                |                               |                         |                        |              |         |             |
| Unit name                          |                                                                                                                                    | string                |                               |                         |                        |              |         |             |
| Unit start year                    |                                                                                                                                    | integer               |                               |                         |                        |              |         |             |
| Parent unit ID                     | Optional, ID of the parent entity, like a unit or generator's plant ID                                                             | string                |                               |                         |                        |              |         |             |
| Co-generation Heat Output          | Total Heat produced in the cogeneration heation                                                                                    | J                     |                               |                         |                        |              |         |             |

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
