---
title: Publishing Data Guidelines
layout: default
---

# Publishing Data Guidelines

## Contents
- [Introduction](#introduction)
- [What is a Data Supply?](#what-is-a-data-supply)
- [Roles in the Data Publishing Process](#roles-in-the-data-publishing-process)
- [Data Publishers’ Responsibilities](#data-publishers-responsibilities)
- [Fulfilling EU and WMO Obligations](#fulfilling-eu-and-wmo-obligations)

---

## Introduction

These guidelines describe how to publish open meteorological and hydrological data through **MeteoGate**, ensuring compliance with **EU High-Value Datasets (HVD)** regulations and **WMO WIS 2.0** obligations.  

The document is intended for **National Meteorological and Hydrological Services (NMHSs)** and other entities publishing data via MeteoGate.

- For an overview of MeteoGate, refer to the _MeteoGate Overview_.
- For detailed instructions on API development, refer to the _MeteoGate API Developer Guide_.

> _Note: Currently MeteoGate introduction, glossary, roles, overall architecture, and technical component descriptions are in the MeteoGate Overview._

> _Documentation map to be added; link to MeteoGate general instructions._

---

## What is a Data Supply?

In MeteoGate, a **Data Supply** is the technical component used to share, manage, and publish data and metadata. It enables:
- Data storage
- API-based access
- Data processing in line with regulations
- Metadata management for discovery
- Notification services to the **WMO WIS2** components

A **Data Publisher** deploys a Data Supply to make datasets available through MeteoGate.

More details on architecture and integration options can be found in the _MeteoGate Overview_.  
> _(link to be updated)_

---

## Roles in the Data Publishing Process

The data publishing process involves several roles:

- **Data Owners**: Define usage rights and conditions.
- **Data Publishers**: Share data through a Data Supply and ensure compliance.
- **Local Capability Operators**: Manage Data Supplies, provide support, enforce policy adherence.
- **Data Consumers**: Access shared data.
- **Community Capability Operators**: Maintain MeteoGate data discovery and sharing infrastructure.
- **FEMDI Coordinating Member**: Oversees system-level governance.

> _A full description of roles and responsibilities is available in the MeteoGate Overview._  
> _Role names to be confirmed._  
> _(link to be updated)_

---

## Data Publishers’ Responsibilities

Data Publishers must ensure:
- Data integrity and availability
- Compliance with MeteoGate policies
- Authorization from Data Owners
- Maintenance of minimum data availability
- Notification of disruptions
- Incident response and support
- Reporting to **EUMETNET** as required

See full policy in the _MeteoGate Participation Management Policy_.  
> _(link to be updated)_

---

## Fulfilling EU and WMO Obligations

MeteoGate aligns with:
- **WMO WIS 2.0**
- **EU Open Data policies**

It integrates with:
- **WIS2 Global Broker**
- **Global Discovery Catalogue**

Compliance is ensured through:
- EU Commission Implementing Regulation **2023/138**
- EU Open Data Directive **(2019/1024)**
- EU INSPIRE Directive **(2007/2)**
- WMO Manual on WIS 2.0
- Guide to WIS 2.0
- WMO Manual on Codes (**WMO No. 306**)

Recommended standards:
- **EUMETDAPS** definitions for European meteorological data-sharing policies
- Standardized vocabularies from **WMO**, **ECMWF**, **EUMETNET**, and **EUMETSAT**
- **[FAIR Principles](https://www.go-fair.org/fair-principles/)** for best practices in data consistency, accuracy, and compliance

---

## Examples 

The following are real-world examples of successful data publication within MeteoGate.

<b>Metadata</b> 

[E-SOH discovery metadata](https://observations.eumetnet.eu/collections/observations/dataset)

[KNMI discovery metadata](https://tlnt19059.sharepoint.com/sites/FEMDI/_layouts/15/download.aspx?UniqueId=6a0407399d65402b8d18c2ef82ac1c06&e=UFaGwI) 

<b>Notifications</b>

[WMO notification examples](https://schemas.wmo.int/wnm/1.0.0/examples) 

<b>APIs</b> 

[DMI’s EDR API](https://opendatadocs.dmi.govcloud.dk/en/APIs/Forecast_Data_EDR_API) 

[DMI’s STAC API](https://opendatadocs.dmi.govcloud.dk/en/APIs/Forecast_Data_STAC-API) 

[FMI’s EDR API](https://opendata.fmi.fi/edr/collections) 

[KNMI’s EDR API](https://developer.dataplatform.knmi.nl/edr-api)  

[MeteoSwiss STAC API](https://github.com/MeteoSwiss/opendata) 

---

## Steps to Publish Data Through MeteoGate

### 1. Select data publishing pattern(s) and deployment environment
The choice between using a MeteoGate HVD service, the API Gateway, or direct access affects design choices and deployment. The deployment platform – whether EWC, public cloud, or on-premises – should align with data access needs and cost considerations. Establish rate limits and potentially access control to ensure efficient resource use and fair access. 

### 2. Structure and Prepare Data

Data must be structured into well-organised datasets and collections, ensuring compatibility with MeteoGate and WMO standards. High data quality is essential, and discovery metadata must be created to enable searchability in MeteoGate, WMO WIS2, and external search engines. 

### 3. Deploy and Integrate Data Supply  

The Data Supply must be deployed according to the chosen publishing pattern. This involves setting up and validating API endpoints, publishing metadata and notifications, and ensuring security, scalability, and compliance. Once ready, Data Publishers must integrate the Data Supply with MeteoGate and WIS2 components. 



## Select data publishing pattern(s) 

The Data Publisher and/or Data Owner will decide how they want their data to be made available. Data can be published through MeteoGate in three different patterns. The choice should be based on the data type, existing technical solutions and runtime environments already used for sharing open data (if any), and specific requirements for access control, cost, and performance. Data Publishers and/or Owners can choose different options for different datasets and APIs. 

The publishing pattern must be selected before designing the Data Supply and APIs as it influences design choices. All options will need MeteoGate-compliant metadata and notifications to be shared to WMO WIS2 (See registering a WIS2 Node). 

### Pattern 1: Using a MeteoGate HVD Service 

Data Publishers integrate their Data Supply components to a MeteoGate HVD service provided by EUMETNET, which proxies and/or standardizes the data before making it available through the MeteoGate API Gateway. 

EUMETNET currently has the following HVD services available:  

- [Land-based surface observations](https://observations.eumetnet.eu/) (E-SOH)  
- [Radar](https://github.com/EURODEO/openradardata-requirements/blob/main/Deliverable_Requirements.md)    
- Climate _<link to instructions, e.g. GitHub> _
- [Warnings](https://meteoalarm.org/en/live/page/redistribution-hub#list)

_Check specific integration requirements and instructions from the respective HVD service links._

This pattern is appropriate for the following situations​: 

- When providing data types already supported by an existing HVD service (e.g., climate, radar, land-based surface observations). 
- Want to leverage standardized, centrally managed services for making data available to MeteoGate. 
- Want to ensure alignment with EU High-Value Dataset (HVD) regulations by using a pre-established, compliant service. 
- Prefer minimal IT investment while ensuring the data is discoverable, accessible, and compliant with MeteoGate requirements. 
- Want to benefit from improved security, efficiency, and cost-effectiveness, as MeteoGate handles security, rate limiting, and monitoring. 

### Pattern 2: Managed and Proxied Access Through MeteoGate API Gateway

The Data Supply component is registered with an appropriate MeteoGate API Gateway​. Currently, these are deployed on the EWC, in ECMWF and EUMETSAT runtime environments. EUMETNET Members will connect their data services via EWC irrespective of the location of their data (e.g., EWC, public cloud, on-premises).​ ​In the future, if requirements change, MeteoGate API Gateway can be implemented also on public clouds. 

The data is requested from and goes through the MeteoGate API Gateway allowing: 

- Managed data requests and data flow e.g., authorization, access control, and rate limiting
- Collection of insights on data requests and access​.

Read more about the MeteoGate API Gateway features on MeteoGate Overview and GitHub _<links TBA>._

This pattern is appropriate for the following situations​: 

- There is no suitable HVD service for the type of data to be shared.
- Data is hosted on EWC, or data access APIs will be hosted on EWC.
- Prefer minimal IT investment while ensuring the data is discoverable, accessible, and compliant with MeteoGate requirements.
- Want to benefit from improved security, efficiency, manageability, and cost-effectiveness, as MeteoGate handles security, rate limiting, and monitoring. 

### Pattern 3: Direct Access 

Data will be discoverable using MeteoGate, but it is requested and accessed directly from the Data Supply​ – not via the MeteoGate API Gateway​. The Data Supply and APIs are responsible for access management, rate limiting, and monitoring​. This requires more capabilities from the Data Supply, potentially making it more expensive to implement.

This pattern is appropriate for the following situations​: 

- Data is already published through a public cloud providers’ open data initiative (e.g., Amazon Sustainability Data Initiative (ASDI), Microsoft Planetary Computer, Google Earth Engine, and Oracle Open Data). Such initiatives may cover the cost of data storage, cross-region duplication, and egress charges, thereby enabling large-scale distribution of data at low cost to the data provider – and, importantly, benefitting from the Quality-of-Service (QoS) provisions implemented by the cloud platform provider. 
- Data is already published through a Government Data Portal​. 
- Data or an existing API used to publish it is hosted on a public cloud (e.g. AWS, Azure, Google Cloud, or Oracle Cloud). Serving data through the MeteoGate API Gateway would be expensive due data egress costs. 
- Large volume of data (e.g. radar or climate forecast data). 
- High performance or availability requirements (e.g., large amount of data transferred or frequent use)​. 
- Want to have full control of how the data is made available. 
- Want to use fine-grained access control (e.g. for example, various groups of registered users with different QoS, paid or premium access, etc.). 
- Want to set rate limits and quotas or allow monitoring above the capabilities offered by the MeteoGate API Gateway. 
- Access control, request management and collection of usage metrics are already handled by a Third Party or the Data Supply/API (for example, the Data Publisher already has API Management implemented as part of the Data    Supply capability).

 
In the future, EUMETNET Members may also make use of a packaged instance of the MeteoGate API that can be deployed locally either on-premises or in a designated cloud tenancy. This will enable the Members to benefit from all necessary API Management capabilities such as rate limiting, access control, and sharing of API keys while limiting its use for only to their own datasets. 

### Choose a Suitable Platform and Understand Related Costs 

When selecting the appropriate runtime environment for publishing data through MeteoGate, Data Publishers should consider: 

- Where their data and data access APIs are hosted (e.g., on-premises, EWC, or public cloud such as AWS, Azure, Google Cloud, or Oracle Cloud). 
- Whether they need or want to use MeteoGate HVD services or the MeteoGate API Gateway (Data Publishing Patterns 1 or 2).

The MeteoGate API Gateway is currently deployed on EWC. There is no MeteoGate API Gateway available on any public cloud. However, currently 80 % of EUMETNET Members either do not need API Gateway protection or can be covered by the MeteoGate API Gateway on EWC. 

**Recommendations**

- All Members providing High-Value Datasets (HVD) should connect to an appropriate HVD service or the MeteoGate API Gateway instance on EWC. 
- If providing data or APIs that are hosted on-premises or in a public cloud, assess whether it makes sense to use HVD services, the MeteoGate API Gateway, or provide direct access. Members hosting data services on a public cloud should consider potential egress costs, as these depend on individual cloud service providers’ pricing models. Carefully evaluate whether the benefits of the MeteoGate API Gateway – such as access control, rate limiting, and monitoring – justify the additional data transfer costs. 


### Decide on Authentication and Rate Limits 

Together with the Data Owner the Data Publisher will define appropriate data access restrictions based on, for example, appropriate data policy, requirements on being able to identify users, performance and availability requirements, and the technical platform of the Data Supply and its limitations. 

Typically, the access restrictions are as follows: 
- Rate limits and quotas: Control the number of requests a user or application can make to an API within a specified time frame to prevent abuse and ensure fair usage. 
- Authentication: Determine if the API will be open to public users or restricted to authenticated users only.

The access restrictions may be different for different datasets and APIs. Typically, it is most feasible to offer datasets sharing the same access restrictions under the same API endpoint. 

The access restrictions are implemented in different ways depending on the Data Publishing Pattern used: 
- Pattern 1: When sharing data through a HVD service, the technical access restrictions also depend on the HVD service’s agreed specifications.  
- Pattern 2: The MeteoGate API Gateway allows setting 1) authentication (whether users need to authenticate using API Key or not to access the endpoint) and 2) rate limits (maximum number of requests per time interval) for each API endpoint. Rate limits can be set separately for authenticated and unauthenticated users, when authentication is not required (i.e. when offering better QoS to registered users). The Data Publisher can set the access restrictions for the Data Supply during the onboarding process.  
- Pattern 3: When sharing data directly, the Data Publisher is responsible for implementing all required access restrictions. 
