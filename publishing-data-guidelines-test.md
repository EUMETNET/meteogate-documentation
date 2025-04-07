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
- **FAIR Principles** for best practices in data consistency, accuracy, and compliance

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
