---
version: 0.0.1
---

## Cloud Region Metadata 

### Introduction

Cloud providers are recognized as significant global procurers of renewable energy. This specification addresses the need for accurate and timely carbon information provision to customers utilizing cloud services, aiming to align with regulatory requirements across various jurisdictions.

Historically, cloud providers have supplied carbon information to their customers every month, often with a delay of several months. Consequently, customers have been compelled to estimate the real-time carbon footprint of their cloud workloads using incomplete public information, leading to overestimations.

To meet regulatory standards in the UK, Europe, California, and emerging elsewhere, cloud providers are required to supply real-time carbon metrics. Recognizing this need, cloud providers have developed custom silicon and system designs to optimize for low power consumption and mitigate the carbon footprint within the supply chain. The efficiency gains achieved, in conjunction with renewable energy purchases, enable direct comparisons with data centre alternatives for specific workloads.

This specification outlines the necessity for real-time carbon reporting to address these concerns and proposes a standardized approach to achieve accurate and timely carbon footprint estimates for cloud workloads. Additionally, it highlights the significance of metadata disclosure by cloud providers for the regions they operate in and the ongoing efforts to consolidate and distribute this information as a singular data source.


### Objective
This specification aims to standardize and clarify cloud region metadata for efficient and accurate usage by cloud service providers and users. It also aims to address discrepancies and variations in data reporting methodologies and definitions among different cloud providers and promote alignment toward standard definitions in future updates.

### Scope

The project aims to enhance the accuracy of the carbon emissions model for cloud-based workloads. This will involve establishing a standard mechanism for cloud providers to share more detailed and useful information using the same data schema. The scope also includes enabling real-time updates to provide minute-level granularity for energy usage and hourly or daily granularity for carbon intensity.

- **Cloud Region Metadata:**
   - Define standard parameters for cloud region metadata, including cloud provider and region specifications.
   - Establish guidelines for annual updates and data lag management (6-18 months), with emphasis on specifying the year or using the latest available data.
   - Clarify the annual average location-based marginal grid-carbon-intensity value for SCI-o, along with its availability and handling of NA data.
- **Standardizing Carbon Models and Data Reporting:**
   - Identify and clarify the multiple carbon models used by different cloud providers.
   - Address the variability of carbon data availability and handling of blank or not-available metrics.
- **Real-time Data Lookup and Provider Keys:**
   - Define the process for real-time lookup of cloud region data via APIs provided by data providers such as Electricity Maps and WattTime.
   - Establish protocols for annual average carbon intensity reporting for each grid region under each cloud provider's model.
- **Carbon-Free Energy and Renewable Energy Definitions:**
   - Define carbon-free energy and its inclusion of nuclear energy, which is distinct from the definition of renewable energy.
   - Address the absence of carbon-free energy data for regions that are not yet operational.
- **Power and Water Usage Effectiveness (PUE and WUE):**
   - Standardize reporting of power usage effectiveness (PUE) and water usage effectiveness (WUE) for each cloud region.
   - Align WUE reporting among cloud providers (e.g., Google matching what Azure provides) and address the variation in PUE data publication schedules.
- **Net Zero Reporting and Goals:**
   - Define the market method for calculating Net Zero goals, including energy-based offsets such as PPAs, RECs, and carbon offsets.
   - Reporting and aligning net carbon data on a region-by-region basis and addressing regions that achieve zero net carbon emissions.
- **Standard Definitions and Alignment:**
   - Establish guidelines for standard definitions and alignment of cloud region metadata, carbon models, and data reporting methodologies among cloud providers (e.g., AWS and Azure aligning with Google's location-based carbon data).

### Normative references
There are no normative references in this document.

### Terms and definitions

For the purposes of this document, the following terms and definitions apply.

ISO and IEC maintain terminological databases for use in standardization at the following addresses:
-	ISO Online browsing platform: available at https://www.iso.org/obp
-	IEC Electropedia: available at http://www.electropedia.org/

### Description

A user of the cloud region metadata can specify which cloud provider and region they use to run a workload and get all the relevant metadata about that region. Cloud region metadata is published annually and lags by 6-18 months, so the year must be specified, or the latest data should be used. The annual average location-based marginal grid-carbon-intensity value required for SCI-o is provided when available. Because of differences between cloud providers, data providers and reporting methodologies, there are several possible carbon models, and data may not be available (NA). Attempting to consume a not-available or blank metric should cause any calculations to fail.

The data provider keys for Electricity Maps and WattTime are returned to allow real-time lookup via their APIs, and the annual average carbon intensity is reported for each grid region for each cloud provider model.

Cloud providers have their own private carbon-free generation capacity, and they report a proportion of their energy consumption offset by carbon-free energy flowing within a “Carbon-Free Energy grid region”. This can reduce their effective grid carbon intensity and is taken into account by the market method that is used for Net Zero reporting, but is not included in the location based method that is required by SCI. The carbon-free energy calculation can be performed on a 24x7 hourly basis and accumulated over the year or on an annual total basis. CFE data is missing for regions that are not yet operational.

Carbon-free energy includes nuclear and is distinct from the definition of renewable energy.

Each cloud region has a power usage effectiveness (PUE) and a water usage effectiveness (WUE) that may be reported. Energy usage at the system level should be multiplied by the PUE ratio to account for losses due to cooling and energy distribution and storage within the cloud provider’s facilities. WUE is measured as litres per kilowatt and is reported for each Azure region. AWS provides a global average WUE, and Google does not currently provide WUE data [Gap: we request AWS and Google match what Azure provides].  PUE data is published on different schedules; Google currently provides quarterly and trailing 12-month data for data centre facilities that it owns, which is a subset of its cloud regions but doesn’t match the names of data centres to cloud region names. AWS doesn’t provide any specific PUE data but claims it operates in the global range from 1.07-1.15. Azure provides PUE and WUE data that matches all its regions. [Gap: We request that AWS and Google match what Azure provides.]

Cloud providers have Net Zero goals, calculated using the market method. This method allows for energy-based offsets, including private Power Purchase Agreements (PPAs), tradable Renewable Energy Credits (RECs), and carbon offsets. They report the net carbon on a region-by-region basis. For many regions, this is already zero.

Cloud providers have different definitions for the data they currently provide. Part of the goal of the GSF real-time cloud project is to clarify those differences and request that standard definitions and alignment occur in future updates. [Gap: Google provides location-based carbon data. Request AWS and Azure match what Google provides.]

### Metric Naming Scheme

- **Provider vs. Grid** - Some data is cloud **provider** specific, and some is generic data for the local **grid**.
- **24x7 vs. Hourly vs. Annual** - Some provider metrics use a 24x7 hourly energy matching scheme, and report data based on an **hourly** weighted average, this is labelled hourly (rather than 24x7). Other metrics are generated based on annual averages and labelled **annual**.
- **Location vs. Market** — The Greenhouse Gas Protocol specifies **location ** and **market ** methodologies for carbon reporting. Market methodology allows energy to be purchased across grids, but AWS states that it purchases within grids “wherever feasible” and reports **market** data on a per-grid basis.
- **Consumption vs. Production** - Within a grid, the energy sources add up to a production-based metric; however, energy flows between grids across interconnects, and the actual energy mix **consumption** in a region takes this into account.
- **Average vs. Marginal** - The **average** carbon intensity gives the total emissions mixture over a time period. The **marginal** emissions account for changes in demand and depend on what kind of energy source is used to supply variable demand, with other energy sources providing base load capacity. For example, many regions use gas-powered peaker plants overnight so that marginal carbon could be purely from gas. At other times, the same region may be curtailing solar power during the day so that marginal carbon would be purely from solar. The average carbon would report the proportional mix of these sources.
- **Not Available** - Accessing blank or unavailable data should cause an exception and interrupt an Impact Framework calculation.

### Cloud Region Metadata Table

| Name                                                 | Units                      | Example               | Description                                                                                                                                                                                                       |
| ---------------------------------------------------- | -------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| year                                                 | numeric                    | 2022                  | Specify which calendar year the data is averaged over. The IF timestamp is used to select a year.                                                                                                                 |
| cloud-provider                                       | string                     | “Google Cloud”        | Cloud Provider name. One of the three required input keys for IF model.                                                                                                                                           |
| cloud-region                                         | string                     | “asia-northeast-3”    | Cloud provider region. One of the three required input keys for IF model.                                                                                                                                         |
| cfe-region                                           | string                     | “South Korea”         | Carbon Free Energy grid region name as reported by the cloud provider.                                                                                                                                            |
| em-zone-id                                           | string                     | “KR”                  | Electricity Maps zone identifier for this region. Can be used to get real time data from their API.                                                                                                               |
| wt-region-id                                         | string                     | “KOR”                 | WattTime region identifier. Can be used to get real time data from their API.                                                                                                                                     |
| location                                             | string                     | “Seoul”               | Location of the region, as reported by the cloud provider.                                                                                                                                                        |
| geolocation                                          | numeric, numeric            | 37.532600, 127.024612 | Latitude and longitude of the location, city level, not exact datacenter coordinates.                                                                                                                             |
| provider-cfe-hourly                                  | numeric proportion 0.0-1.0 | 0.31                  | Carbon Free Energy proportion for this cloud provider and region, weighted by the hourly usage through the year.                                                                                                  |
| provider-cfe-annual                                  | numeric proportion 0.0-1.0 | 0.28                  | Carbon Free Energy proportion for this cloud provider and region, calculated on an annual totals basis.                                                                                                           |
| power-usage-effectiveness                            | numeric                    | 1.18                  | Power Usage Effectiveness (PUE) ratio for the region, averaged across individual datacenters.                                                                                                                     |
| water-usage-effectiveness                            | litres/kWh                 | 2.07                  | Water Usage Effectiveness in litres per kilowatt hour for the region, averaged across individual datacenters.                                                                                                     |
| provider-carbon-intensity-market-annual              | gCO2e/kWh                  | 0                     | Scope 2 market based carbon intensity including any energy and carbon offsets obtained by the provider, that rolls up to their Net Zero reporting.                                                          |
| provider-carbon-intensity-average-consumption-hourly | gCO2e/kWh                  | 354                   | Electricity Maps consumption based carbon intensity weighted by the provider’s hourly usage through the year as part of a 24x7 calculation.                                                                       |
| grid-carbon-intensity-average-consumption-annual     | gCO2e/kWh                  | 429                   | Electricity Maps consumption based carbon intensity annual average for the em-zone-id                                                                                                                             |
| grid-carbon-intensity-marginal-consumption-annual    | gCO2e/kWh                  | 686.0136038           | WattTime marginal carbon intensity annual average for the wt-region-id                                                                                                                                            |
| grid-carbon-intensity                                | gCO2e/kWh                  | 686                   | Specific named output for Impact Framework model that is consumed by SCI-o. It’s defined by SCI to be location based and is currently set to the same value as grid-carbon-intensity-marginal-consumption-annual. |

### References
- [Amazon Renewable Energy Methodology](https://sustainability.aboutamazon.com/renewable-energy-methodology.pdf)
- [Amazon Carbon Methodology](https://sustainability.aboutamazon.com/carbon-methodology.pdf)
- [Azure Datacenter Fact Sheets for 2022](https://web.archive.org/web/20240308233631/https://datacenters.microsoft.com/globe/fact-sheets/)
- [Google Carbon-Free Energy by Region](https://cloud.google.com/sustainability/region-carbon)
- [Google Sustainability Report for 2023](https://www.gstatic.com/gumdrop/sustainability/google-2023-environmental-report.pdf#page=90)