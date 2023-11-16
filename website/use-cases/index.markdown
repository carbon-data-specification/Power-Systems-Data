---
layout: base
nav: use_cases
title: Use Cases
meta_description: What are the use cases that the Power System Data specifications are trying to address?
---
[Home]({{ "/" | relative_url }}) / Use Cases

# Use Cases

The Power Systems Data working group is defining specifications for standardized grid data. They must address several use cases where a standardised access to power data can enable decarbonation pathways.

These use cases highlight the need for consistent data reporting accross different power system operators. They indeed all require the collaboration between that data and modelling tools that estimate physical properties of the grids. Typically, running a carbon model to compute emissions across grids is only possible if the data is consistent and reliable.

## Use Case 1: Carbon Accounting <a id="use-case-carbon-accounting" href="#use-case-carbon-accounting" class="permalink">🔗</a>

Many companies, organizations, and governments around the world are working to measure and track their carbon emissions. This is called
[carbon accounting](https://en.wikipedia.org/wiki/Carbon_accounting).

There are multiple methods to account for carbon emissions from power systems.
They can be classified into some categories:

* __Location based__:

In this paradigm, carbon emissions are computed using the average emissions factor at the location of the consumption.

* __Market based__:

In this paradigm, electricity consumption can be matched with renewable energy certificates (_RECs_) on a MWh basis, bought from renewable generators. Consumption matched with RECs is claimed as carbon-free, and the rest of the unmatched consumption's emissions are computed from the residual mix (average emissions factor at the location of the consumption, accounting for already claimed certificates).

* __Impact based__:

In this paradigm, the emissions impact of electricity consumption and generation are calculated using marginal emissions. 

### Needs

For all of these accounting approaches, it is required to have access to accurate and detailed grid emissions and generation data.

### Example users

* Organisations aggregating grid data to help others with their carbon accounting ([Electricity Maps](https://www.electricitymaps.com/guides/accounting-guide), [Singularity](https://singularity.energy/), [WattTime](https://www.watttime.org/), etc.)

* Organisations looking to adopt the [24/7 Carbon-free Energy Compact](https://gocarbonfree247.com/) and other [ESG](https://en.wikipedia.org/wiki/Environmental%2C_social_and_corporate_governance) pledges

* Organisations that have pledged to be carbon free ([Google](https://www.google.com/about/datacenters/cleanenergy/), [Apple](https://www.apple.com/newsroom/2020/07/apple-commits-to-be-100-percent-carbon-neutral-for-its-supply-chain-and-products-by-2030/), [Microsoft](https://www.microsoft.com/en-us/corporate-responsibility/sustainability/operations), etc.)

* Organisations that are measuring the impact of their electricity consumption and renewable energy generation as part of the ([Emissions First Partnership](https://www.emissionsfirst.com/)


## Use Case 2: Consumption Optimisation <a id="use-case-consumption-optimisation" href="#use-case-consumption-optimisation" class="permalink">🔗</a>

Decarbonization of electricity grids can be accelerated by optimising the consumption of electricity to match low emissions periods. This is called consumption optimisation or load shifting.  Examples include [load shifting car charges](https://evcharging.enelx.com/products/juicenet-green) or [compute jobs](https://blog.google/inside-google/infrastructure/data-centers-work-harder-sun-shines-wind-blows/).

### Needs

For these active load management strategies, it is required to have access to accurate and detailed grid emissions and generation data.

### Example users

* Organisations forecasting grid emissions to help others optimise their electricity consumption ([Electricity Maps](https://www.electricitymaps.com/guides/accounting-guide), [Singularity](https://singularity.energy/), [WattTime](https://www.watttime.org/), etc.)

* Organisations with climate goals that have the flexibility to shift their consumption ([Google](https://www.google.com/about/datacenters/cleanenergy/), [Apple](https://www.apple.com/newsroom/2020/07/apple-commits-to-be-100-percent-carbon-neutral-for-its-supply-chain-and-products-by-2030/), [Microsoft](https://www.microsoft.com/en-us/corporate-responsibility/sustainability/operations), etc.)

## Use Case 3: Renewable Project Siting <a id="use-case-renewable-siting" href="#use-case-renewable-siting" class="permalink">🔗</a>

Decarbonization can be accelerated by siting clean energy projects in electricity grids with higher emissions. 

### Needs

To make impactful clean energy siting decisions and to measure the carbon impact of existing clean energy projects, access to accurate and detailed grid data is required to calculate long-run grid emissions.

### Example users

* Organisations forecasting grid emissions to help others optimize their clean energy investments.

* Organisations with climate goals that can target their clean energy investments to grids with higher emissions.

## Use Case 4: Measurement of Decarbonization Projects <a id="use-case-decarb-projects" href="#use-case-decarb-projects" class="permalink">🔗</a>

Decarbonization can be accelerated through the accurate measuring of the impacts of various decarbonization projects, such as HVAC electrification and energy efficiency measures.  

### Needs

To make trustworthy claims on the impact of decarbonization projects, access to accurate and detailed grid data is required to calculate the carbon impacts based on changes in energy consumption. 

### Example users

* Organisations conducting measurement and verification of decarbonization projects.

* Organisations managing registries for carbon offsets or energy attribute certificates.

## Use cases NOT in scope for this working group <a id="not-in-scope" href="#not-in-scope" class="permalink">🔗</a>

* __Emissions Models__: This working group is not currently working on defining the models for calculating emissions factors based on the grid data. 

* __Certificates__: Any work involving gathering RECs data is not technically power systems data. It is therefore not part of this scope. For this, see the work of the [customer data working group](https://customerdata.carbondataspec.org/)
