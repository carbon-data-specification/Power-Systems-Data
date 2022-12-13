---
layout: base
nav: use_cases
title: Use Cases
meta_description: What are the use cases that the Power System Data specifications are trying to address?
---
[Home]({{ "/" | relative_url }}) / Use Cases

# Use Cases

The Power Systems Data working group is defining specifications. They must address several use cases where a standardised access to power data can enable decarbonation pathways.

These use cases highlight the need for consistent data reporting accross different power system operators. They indeed all require the collaboration between that data and modelling tools that estimate physical properties of the grids. Typically, running a carbon model to compute emissions across grids is only possible if the data is consistent and reliable.

## Use Case 1: Carbon Accounting <a id="use-case-carbon-accounting" href="#use-case-carbon-accounting" class="permalink">🔗</a>

Many companies, organizations, and governments around the world are working to
measure and track their carbon emissions. This is called
[carbon accounting](https://en.wikipedia.org/wiki/Carbon_accounting).

There are multiple methods to account for carbon emissions from power systems.
They can be classified into two main categories:

* __Location based__:

In this paradigm, carbon emissions are computed using the grid mix at the location of the consumption.

* __Market based__:

In this paradigm, electricity consumption can be matched with renewable energy certificates (_RECs_), bought from renewable generators.
Carbon emissions are then computed using two mechanisms. Consumption matched with RECs is claimed as carbon-free, and the rest of the unmatched consumption's emissions are computed from the grid mix at the location of the consumption, accounting for already claimed certificates (residual mix).

### Needs

For both the location-based and market-based (if not fully matched) approaches, it is required to have access to accurate grid mix data.

* __Grid mix data__: Temporaly and spatially granular data is a prerequisite to compute accurate carbon emissions. Power system operators should therefore disclose grid mix data, with a per production mode breakdown (e.g. coal, gas, nuclear, hydro, wind, solar, etc.). This data should be available at a minimum of hourly granularity, at the lowest possible geographical level (e.g. substation, feeder, etc.).

* __Consistency__: A lot of organisations doing carbon accounting have operations in different regions of the world. It is therefore important that the data is consistent across different power system operators. This means that the same production mode should be reported with the same name, and that the same geographical level should be reported with the same name.

### Example users

* Organisations aggregating grid data to help others with their carbon accounting ([Electricity Maps](https://www.electricitymaps.com/guides/accounting-guide), [Singularity](https://singularity.energy/), [WattTime](https://www.watttime.org/), etc.)

* Organisations looking to adopt the [24/7 Carbon-free Energy Compact](https://gocarbonfree247.com/) and other [ESG](https://en.wikipedia.org/wiki/Environmental%2C_social_and_corporate_governance) pledges

* Organisations that have pledged to be carbon free ([Google](https://www.google.com/about/datacenters/cleanenergy/), [Apple](https://www.apple.com/newsroom/2020/07/apple-commits-to-be-100-percent-carbon-neutral-for-its-supply-chain-and-products-by-2030/), [Microsoft](https://www.microsoft.com/en-us/corporate-responsibility/sustainability/operations), etc.)


## Use Case 2: Consumption Optimisation <a id="use-case-consumption-optimisation" href="#use-case-consumption-optimisation" class="permalink">🔗</a>

### Needs

### Example users


## Use cases NOT in scope for this working group <a id="not-in-scope" href="#not-in-scope" class="permalink">🔗</a>

* __Certificates__: Any work involving gathering RECs data is not technically power systems data. It is therefore not part of this scope. For this, see the work of the [customer data working group](https://customerdata.carbondataspec.org/)
