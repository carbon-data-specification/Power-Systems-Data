# Data model initial suggestion

This markdown is an initial data model suggestion (based on electricityMap data) for the Power Systems Data working group.

It is by no means meant to be exhaustive or complete, but rather to use as a starting point for discussion and elaboration.

Note that this may not be the best format, but I suggest we start here and evolve the document / format as we see fit.

## Data terms, definitions, types and other attributes

***NOTE:*** All of the following data are at an hourly temporal granularity. We can discuss whether / how to represent various temporal granularities.

Production class:
- Biomass
- Fossil
- Geothermal
- Hydro
- Marine
- Nuclear
- Other renewable
- Solar
- Waste
- Wind
- Other non-renewable
- Storage Discharge

Fuel type:
- Brown coal/Lignite
- Hard coal
- Coal-derived gas
- Other coal
- Natural gas
- Landfill gas
- Other gas
- Wood / Wood waste
- Municipal waste
- Propane oil
- Shale oil
- Distillate oil
- Other oil
- Peat
- Uranium, Thorium, Plutonium
- Solar
- Wind
- Geothermal
- Water

Technology:
- Energy Storage, Battery
- Energy Storage, Compressed Air
- Energy Storage, Concentrated Solar Power
- Energy Storage, Flywheel
- Energy Storage, Reversible Hydraulic Turbine
- Energy Storage, Other
- Fuel Cell
- Turbine, Binary Cycle
- Turbine, Combined-Cycle
- Turbine, Steam
- Turbine, Gas Combustion
- Hydrokinetic, Axial Flow Turbine
- Hydrokinetic, Wave Buoy
- Hydrokinectic, Other
- Hydraulic Turbine
- Internal Combustion
- Other
- Photovoltaic
- Wind Turbine, Onshore
- Wind Turbine, Offshore

Sub-Class - a combination of Class + Technology + Fuel

## STATIC DATA
These data describe grid charateristics that change infrequently, but needs to be updated and tracked. 

### Endpoint attributes
For each data timeseries, it should be associated with the most granular node/edge in the topological definition. 


<br>     

### Node Attributes
This refers to nodes on the topological mapping of the power sytem. A node could be an actual node, or a region. 

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|latitude                                |Decimal Degrees  |float|Latitude|If the node represents a region, the lat/long should represent the region's centroid. Lat/long will be used to identify the local prevailing time zone for each data endpoint.  
|longitude                                |Decimal Degrees  |float|Longitude|If the node represents a region, the lat/long should represent the region's centroid. Lat/long will be used to identify the local prevailing time zone for each data endpoint. 


<br> 

### Edge Attributes
This refers to edges that connect nodes at each level of spatial granularity. This edges should generally represent distribution or transmission lines that connect each node, and thus the static properties will generally represent the characteristics of these lines.

<br> 

## DYNAMIC DATA
These data describe grid charateristics that are updated frequently. 

### Timestamp
***NOTE:*** Timestamps should only be provided in UTC. Information to convert timestamp into local prevailing time will be a static property of the spatial "node" with which each data source is associated.

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|datetime_utc                                |ISO format (UTC)  |datetime|ex: 2018-31-01T00:00:00+00:00                                             |The datetime represents the start of hour e.g. 16:00 represents data from 16:00-16:59   
<br> 
### Carbon data

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|emission_factor_{production_mode}_avg   |gCO2eq/kWh        |numeric |The emission intensity when generating using a specific mode of production|This can be lifecycle, direct...                                                                                            |
|carbon_intensity_average_avg            |gCO2eq/kWh        |numeric |The carbon intensity of the grid in eq. grams of CO2                      |This accounts for all imports/exports, as well as any storage/discharge in the grid                                         |

<br>

### Production

***NOTE:*** Many of these production modes are aggregates of multiple production types. By production type, we refer to the kind of process that lead to the electricity generation e.g. geothermal, hydro... etc.

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|power_production_biomass_avg            |MW                |numeric |Aggregate generation from biomass                                         |This also includes waste                                                                                                    |
|power_production_coal_avg               |MW                |numeric |Aggregate generation from coal-related fuels                              |This includes coal, lignite, oil shale, and fossil peat                                                                     |
|power_production_gas_avg                |MW                |numeric |Aggregate generation from gas fuels                                       |This includes coal-derived gas and natural gas                                                                              |
|power_production_hydro_avg              |MW                |numeric |Aggregate generation from hydro                                           |This includes both run-of-the-river and reservoirs. Does not include pumped hydro                                           |
|power_production_nuclear_avg            |MW                |numeric |Aggregate generation from nuclear                                         |                                                                                                                            |
|power_production_oil_avg                |MW                |numeric |Aggregate generation from oil fuels                                       |                                                                                                                            |
|power_production_solar_avg              |MW                |numeric |Aggregate generation from any PV technology                               |This includes solar across the grid ie transmission down to behind-the-meter (rooftop). Does not include off-grid generation|
|power_production_wind_avg               |MW                |numeric |Aggregate generation from any wind-based technology                       |This includes both onshore and offshore wind                                                                                |
|power_production_geothermal_avg         |MW                |numeric |Aggregate generation from geothermal                                      |                                                                                                                            |
|power_production_unknown_avg            |MW                |numeric |Aggregate generation from any technology not categorised above            |This includes marine, other renewable, and other unknown                                                                    |
<br>

### Power storage and exchanges

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|power_net_battery_discharge_avg         |MW                |numeric |Aggregate generation or storage from battery technology                   |This includes technologies such as utility-scale battery energy storage. Positive when discharging, negative when charging  |
|power_net_hydro_discharge_avg           |MW                |numeric |Aggregate generation or storage from pumped-hydro                         |Positive when discharging, negative when charging                                                                           |
|power_net_import_{neighbouring_zone}_avg|MW                |numeric |Net import at exchange from neighbouring grid zone                        |Positive when importing, negative when exporting                                                                            |

<br>

### Consumption

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|power_consumption_biomass_avg           |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_coal_avg              |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_gas_avg               |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_hydro_avg             |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_nuclear_avg           |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_oil_avg               |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_solar_avg             |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_wind_avg              |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_geothermal_avg        |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_unknown_avg           |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_battery_discharge_avg |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |
|power_consumption_hydro_discharge_avg   |MW                |numeric |Same as for production, though accounting for imports/exports             |                                                                                                                            |

<br>

### Price

|Data column                             |Unit              |Type    |Definition                                                                |Notes                                                                                                                       |
|----------------------------------------|------------------|--------|--------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|latest_forecasted_price_avg             |Local currency/MWh|numeric |Day ahead wholesale market clearing price
