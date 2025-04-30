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

## Quality-of-Service 

MeteoGate Community Components must be operated in accordance with the Quality-of-Service (QoS) requirements defined by the FEMDI Programme. These requirements aim to ensure consistent service availability, performance, and resilience across the federated infrastructure. 

To meet these objectives, Community Capability Operators must: 

- Leverage cloud-native scalability features, including autoscaling, node-level resource limits, and container orchestration (Kubernetes). 
- Ensure that sufficient compute, memory, and storage resources are provisioned to support expected traffic volumes, performance requirements, and failover scenarios. 
- Deploy core components across redundant availability zones or regions, where supported (e.g. in EWC across ECMWF and EUMETSAT sites). 
- Use a cloud provider–specific or open-source load balancer (e.g. NGINX, Route 53) to distribute traffic across environments and ensure failover capability. 

The standard EWC deployment supports geographical redundancy using DNS-based load balancing (Route 53). As EWC infrastructure evolves, it may support cross-zone application-level availability and external failover, which should be adopted when feasible. 

QoS metrics and expectations—such as uptime, latency, and response time—will be specified in the final QoS documentation. Operators are expected to monitor performance and report monthly as part of operational governance. 

See also: Section about [Monitoring](#monitoring-and-observability) and Reporting Performance. 

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

## Installations

The following MeteoGate Community Components are automatically installed and pre-configured using Terraform as part of the infrastructure deployment process. This ensures consistent and reproducible setup across environments. 

Some manual actions (such as registering new API Gateway instances and injecting sensitive secrets) are still required. These are described in the following section. 

**API Gateway (APISIX)**
- Deployment and basic configuration handled by Terraform. 
- To activate a new API Gateway instance, it must be manually registered in the API Management Tool (GitHub-based). Integration instructions for new instances are available in the API Management Tool repository: EUMETNET/api-management-tool-poc. (See section 4 for more on the API Management Tool). 
- Routes are managed using the API Management Tool in GitHub (see section 4). <add link> 
- Secrets for API Gateway admin access must be added to Vault during setup. 

**Developer Portal** 
- Deployed as custom UI and backend components via a Helm chart. 
- Terraform handles installation and initial configuration. 
- See: EURODEO/Dev-portal – Helm chart 

**Key Vault (HashiCorp Vault)** 
- Installed in HA mode (cluster) by Terraform. 
- Secrets used in route configuration should be added via GitHub-managed configurations. 

**Identity & Access Manager (Keycloak)** 
- Installed and initially configured by Terraform. 
- Some IdPs (e.g. GitHub, Google) are pre-registered; secrets such as client IDs and passwords must be provided as parameters. 
- Additional setup can be done via the Keycloak UI. 
- Documentation pending. 

**Third-Party Configuration Management Tool** 
- GitHub-based tool for managing API Gateway route configuration. 
- Operators must be granted access to manage route YAML files and trigger CI/CD workflows.

## Initial Configuration 

Most installation and configuration steps are automated. Manual tasks are limited to injecting required secrets and managing access credentials. After provisioning, the following initial tasks may be required: 

- Identity Provider Setup: Some IdPs (such as GitHub or Google) are pre-configured via Terraform. Credentials (e.g. client secrets) must be provided securely before deployment. Additional IdPs must be added manually. See section 5.2 for managing IdPs in Keycloak. <add link> 
- Key Vault Bootstrapping: Vault is initialised by Terraform. Secrets used for configuring API Gateway admin access and CI/CD credentials must be added via GitHub. 

## Updates 

Updates to MeteoGate Community Components use the same Terraform-based IaC approach as the initial deployment, enabling operators to manage updates consistently with version-controlled configurations and reproducible pipelines.

Applying updates is the responsibility of the Community Capability Operator hosting the environment. 

Update needs include: 

- Security patches and version upgrades for the API Gateway, Vault, and Keycloak. 
- Regular updates to underlying infrastructure (e.g., Kubernetes, operating systems). 
- Synchronisation of configuration or deployment changes introduced in the GitHub repositories. 

In EWC environments, Community Capability Operators are expected to coordinate with EUMETSAT and ECMWF as infrastructure providers regarding updates that impact shared platform services. 

More detailed update instructions, including versioning recommendations and rollback procedures, will be provided in the GitHub repository. (_not yet documented_)

## Testing and Quality Management 

All new deployments of MeteoGate Community Components must be tested to verify that services are correctly installed, configured, and reachable. Existing deployments should also be re-tested after updates or configuration changes to ensure continuity and stability. 

Testing should cover the following areas: 
- Basic availability checks for each component (e.g., API Gateway routes, Developer Portal access). 
- Authentication and authorisation flows via configured Identity Providers. 
- Secret management and API key issuance. 
- End-to-end functional testing, where feasible. A dedicated test script for verifying component readiness is planned and tracked in the FEMDI backlog. 

Integration or end-to-end test automation is recommended when possible, although unit testing for Terraform-managed infrastructure remains limited in practice. 

You can use the [Test plan from the FEMDI Programme](https://tlnt19059.sharepoint.com/:w:/r/sites/FEMDI/ET%20FEMDI/ET%20Working%20folder/RODEO_FEMDI_test_plan.docx?d=w510c36cc158f4dc18044cd4c072dea39&csf=1&web=1&e=AimA5j) as a reference when planning or executing tests. 

## Logging and Audit Trail 

Logging is an essential part of operating MeteoGate Community Components. It supports system observability, incident investigation, auditing, and performance monitoring. All core components—including the API Gateway, Developer Portal, Vault, and Identity & Access Manager—should have logging enabled. 

Community Capability Operators are responsible for configuring, maintaining, and securing logging mechanisms in their environments. 

**Logging Requirements** 

The logging setup should capture at minimum: 
- Authentication and authorisation events (e.g., login attempts, group assignments, API key usage) 
- API request metadata and error responses 
- Component health and operational status 
- Administrative actions and configuration changes 
- System warnings and failures 

**Responsibilities of Operators** 

Operators must ensure that: 
- Logs are retained securely and in accordance with applicable compliance or internal policy requirements. 
- Access to logs is restricted to authorised personnel. 
- Logs are collected and made available for use in incident response, performance analysis, and system audits. 
- Centralised logging tools (e.g., ELK stack, Loki, or cloud-native logging solutions) are used where possible to streamline access and analysis.

Logging also supports processes defined in Chapter 5 (e.g., Incident Management and Problem Management) by providing the necessary data for troubleshooting and post-incident review. _<links>_ 

Further logging configuration examples and integrations (e.g., with Prometheus or Alertmanager) will be available in the GitHub repository: [EUMETNET/femdi-gateway-iac](https://github.com/EUMETNET/femdi-gateway-iac) 

_TBA_

## Monitoring and Observability 

Monitoring and observability are essential for maintaining reliable operations, detecting issues early, and supporting performance analysis across MeteoGate Community Components. 

Each deployment should include: 
- Basic service health and resource monitoring. 
- Performance metrics collection. 
- Alerting for critical failures or degraded service.

**Tools and setup** 

- **Prometheus** is used as the primary monitoring solution to collect metrics from components such as the API Gateway and Kubernetes infrastructure. Prometheus-based setup is not yet fully documented. Configuration examples will be provided in the GitHub repository. 

- **Alertmanager** is used to trigger notifications based on defined rules (e.g. service downtime, resource limits). The default Alertmanager configuration, including email-based alerting setup and instructions for extending it (e.g. Slack notifications, grouping), is described in the GitHub repository: EUMETNET/femdi-gateway-iac 

A central **Insights Service** is planned as part of MeteoGate to provide shared usage metrics and operational dashboards. Further information will be added as the service becomes available. 

## Continuity and Disaster Recovery

To ensure service continuity and enable recovery from system failures or data loss, each MeteoGate Community Component deployment must implement a disaster recovery plan. This includes regular backups of application data and configuration, as well as defined restore procedures. 

Community Capability Operators are responsible for implementing and maintaining the backup and disaster recovery solutions in their respective environments. This includes verifying that backups are functional, accessible, and restorable when needed. 

ll core components, including: 
- API Gateway (APISIX): etcd database 
- Identity & Access Manager (Keycloak): PostgreSQL database 
- Key Vault (Vault): Raft storage 
are configured with scheduled Cron jobs to perform backups. These jobs are defined using Terraform and store encrypted backup files in an AWS S3 bucket by default. The setup supports adaptation to other cloud environments. 

Backups are retained according to the policies defined by the Community Capability Operator. The backup frequency can be customised; a recommended baseline is: _<to be confirmed>_ 

- Daily backups of all stateful components. 
- Retention period of at least 7–14 days for critical data. 
- Full restore test at least every 6 months. 

Restore operations are executed manually using predefined command templates. These templates are included in the infrastructure codebase and support consistent restoration across environments. 

**Example: Cron Job for Vault Backup**

cron_schedule: “0 2 * * *”  # Every day at 2 AM

backup_command: “/vault/scripts/backup.sh” 

storage_target: “s3://meteogate-backups/vault/” 

_<to be verified>_


See the [GitHub repository](EURODEO/femdi-gateway-iac) for backup and restore configuration and instructions. 

## Security 

"Security is crucial for MeteoGate Community Components. Each operator must ensure their deployment environment meets MeteoGate Quality-of-Service requirements, Cyber Security Policy, and regulatory frameworks like the EU’s NIS2 Directive.

MeteoGate uses built-in security features such as access control, encryption, and secret management, but secure operations also require hardening, secure configurations, and regular monitoring at both infrastructure and application levels."

Operators must ensure protection against: 

- Unauthorised access to administrative interfaces and user data. 
- Denial-of-service (DoS) attacks and traffic spikes. 
- Misconfigurations that could expose APIs or internal components. 
- Credential or token leakage (e.g. admin passwords, service tokens). 

### Authentication and Authorisation 

- Authentication is handled via third-party Identity Providers (IdPs), federated through Keycloak. 
- User registration and login are managed by the Developer Portal, which interacts with Keycloak and creates consumer records in the API Gateway (APISIX). 
- Access to APIs can be restricted via API Keys, which are issued by the Developer Portal and stored securely in the Key Vault (Vault). 
- Admins can manage user roles and disable access through backend scripts (see Section 4.3).

### Encryption 

- All communication between MeteoGate components and users is encrypted using HTTPS/TLS. 
- Internal traffic between Vault instances in HA mode is also encrypted. 
- Encrypted storage and secure access are used for backups, secrets, and API credentials.

Important: Ensure all public endpoints use valid TLS certificates and that internal service communication is encrypted and authenticated. 

### Security Management 

Community Capability Operators must ensure that: 

- Access to sensitive infrastructure (e.g. Kubernetes clusters, GitHub secrets, Vault root tokens) is limited and auditable. 
- Logs are retained and regularly reviewed (see Section 5.5). 
- System components are patched in a timely manner (see Section 3.4 Updates). 
- Security incidents are managed according to the incident process (see Section 5.1). <links>

For more information, refer to the **MeteoGate Cybersecurity Policy** _[link to be added]_.

# Service Operations 

This section provides operational guidance for Community Capability Operators to maintain MeteoGate Community Components. Tasks ensure shared services like the API Gateway, Developer Portal, Key Vault, and Identity & Access Management remain available, secure, and efficient.

Service operations include configuring API routes, managing user authentication and authorization, handling API keys, and synchronizing environments. The goal is to maintain a reliable, scalable foundation for exposing APIs and supporting secure access to meteorological and hydrological data.

The section covers API Gateway tasks (e.g., onboarding new APIs, managing routes and rate limits) and support services (e.g., handling user groups, IdP configurations, and secrets management). Many operations are implemented through configuration as code and automated workflows in GitHub repositories




## Performance reporting

Operators must submit regular reports (e.g., monthly) to the MeteoGate Programme Manager. These reports enable performance tracking against QoS targets and support transparency across MeteoGate. 

Reports may include: 
- Availability and uptime metrics 
- API Gateway usage statistics 
- Incident and support request summaries 
- Capacity and resource usage 

Guidelines and templates will be provided by the Programme team. Reports are compiled using monitoring tools and logs (see seaction: [Monitoring](#monitoring-and-observability))


