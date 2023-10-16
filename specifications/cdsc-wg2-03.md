# _CDSC-WG2-03 - Endpoints_ 

Version: _0.0.1_

Status: Pre-Draft
 
© _________________

This specification is subject to the Community Specification License 1.0, available at [https://github.com/CommunitySpecification/1.0](https://github.com/CommunitySpecification/1.0).
© _________________

This specification is subject to the license, available [here][LICENSE].

## Contents <a id="table-of-contents" href="#table-of-contents" class="permalink">🔗</a>
* [1. Foreword](#foreword)  
* [2. Introduction](#introduction)
* [3. Endpoint Categories](#endpoints)  
  * [3.1 Metadata ](#endpoints-metadata)
  * [3.2 PowerSystemResource Metadata](#endpoints-psrmeta)
  * [3.3 PowerSystemResource Timeseries Data](#endpoints-psrtime)


## 1. Foreword <a id="foreword" href="#foreword" class="permalink">🔗</a>

Attention is drawn to the possibility that some of the elements of this document may be the subject of patent rights. No party shall not be held responsible for identifying any or all such patent rights.

Any trade name used in this document is information given for the convenience of users and does not constitute an endorsement.

This document was prepared by [Insert name of group].

This second/third/… edition cancels and replaces the first/second/… edition (#####:####), which has been technically revised.
The main changes compared to the previous edition are as follows:
—	xxx xxxxxxx xxx xxxx

Known patent licensing exclusions are available in the specification’s repository’s Notices.md file.

Any feedback or questions on this document should be directed to specifications repository, located at _________________.

THESE MATERIALS ARE PROVIDED “AS IS.” The Contributors and Licensees expressly disclaim any warranties (express, implied, or otherwise), including implied warranties of merchantability, non-infringement, fitness for a particular purpose, or title, related to the materials.  The entire risk as to implementing or otherwise using the materials is assumed by the implementer and user. IN NO EVENT WILL THE CONTRIBUTORS OR LICENSEES BE LIABLE TO ANY OTHER PARTY FOR LOST PROFITS OR ANY FORM OF INDIRECT, SPECIAL, INCIDENTAL, OR CONSEQUENTIAL DAMAGES OF ANY CHARACTER FROM ANY CAUSES OF ACTION OF ANY KIND WITH RESPECT TO THIS DELIVERABLE OR ITS GOVERNING AGREEMENT, WHETHER BASED ON BREACH OF CONTRACT, TORT (INCLUDING NEGLIGENCE), OR OTHERWISE, AND WHETHER OR NOT THE OTHER MEMBER HAS BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## 2. Introduction <a id="introduction" href="#introduction" class="permalink">🔗</a>
This specification was developed as part of the global effort to combat the climate crisis. Specifically, in order to scalably measure carbon emissions of organizations and calculate the impact of deploying and operating clean energy technologies, companies need an efficient means to discover the details and capabilities of energy utilities and other similar entities.

This specification offers a specification for a set of endpoints that would contain the necessary information for carbon data calculations.
 
###	Normative references

The following documents are referred to in the text in such a way that some or all of their content constitutes requirements of this document. For dated references, only the edition cited applies. For undated references, the latest edition of the referenced document (including any amendments) applies.

- IEC 61968/61970: Common Information Model 
- ISO 4217: Currency Codes

## 3. Endpoint Categories <a id="endpoints" href="#endpoints" class="permalink">🔗</a>

The following is a list of the endpoints that will be subsequently defined in this section.
```
# Metadata
/metadata/topology-types (LIST)
/metadata/fuel-source/types (LIST)
/metadata/fuel-source/technologies (LIST)
/power-system-resources (LIST) 
# PSR-Specific Metadata
/power-system-resources/{id}/describe (GET)
/power-system-resources/{id}/capacity (GET)
/power-system-resources/{id}/transmission-capacity (GET)
/power-system-resources/{id}/topology (GET)
# Timeseries Data
/power-system-resources/{id}/timeseries/generation (GET)
/power-system-resources/{id}/timeseries/demand (GET)
/power-system-resources/{id}/timeseries/imports (GET)
/power-system-resources/{id}/timeseries/exports (GET)
/power-system-resources/{id}/timeseries/price (GET)
```

### 3.1 Metadata<a id="endpoints-metadata" href="#endpoints-metadata" class="permalink">🔗</a>
ResourceType objects represent a specific hierarchical level of the grid. 

#### 3.1.1 Topology Type (List) `metadata/topology-types`

##### Response Object
- `id` - _string_ - (REQUIRED) - The unique identifier representing this resource. It **may** be human-readable, such as `US-WECC-CISO`.
- `level` - int - (OPTIONAL) - A number representing the hierarchy of this resource topology in relation to the other resource types. These levels **shall** include a sequential set of positive integers starting at 0.
##### Example
```
==Request==
GET metadata/topology-types HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
"topology_types": [
			{
		     "id": "Interconnection",
		     "level": 0
			},
			{
		     "id": "Balancing Area",
		     "level": 1
			},
		  {
		     "id": " Generating Plant",
		     "level": 2
			},
			{
		     "id": " Generating Unit",
		     "level": 3
			},
		  {
		     "id": " Consumption Unit",
		     "level": 3
			},
	],
  "next": null,
  "previous": null
}
```
##### Reference TopologyTypes
The following table shows an example list of topology types for US and European Grids:
|Level|US Grid|European Grid|CIM|
|--|--|--|--|
|0|Interconnection|Synchronous area|GeographicalRegion |
|1|Balancing Authority Area|LFC block/LFC area|GeographicalRegion, SubGeographicalRegion|
|2|Zone|Bidding zone|SubGeographicalRegion |
|3|Transmission Node/Substation|Control area|Substation |
|4|Generating Plant|Scheduling Area/Sub scheduling area|Feeder,GeneratingUnit|
|5|Meter (Generator or Load)|Metering Grid Area, MeteringPoint|GeneratingUnit |

#### 3.1.2 Fuel Source - Type (LIST) `metadata/fuel-source/types`
***TODO: This depends on if we want to fully adopt AIB or whether we want to allow for a more flexible solution.*** [Github Issue](https://github.com/carbon-data-specification/Power-Systems-Data/issues/72)

##### Response Object
- `name`: - _string_ - (REQUIRED) - A common name to use for the fuel type. If using AIB codes, it should be a concatenation of the three code descriptions with a dash between (i.e. `Solar - Photovoltaic - Unspecified`)
- `external_reference`: _String_ - (OPTIONAL?) - A reference that provides context for this specific fuel type.
- `external_id`:  - _String_ - (OPTIONAL?) - A unique code (***AIB code?***) referencing the type of fuel

##### Example
```
==Request==
GET /metadata/fuel-types HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
   "types": [
	   {
		   "name": "Solar - Photovoltaic - Unspecified",
		   "external_reference": "EECS Rules Fact Sheet 5 TYPES OF ENERGY INPUTS AND TECHNOLOGIES",
		   "external_id": "T010100"
	   },
	   {
		   "name": "Fossil - Solid - Hard Coal - Unspecified",
		   "external_reference": "EECS Rules Fact Sheet 5 TYPES OF ENERGY INPUTS AND TECHNOLOGIES",
		   "external_id": "F02010100"
	   },
  ],
  "next": null,
  "previous": null
}
```
#### 3.1.3 Fuel Source - Technology (LIST) `metadata/fuel-source/technologies`
***TODO: This depends on if we want to fully adopt [AIB](https://www.aib-net.org/sites/default/files/assets/eecs/facts-sheets/AIB-2019-EECSFS-05%20EECS%20Rules%20Fact%20Sheet%2005%20-%20Types%20of%20Energy%20Inputs%20and%20Technologies%20-%20Release%207.7%20v5.pdf) or whether we want to allow for a more flexible solution.*** [Github Issue](https://github.com/carbon-data-specification/Power-Systems-Data/issues/72)

##### Response Object
- `name`: - _string_ - (REQUIRED) - A common name to use for the technology. If using AIB codes, it should be a concatenation of the three code descriptions with a dash between (i.e. `Solar - Photovoltaic - Unspecified`)
- `externalReference`: _Dict_ - (OPTIONAL?) - A reference that provides context for this specific technology. ***TODO: This can be more specifically defined if we decide to go with AIB*** [Github Issue](https://github.com/carbon-data-specification/Power-Systems-Data/issues/72)


```json
{
   "technologies": [
	   {
		   "name": "Mechanical source or other - Wind - Unspecified",
		   "externalReference": {
			   "sourceDocument": "EECS Rules Fact Sheet 5 TYPES OF ENERGY INPUTS AND TECHNOLOGIES",
			   "aibCode": "F01050100"
		   }
	   },
	   {
		   "name": "Thermal - Steam engine - Unspecified",
		   "externalReference": {
			   "sourceDocument": "EECS Rules Fact Sheet 5 TYPES OF ENERGY INPUTS AND TECHNOLOGIES",
			   "aibCode": "T050900"
			 }
	   },
  ],
  "next": null,
  "previous": null
}
```

### 3.2 PowerSystemsResources Metadata<a id="endpoints-psrmeta" href="#endpoints-psrmeta" class="permalink">🔗</a>

The primary set of endpoints reference PowerSystemResource (PSR) objects. These objects contain several metadata fields as well as timeseries information such as generation, demand, and capacity.

#### 3.2.1 PSR List `/power-system-resources`
##### Request Object
- `type`: _string_ - (OPTIONAL) - An optional filter to only return PSR objects with the given *type*.

##### Response Object
- `id` - _string_ - (REQUIRED) - The unique identifier representing this resource. It SHOULD be human-readable, and where appropriate, MAY incorporate the `id` of its parent objects in order to easily understand its place in the topology. An example of such an id is `US-WECC-CISO`. The `id` MUST be URL safe. 
- `type` - _string_ - (REQUIRED) - The id of the resource type for this PowerSystemsResource.
-  `name` - _string_ - (OPTIONAL) - A descriptive name to provide additional context to the PSR.

***TODO: Placeholder spot for where we can decide what rules/guidelines/parameters/suggestions would be good for generating human-readable PSR `id`s*** [Github Issue](https://github.com/carbon-data-specification/Power-Systems-Data/issues/80)

#### Example
The following is an example of the endpoint that returns a list of power system resources. This LIST endpoint SHOULD only includes the `id`, `name`, and `type` fields. It MUST not contain fields of undefined size (such as fields that con contain lists or dicts), as this endpoint is meant to be capable of returning several entries.
```
==Request==
GET /power-system-resources HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
"power_system_resources": [
			{
		     "id": "US-WECC",
		     "name": "Western Electricity Coordinating Council",
		     "type": "Interconnection"
			},
			{
		     "id": "US-WECC-CISO",
		     "name": "California ISO",
		     "type": "Balancing Area"
			},
			{
		     "id": "US-WECC-CISO-ABC-HYDRO",
		     "name": "Hydropower Plant in ABC",
		     "type": "Generating Plant"
			},
	],
  "next": null,
  "previous": null
}
```
#### 3.2.2 PSR Describe `/power-system-resources/{id}/describe`

##### Response Object
- `id` - _string_ - REQUIRED - The `id` of the PowerSystemResource associated with this location.
- `describe`
	-  `asset_info` - _Array[[AssetInfo](https://zepben.github.io/evolve/docs/cim/cim100/TC57CIM/IEC61968/Assets/AssetInfo/)]_ - (OPTIONAL) - A list of additional information associated with that PSR.
	- `location` - (OPTIONAL)
		- `main_address` - [StreetAddress](https://zepben.github.io/evolve/docs/cim/evolve/IEC61968/Common/StreetAddress) - (OPTIONAL) 
			- `postal_code`
			- `town_detail` - TownDetail
				- `name`
				- `state_or_province`
			- `street_detail` - StreetDetail
				- `building_name`
				- `display_address`
				- ...
		- `description` - _string_ - (OPTIONAL)
		- `geojson` - _geojson_ - (OPTIONAL)

The following endpoint returns the location information for a specific PSR.

##### Example
```
==Request==
GET /power-system-resources/US-WECC-CISO/describe HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
  "id": "US-WECC-CISO",
  "describe": {
    "location": {
	    "geojson": {
		    "type": "FeatureCollection",
		    "features": [
		    {
	           "type": "Feature",
	           "geometry": {
	               "type": "Polygon",
	               "coordinates": [
	                   [[-122.376131, 42.009499],...]
	               ]
			       }
			  },
		  ]
		},
  "next": null,
  "previous": null
}
```

#### 3.2.3 PSR Topology `/power-system-resources/{id}/topology`

The topology endpoint provides a means for understanding how each PSR relates to others. 

##### Request Object
- `numLevels` - _integer_ - Number of levels above and below the requested PSR to return in the response. A value of *1* means that it will only return that PSR's direct parent and children.

##### Response Object
- `id` - _string_ - REQUIRED - The `id` of the PowerSystemResource associated with this location.
- `topology` - _Object_
	- `parent` - _Object_ - (OPTIONAL)
		-  `id` - _String_ The unique identifier representing the *id* of the PSR that is one level above in the topology.
		- `parent` - _Object_ - (OPTIONAL)
			- ...
	- `connectedSiblings` - _Array_ - (OPTIONAL)
		- `id` - _String_ - A unique identifier representing the *id* of a PSR that has some transmission-based connection.
	  - `connectedSiblings` - _Array_ - (OPTIONAL)
			- `id` - _String_
	- `children` - _Array_ - (OPTIONAL)
		-  `id` - _String_ - A unique identifier representing the *id* of a PSR that is one level below in the topology.
		- `connectedSiblings` - _Array_ - (OPTIONAL)
		- `children` - _Array_ - (OPTIONAL)
			- ...
```
==Request==
GET /power-system-resources/US-WECC-CISO/topology?numLevels=2 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
  "id": "US-WECC-CISO",
  "topology": {
	  "parent": {"id": "US-WECC"},
	  "connectedSiblings": {"id": "US-ERCOT"},
	  "children": [
			  {"id": "US-WECC-CISO-ABC", "children": []}
			],
	},
  "next": null,
  "previous": null
}
```

#### 3.2.4 PSR Capacity `/power-system-resources/{id}/capacity`

The capacity endpoint provides a means for providing capacity information by fuel type and technology.

##### Response Object
- `id` - _string_ - REQUIRED - The `id` of the PowerSystemResource associated with this location.
- `unit` - _string_ - (REQUIRED) - For electricity, SHOULD be one of:  [`MW`, `kW`, `W`]
- `capacity` - _Array_
	 - `fuelSource` - _Object_
		 - `technology` - _String_ - (OPTIONAL) - *id* of the technology for generating this fuel.
		  - `type` - _String_ - (REQUIRED) - *id* of the fuel type used for generation.
	- `value` - _float_ - A value of the amount of generation that took place at this PSR using the given *technology* and *fuel_source*.
	-  `startDatetime` - _ISO8601 Datetime_ - (REQUIRED) - The datetime MUST be timezone aware. This allows for the defining of historical capacity values and to indicate when new resources came online.
	-   `endDatetime` - _ISO8601 Datetime_ - (OPTIONAL)  - The datetime MUST be timezone aware. This allows for the defining of historical capacity values and to indicate when old resources came offline. An empty value assumes it is still operational.
```
==Request==
GET /power-system-resources/US-WECC-CISO/topology?numLevels=2 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
  "id": "US-WECC-CISO",
  "unit": "MW",
  "capacity": [
	  {
		  "fuelSource": {
			  "technology": "Thermal - Steam engine - Unspecified",
			  "type": "Fossil - Solid - Hard Coal - Unspecified",
		  "value": 500,
		  "startDatetime": "2015-06-01 00:00:00+00",
		  "endDatetime": "2021-06-01T0 :00:00+00",
	  },
	  	  {
		  "technology": "Thermal - Steam engine - Unspecified",
		  "fuelSource": "Fossil - Solid - Hard Coal - Unspecified",
		  "value": 500
	  },
  ],
  "next": null,
  "previous": null
}
```

#### 3.2.5 PSR Topology `/power-system-resources/{id}/transmission-capacity`

The topology endpoint provides a means for understanding how each PSR relates to others. 

##### Response Object
- `id` - _String_ - REQUIRED - The `id` of the PowerSystemResource associated with this location.
 `unit` - _String_ - (REQUIRED) - For electricity, SHOULD be one of:  [`MW`, `kW`, `W`]
- `transmissionCapacity` - _Array_
	- `connectedPSR` - _Object_ 
		- `id`  - _String_ The unique identifier representing the *id* of the PSR connected to the requested PSR.
	 - `value` - _float_ - A value of the amount of transmission capacity available between the two PSRs. 
```
==Request==
GET /power-system-resources/US-WECC-CISO/transmission-capacity HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
  "id": "US-WECC-CISO",
  "unit": "MW",
  "transmissionCapacity": [
	  {
		  "connectedPSR": {"id": "US-WECC-NEVP"},
		  "value": 100
	  },
	  {
		  "connectedPSR": {"id": "US-WECC-ABCD"},
		  "value": 50
	  },...
	],
  "next": null,
  "previous": null
}
```

### 3.3 PowerSystemsResources Timeseries Data<a id="endpoints-psrtime" href="#endpoints-psrtime" class="permalink">🔗</a>

#### 3.3.1 PSR Generation `/power-system-resources/{id}/generation`
A generation object returns a timeseries of values representing energy that was generated at a PSR, as well as a breakdown of that generation by fuel type.

##### Request Object
- `startDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *startDatetime* occurs AT or AFTER the *startDatetime* specified in the request.  
- `endDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *endDatetime* occurs AT or BEFORE the *endDatetime* specified in the request.  

##### Response Object
-  `id` - _string_ - (REQUIRED) - The `id` of the Power System Resource associated with this unit of generation.
- `unit` - _string_ - (REQUIRED) - For electricity, MUST be one of:  [`MWh`, `kWh`, `Wh`]
- `generation` - _Array_
	-  `startDatetime` - _ISO8601 Datetime_ - (REQUIRED) - The datetime MUST be timezone aware.
	-   `endDatetime` - _ISO8601 Datetime_ - (REQUIRED)  - The datetime MUST be timezone aware.
	- `value` - _float_ - (REQUIRED) - A value of the amount of generation that took place at this PSR. A positive number indicates generation.
	- `valueByFuelSource` - _Array_ - (REQUIRED) - Lists of fuel types, technologies, and the amount of generation that comes from that fuel type. The unit for these values MUST be the same as that of the `unit` field.
	  - `technology` - _String_ - (OPTIONAL) - *id* of the technology that was generated from this fuel.
	  - `type` - _String_ - (REQUIRED) - *id* of the fuel type used for generation.
	  - `value` - _float_ - A value of the amount of generation that took place at this PSR using the given *technology* and *fuel_source*.

```
==Request==
GET /power-system-resources/US-WECC-CISO/generation?startDatetime=2022-06-01&endDatetime=201-06-02 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
	"id": "US-WECC-CISO",
	"unit": "MWh",
	"generation": [
	  {
		  "startDatetime": "2021-06-01 00:00:00+00",
		  "endDatetime": "2021-06-01 01:00:00+00",
		  "value": 10.5,
		  "valueByFuelSource": [
			  {
			    "technology": "Thermal - Steam engine - Unspecified",
			    "type": "Fossil - Solid - Hard Coal - Unspecified",
			    "value": 5.0
			  },
			  {
			    "technology": "Solar - Photovoltaic - Unspecified",
			    "type": "Renewables - Heating and Cooling - Solar",
			    "value": 10.5
			  },
			},...
	  }
	],
	"next": null,
	"previous": null
}
```

#### 3.3.2 PSR Demand `/power-system-resources/{id}/demand`
A demand object returns a timeseries of values representing energy that was demanded at a PSR.

##### Request Object
- `startDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *startDatetime* occurs AT or AFTER the *startDatetime* specified in the request.  
- `endDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *endDatetime* occurs AT or BEFORE the *endDatetime* specified in the request.  

##### Response Object
-  `id` - _string_ - (REQUIRED) - The `id` of the Power System Resource associated with this demand.
- 	`unit` - _string_ - (REQUIRED) - For electricity, SHOULD be one of:  [`MWh`, `kWh`, `Wh`]
- `demand` - _Array_
	-  `startDatetime` - _ISO8601 Datetime_ - (REQUIRED) - The datetime MUST be timezone aware.
	-   `endDatetime` - _ISO8601 Datetime_ - (REQUIRED)  - The datetime MUST be timezone aware.
	- `value` - _float_ - (REQUIRED) - A value of the amount of demand that took place at this PSR. A positive number indicates demand that was needed. A negative number represents energy that was generated by un-registered demand-side resources.
```
==Request==
GET /power-system-resources/US-WECC-CISO/demand?startDatetime=2022-01-01&endDatetime=2023-01-01 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
	"id": "US-WECC-CISO",
	"unit": "MWh",
	"demand": [
	  {
		  "startDatetime": "2021-06-01 00:00:00+00",
		  "endDatetime": "2021-06-01 01:00:00+00",
		  "value": 10.5,
	  },...
	],
	"next": null,
	"previous": null
}
```

#### 3.3.3 PSR Imports `/power-system-resources/{id}/imports`
An import object returns a timeseries of values representing energy that was imported at a PSR. 

##### Request Object
- `startDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *startDatetime* occurs AT or AFTER the *startDatetime* specified in the request.  
- `endDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *endDatetime* occurs AT or BEFORE the *endDatetime* specified in the request.  

##### Response Object
-  `id` - _string_ - (REQUIRED) - The `id` of the Power System Resource associated with this import.
- 	`unit` - _string_ - (REQUIRED) - For electricity, SHOULD be one of:  [`MWh`, `kWh`, `Wh`]
- `imports` - _Array_
	-  `startDatetime` - _ISO8601 Datetime_ - (REQUIRED) - The datetime MUST be timezone aware.
	-   `endDatetime` - _ISO8601 Datetime_ - (REQUIRED)  - The datetime MUST be timezone aware.
	- `value` - _positive float_ - (REQUIRED) - A value of the amount of energy imported by this PSR. The value must be 0 or a positive value (exports should be provided in the other endpoint).
	  - `valueByConnectedPSR` - _Array_ - (REQUIRED) - Key-value pairs of the connected PSR that this PSR is importing energy from and the amount of energy that comes from that PSR. The unit for these values MUST be the same as that of the `unit` field.
	  - `connectedPSR` - _String_
	  - `value` - _positive float_ - A value of the amount of energy imported from the `connectedPSR`. he value must be 0 or a positive value (exports should be provided in the other endpoint).
```
==Request==
GET /power-system-resources/US-WECC-CISO/imports?startDatetime=2022-01-01&endDatetime=2023-01-01 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
	"id": "US-WECC-CISO",
	"unit": "MWh",
	"imports": [
	  {
		  "startDatetime": "2021-06-01 00:00:00+00",
		  "endDatetime": "2021-06-01 01:00:00+00",
		  "value": 10.5,
		   "importByConnectedPSR": [
			  {
			    "connectedPSR": "US-WECC-LADWP",
			    "value": 5.5
			  },
			  {
			    "connectedPSR": "US-WECC-BANC",
			    "value": 5
			  },
			},...
	  },...
	],
	"next": null,
	"previous": null
}
```

#### 3.3.4 PSR Exports `/power-system-resources/{id}/exports`
An export object returns a timeseries of values representing energy that was exported to a PSR. 

##### Request Object
- `startDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *startDatetime* occurs AT or AFTER the *startDatetime* specified in the request.  
- `endDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *endDatetime* occurs AT or BEFORE the *endDatetime* specified in the request.  

##### Response Object
-  `id` - _string_ - (REQUIRED) - The `id` of the Power System Resource associated with this export.
- 	`unit` - _string_ - (REQUIRED) - For electricity, SHOULD be one of:  [`MWh`, `kWh`, `Wh`]
- `exports` - _Array_
	-  `startDatetime` - _ISO8601 Datetime_ - (REQUIRED) - The datetime MUST be timezone aware.
	-   `endDatetime` - _ISO8601 Datetime_ - (REQUIRED)  - The datetime MUST be timezone aware.
	- `value` - _positive float_ - (REQUIRED) - A value of the amount of energy exported by this PSR. The value must be 0 or a positive value (imports should be provided in the other endpoint).
	  - `valueByConnectedPSR` - _Array_ - (REQUIRED) - Key-value pairs of the connected PSR that this PSR is exporting energy to and the amount of energy that comes from that PSR. The unit for these values MUST be the same as that of the `unit` field.
	  - `connectedPSR` - _String_
	  - `value` - _positive float_ - A value of the amount of energy exported to the `connectedPSR`. The value must be 0 or a positive value (exports should be provided in the other endpoint).
```
==Request==
GET /power-system-resources/US-WECC-CISO/exports?startDatetime=2022-01-01&endDatetime=2023-01-01 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
	"id": "US-WECC-CISO",
	"unit": "MWh",
	"exports": [
	  {
		  "startDatetime": "2021-06-01 00:00:00+00",
		  "endDatetime": "2021-06-01 01:00:00+00",
		  "value": 10.5,
		   "exportByConnectedPSR": [
			  {
			    "connectedPSR": "US-WECC-LADWP",
			    "value": 5.5
			  },
			  {
			    "connectedPSR": "US-WECC-BANC",
			    "value": 5.0
			  },
			},...
	  },...
	],
	"next": null,
	"previous": null
}
```

#### 3.3.5 PSR Prices `/power-system-resources/{id}/price`
A demand object returns a timeseries of values representing energy that was demanded at a PSR.

##### Request Object
- `startDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *startDatetime* occurs AT or AFTER the *startDatetime* specified in the request.  
- `endDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *endDatetime* occurs AT or BEFORE the *endDatetime* specified in the request.  
- `timeframe` - _Enum_ - One of `day-ahead`, `hour-ahead`, `real-time`

##### Response Object
-  `id` - _string_ - (REQUIRED) - The `id` of the Power System Resource associated with this demand.
- 	`unit` - _string_ - (REQUIRED) - Should be a a currency code in accordance with the [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) standard. 
- `timeframe` - _String_ The selected `timeframe` from the request object
- `demand` - _Array_
	-  `startDatetime` - _ISO8601 Datetime_ - (REQUIRED) - The datetime MUST be timezone aware.
	-   `endDatetime` - _ISO8601 Datetime_ - (REQUIRED)  - The datetime MUST be timezone aware.
	- `value` - _float_ - (REQUIRED) - A value of the price of energy  took place at this PSR. 
```
==Request==
GET /power-system-resources/US-WECC-CISO/price?startDatetime=2022-01-01&endDatetime=2023-01-01 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
	"id": "US-WECC-CISO",
	"unit": "USD",
	"price": [
	  {
		  "startDatetime": "2021-06-01 00:00:00+00",
		  "endDatetime": "2021-06-01 01:00:00+00",
		  "value": 10.5,
	  },...
	],
	"next": null,
	"previous": null
}
```

#### 3.3.6 PSR Curtailment `/power-system-resources/{id}/curtailment`
A curtailment object returns a timeseries of values representing energy that was generated at a PSR, as well as a breakdown of that generation by fuel type.

##### Request Object
- `startDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *startDatetime* occurs AT or AFTER the *startDatetime* specified in the request.  
- `endDatetime` - _ISO8601 Datetime_ -  A datetime indicating that the response should return data where the earliest entry's *endDatetime* occurs AT or BEFORE the *endDatetime* specified in the request.  

##### Response Object
-  `id` - _string_ - (REQUIRED) - The `id` of the Power System Resource associated with this unit of curtailment.
- `unit` - _string_ - (REQUIRED) - For electricity, MUST be one of:  [`MWh`, `kWh`, `Wh`]
- `generation` - _Array_
	-  `startDatetime` - _ISO8601 Datetime_ - (REQUIRED) - The datetime MUST be timezone aware.
	-   `endDatetime` - _ISO8601 Datetime_ - (REQUIRED)  - The datetime MUST be timezone aware.
	- `value` - _float_ - (REQUIRED) - A value of the amount of curtailment that took place at this PSR. A positive number indicates curtailment.
	- `valueByFuelSource` - _Array_ - (REQUIRED) - Lists of fuel types, technologies, and the amount of curtailment that occurs at that fuel type. The unit for these values MUST be the same as that of the `unit` field.
	  - `technology` - _String_ - (OPTIONAL) - *id* of the technology that this fuel type used for curtailment.
	  - `type` - _String_ - (REQUIRED) - *id* of the fuel source used for curtailment.
	  - `value` - _float_ - A value of the amount of curtailment that took place at this PSR using the given *technology* and *fuel_source*.

```
==Request==
GET /power-system-resources/US-WECC-CISO/curtailment?startDatetime=2022-06-01&endDatetime=201-06-02 HTTP/1.1
Host: demoutility.com

==Response==
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
```
```json
{
	"id": "US-WECC-CISO",
	"unit": "MWh",
	"curtailment": [
	  {
		  "startDatetime": "2021-06-01 00:00:00+00",
		  "endDatetime": "2021-06-01 01:00:00+00",
		  "value": 10.5,
		  "valueByFuelSource": [
			  {
			    "technology": "Thermal - Steam engine - Unspecified",
			    "type": "Fossil - Solid - Hard Coal - Unspecified",
			    "value": 5.0
			  },
			  {
			    "technology": "Solar - Photovoltaic - Unspecified",
			    "type": "Renewables - Heating and Cooling - Solar",
			    "value": 10.5
			  },
			},...
	  }
	],
	"next": null,
	"previous": null
}
```
