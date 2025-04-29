# 4. Operating and Maintaining MeteoGate Community Components

This section provides technical guidance for MeteoGate Community Capability Operators, typically NMHSs or designated EUMETNET members, responsible for operating and maintaining MeteoGate's Community Components. 

It covers deploying, configuring, and managing shared platform services like Data Explorer, API Gateway, Developer Portal, and authentication components. 

This guide will be updated as MeteoGate evolves, incorporating feedback, lessons learned, and improvements based on operational experience and new technical developments. 

## Community Components 

The Community Components covered in this guide include: 

- **Data Explorer**: User interface for data discovery and visualisation.
- **API Gateway (APISIX)**: Routes and secures external API traffic.
- **Developer Portal**: User interface for registration and API key management.
- **Key Vault (HashiCorp Vault)**: Stores and manages secrets such as API keys.
- **Identity & Access Manager (Keycloak)**: Handles identity federation and user group management.
- **Third-Party Configuration Management Tool (GitHub)**: Automates API route deployment and configuration via the API Management Tool.

## Responsibilities by role

The agreed responsibilities of MeteoGate operators include:
  
- **MeteoGate Community Capability Operators** provide IT services for MeteoGate environments (e.g., EWC, AWS) they host and operate the Community Components within those environments. They can use their own IT service management practices if they meet MeteoGate Quality of Service criteria and FEMDI Programme requirements. They collaborate with Data Providers, other Community Capability Operators, WMO WIS2 Capability Operators, FEMDI Solution Manager, and Programme Manager. Typically, they are NMHSs or designated EUMETNET members.

- **The MeteoGate Programme Manager** is part of the Coordinating Member, which has delegated authority from the FEMDI Owner to operate the FEMDI Programme. The Coordinating Member, an EUMETNET member selected through a bidding process, coordinates and governs the FEMDI Programme.

- **MeteoGate Solution Manager**: Oversees FEMDI Programme IT Service Management, providing Quality of Service criteria, guidance to Community Capability Operators, and monitoring compliance.

- **MeteoGate Service Desk**: Acts as the first point of contact for stakeholders, offering first-level support, coordinating with other parties to address user requests, incidents, and problems. It is organized by a Consortium Member.

## Onboarding and Offboarding 

The onboarding and offboarding of MeteoGate Community Capability Operators is described in [MeteoGate Participation Management Policy](https://tlnt19059.sharepoint.com/:w:/r/sites/FEMDI/ET%20FEMDI/ET%20Working%20folder/Operating%20Model/Participation%20Management%20Policy.docx?d=waa4e55e149fb4560af56c8680d2fa26e&csf=1&web=1&e=Z92DgA). 

## Managing MeteoGate IT Services 

IT Service Management ensures MeteoGate Community Components meet user needs and Quality of Service criteria. It includes managing user requests, operating and monitoring technical components, ensuring system availability and performance, handling technical problems, and reporting on service provision.

Designing and building new Community Components is not included. 

The FEMDI Programme recommends having mature IT service management capabilities but does not mandate specific frameworks or practices, with a few exceptions explained below. 

ITIL definitions will be used where required. These can be found here: [ITIL Glossary/ ITIL Terms I | IT Process Wiki](https://wiki.en.it-processmaps.com/index.php/ITIL_Glossary/_ITIL_Terms_I). 

## Minimum Processes for Community Capability Operators 

There are a few IT Service Management processes that Community Capability Operators need to implement and follow. 

**Support Request Management**
- Tracking, prioritizing, and resolving MeteoGate stakeholder inquiries and issues. 

**Incident & Problem Management**
- Timely resolution of incidents to restore normal service operation as quickly as possible, in co-operation with relevant parties. 
- Identifying and resolving underlying causes of recurring incidents and problems, in co-operation with relevant parties. 

**Change Management** 
- Controlled and efficient handling of changes to IT services, systems, and infrastructure, in co-operation with relevant parties. 

**Performance Management and Reporting** 
- Ensures that the performance of MeteoGate services is being monitored on an ongoing basis, with regular reports on functions like the availability and capacity. 

# Service Lifecycle 

This section outlines the lifecycle of hosting and operating MeteoGate Community Components, including setup, configuration, testing, updates, and ongoing operations.

The guidance covers infrastructure provisioning with Infrastructure as Code (IaC), component installation, initial setup (e.g., identity provider configuration), and practices for ensuring availability, security, performance, and disaster recovery.

It aligns with MeteoGate Quality-of-Service and cybersecurity requirements and complements related documentation in GitHub repositories and GeoWeb platform.

_Data Explorer info to be added_

## Setting Up Infrastructure 

In MeteoGate, the API Gateway and Support Services are deployed using Infrastructure as Code (IaC) with Terraform. The infrastructure is designed to be cloud-platform agnostic, with a primary reference deployment on the European Weather Cloud (EWC). 

Currently supported environments: 

- EWC (fully supported)
- AWS (Terraform support available; documentation to be updated) 

In the EWC environment: 

- Kubernetes clusters are created within the Community Capability Operator’s EWC tenant. 
- Identical mirrored environments can be deployed across ECMWF and EUMETSAT availability zones to ensure redundancy and high availability. A shared DNS-based load balancing setup using Route 53 distributes traffic between the two environments. This setup ensures failover and geographical redundancy, with Route 53 automatically directing requests to an available instance. 
- All core components (API Gateway, Developer Portal, Vault, Keycloak, etc.) are then automatically installed and configured via Terraform into these Kubernetes clusters. Most installation and configuration steps are automated. Manual tasks are limited to injecting required secrets and managing access credentials. 
- The deployed services are placed behind a load balancer and accessed through a Nginx reverse proxy. 
- Each Kubernetes node runs Ubuntu 22 LTS.

Terraform modules and technical instructions are available at: [GitHub – EURODEO/femdi-gateway-iac](https://github.com/EURODEO/femdi-gateway-iac) 

Resource recommendations (e.g. for Kubernetes clusters, VM sizes) are available in the repository _Link TBA_

