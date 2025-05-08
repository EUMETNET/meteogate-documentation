# 4. Operating and Maintaining MeteoGate Community Components

This section provides technical guidance for MeteoGate Community Capability Operators, typically NMHSs or designated EUMETNET members, responsible for operating and maintaining MeteoGate's Community Components. 

It covers deploying, configuring, and managing shared platform services like Data Explorer, API Gateway, Developer Portal, and authentication components. 

These instructions will be updated as MeteoGate evolves, incorporating feedback, lessons learned, and improvements based on operational experience and new technical developments.

---

## Community Components 

The Community Components covered in this guide include: 

- **Data Explorer**: User interface for data discovery and visualisation.
- **API Gateway (APISIX)**: Routes and secures external API traffic.
- **Developer Portal**: User interface for registration and API key management.
- **Key Vault (HashiCorp Vault)**: Stores and manages secrets such as API keys.
- **Identity & Access Manager (Keycloak)**: Handles identity federation and user group management.
- **Third-Party Configuration Management Tool (GitHub)**: Automates API route deployment and configuration via the API Management Tool.

## Responsibilities by Role

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

---

## Service Lifecycle 

This section outlines the lifecycle of hosting and operating MeteoGate Community Components, including setup, configuration, testing, updates, and ongoing operations.

The guidance covers infrastructure provisioning with Infrastructure as Code (IaC), component installation, initial setup (e.g., identity provider configuration), and practices for ensuring availability, security, performance, and disaster recovery.

It aligns with MeteoGate Quality-of-Service and cybersecurity requirements and complements related documentation in GitHub repositories and GeoWeb platform.

_Data Explorer info to be added_

### Setting Up Infrastructure 

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

### Installations

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

### Initial Configuration 

Most installation and configuration steps are automated. Manual tasks are limited to injecting required secrets and managing access credentials. After provisioning, the following initial tasks may be required: 

- Identity Provider Setup: Some IdPs (such as GitHub or Google) are pre-configured via Terraform. Credentials (e.g. client secrets) must be provided securely before deployment. Additional IdPs must be added manually. See section 5.2 for managing IdPs in Keycloak. <add link> 
- Key Vault Bootstrapping: Vault is initialised by Terraform. Secrets used for configuring API Gateway admin access and CI/CD credentials must be added via GitHub. 

### Updates 

Updates to MeteoGate Community Components use the same Terraform-based IaC approach as the initial deployment, enabling operators to manage updates consistently with version-controlled configurations and reproducible pipelines.

Applying updates is the responsibility of the Community Capability Operator hosting the environment. 

Update needs include: 

- Security patches and version upgrades for the API Gateway, Vault, and Keycloak. 
- Regular updates to underlying infrastructure (e.g., Kubernetes, operating systems). 
- Synchronisation of configuration or deployment changes introduced in the GitHub repositories. 

In EWC environments, Community Capability Operators are expected to coordinate with EUMETSAT and ECMWF as infrastructure providers regarding updates that impact shared platform services. 

More detailed update instructions, including versioning recommendations and rollback procedures, will be provided in the GitHub repository. (_not yet documented_)

### Testing and Quality Management 

All new deployments of MeteoGate Community Components must be tested to verify that services are correctly installed, configured, and reachable. Existing deployments should also be re-tested after updates or configuration changes to ensure continuity and stability. 

Testing should cover the following areas: 
- Basic availability checks for each component (e.g., API Gateway routes, Developer Portal access). 
- Authentication and authorisation flows via configured Identity Providers. 
- Secret management and API key issuance. 
- End-to-end functional testing, where feasible. A dedicated test script for verifying component readiness is planned and tracked in the FEMDI backlog. 

Integration or end-to-end test automation is recommended when possible, although unit testing for Terraform-managed infrastructure remains limited in practice. 

You can use the [Test plan from the FEMDI Programme](https://tlnt19059.sharepoint.com/:w:/r/sites/FEMDI/ET%20FEMDI/ET%20Working%20folder/RODEO_FEMDI_test_plan.docx?d=w510c36cc158f4dc18044cd4c072dea39&csf=1&web=1&e=AimA5j) as a reference when planning or executing tests. 

### Logging and Audit Trail 

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

### Monitoring and Observability 

Monitoring and observability are essential for maintaining reliable operations, detecting issues early, and supporting performance analysis across MeteoGate Community Components. 

Each deployment should include: 
- Basic service health and resource monitoring. 
- Performance metrics collection. 
- Alerting for critical failures or degraded service.

**Tools and setup** 

- **Prometheus** is used as the primary monitoring solution to collect metrics from components such as the API Gateway and Kubernetes infrastructure. Prometheus-based setup is not yet fully documented. Configuration examples will be provided in the GitHub repository. 

- **Alertmanager** is used to trigger notifications based on defined rules (e.g. service downtime, resource limits). The default Alertmanager configuration, including email-based alerting setup and instructions for extending it (e.g. Slack notifications, grouping), is described in the GitHub repository: EUMETNET/femdi-gateway-iac 

A central **Insights Service** is planned as part of MeteoGate to provide shared usage metrics and operational dashboards. Further information will be added as the service becomes available. 

### Continuity and Disaster Recovery

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

### Security 

"Security is crucial for MeteoGate Community Components. Each operator must ensure their deployment environment meets MeteoGate Quality-of-Service requirements, Cyber Security Policy, and regulatory frameworks like the EU’s NIS2 Directive.

MeteoGate uses built-in security features such as access control, encryption, and secret management, but secure operations also require hardening, secure configurations, and regular monitoring at both infrastructure and application levels."

Operators must ensure protection against: 

- Unauthorised access to administrative interfaces and user data. 
- Denial-of-service (DoS) attacks and traffic spikes. 
- Misconfigurations that could expose APIs or internal components. 
- Credential or token leakage (e.g. admin passwords, service tokens). 

#### Authentication and Authorisation 

- Authentication is handled via third-party Identity Providers (IdPs), federated through Keycloak. 
- User registration and login are managed by the Developer Portal, which interacts with Keycloak and creates consumer records in the API Gateway (APISIX). 
- Access to APIs can be restricted via API Keys, which are issued by the Developer Portal and stored securely in the Key Vault (Vault). 
- Admins can manage user roles and disable access through backend scripts (see Section 4.3).

#### Encryption 

- All communication between MeteoGate components and users is encrypted using HTTPS/TLS. 
- Internal traffic between Vault instances in HA mode is also encrypted. 
- Encrypted storage and secure access are used for backups, secrets, and API credentials.

Important: Ensure all public endpoints use valid TLS certificates and that internal service communication is encrypted and authenticated. 

#### Security Management 

Community Capability Operators must ensure that: 

- Access to sensitive infrastructure (e.g. Kubernetes clusters, GitHub secrets, Vault root tokens) is limited and auditable. 
- Logs are retained and regularly reviewed (see Section 5.5). 
- System components are patched in a timely manner (see Section 3.4 Updates). 
- Security incidents are managed according to the incident process (see Section 5.1). <links>

For more information, refer to the **MeteoGate Cybersecurity Policy** _[link to be added]_.

---

## Service Operations 

This section provides operational guidance for Community Capability Operators to maintain MeteoGate Community Components. Tasks ensure shared services like the API Gateway, Developer Portal, Key Vault, and Identity & Access Management remain available, secure, and efficient.

Service operations include configuring API routes, managing user authentication and authorization, handling API keys, and synchronizing environments. The goal is to maintain a reliable, scalable foundation for exposing APIs and supporting secure access to meteorological and hydrological data.

The section covers API Gateway tasks (e.g., onboarding new APIs, managing routes and rate limits) and support services (e.g., handling user groups, IdP configurations, and secrets management). Many operations are implemented through configuration as code and automated workflows in GitHub repositories

### Managing API Routes 

API Gateway configuration—including available Data Supply API endpoints (i.e. upstream APIs), rate limiting, and authentication settings—is managed through the API Management Tool at [EUMETNET/api-management-tool-poc](https://github.com/EUMETNET/api-management-tool-poc) 

### Configuring API Routes

Configuration is defined in YAML files and maintained in version control. API Gateway route configuration (API endpoints) is stored in GitHub in YAML. Admins can update endpoints, rate limiting, and authentication settings in the file. Upon commit and push, an updated route configuration is generated, which is simpler than the APISIX configuration file.

A GitHub action workflow (Python code) generates a detailed APISIX configuration (YAML) and publishes it to selected API Gateway instances via APISIX REST API, updating the APISIX instance on the target platform.

Instructions for using the API Management Tool, including defining new routes, applying rate limits, and controlling access, are available in the GitHub repository’s README and examples.

_Note_: Some upstream APIs may require a dedicated API key for the API Gateway to access the backend service. Instructions for including upstream credentials securely are available in the API Management Tool repository. 

Onboarding an API follows the below process:

1. Create a Pull Request 
    - Navigate to the [designated repository](https://github.com/EURODEO/api-management-tool-poc/)
    - Create a new branch for your changes
    - Add the necessary code to onboard the API. 
 
2. Gateway Configuration - YAML File 
    - Add the following configuration to the YAML file: 
```
id: xxx                         #replace with your own descriptive and unique id 
version: 1.0.0 
platform: EWC    
routes: 
  - Route: 
      id:                       #add descriptive and unique id for API endpoint here 
      endpoint: https://api...  # add API endpoint URL here, needs to be a static URL 
      rateLimitAuth:            #add your rate limit values for authenticated user 
        count:                  #add rate limit here  
        time_window:            #add time window here 
      rateLimitAnon:            #add your rate limit values for anonymous user 
        count:                  #add rate limit here 
        time_window:            #add time window here 
      cors: true   

 ```

3. Commit & Merge 
    - Commit your changes to the branch. 
    - Create a pull request and request a review. 
    - Once approved, merge the pull request into the main branch. 
 
4. Check on Development Portal 
    - Verify that the new route is listed in the Developer Portal.
  
### Naming API Routes 

API routes configured through the MeteoGate API Gateway must follow a consistent and descriptive naming scheme to ensure clarity, discoverability, and alignment across the Developer Portal and Data Explorer. 
Routes are defined in the API Management Tool (GitHub) and are: 
- Exposed through structured URLs, such as: https://meteogate.api.eu/{service}/{dataset} 
- Visible to end users in the Developer Portal and Data Explorer. 
- Identified using a parameter:id in the YAML configuration. 

A formal naming scheme and hierarchy for API routes will be defined and published by the Programme team. This will ensure alignment across services such as E-SOH, radar data, and climate data APIs. 

Until then, route paths should be: 
- Descriptive and human-readable. 
- Consistent across environments. 
- Designed to reflect the data service, collection, or domain. 

Further guidance and examples will be added to the API Management Tool documentation as the scheme is finalized. 

_Route names / naming schemes / hierarchy <- guidance needed on the naming scheme: The URL that someone will use to access e.g. E-SOH data via MeteoGate GW. i.e. meteogate.api.eu/XXX  
Stuart and Vegar to make a proposal_

### Managing Admin Users 

Administration credentials for MeteoGate Community Components must be handled securely and consistently across all environments. Each component has its own method for managing admin-level access, and some share credentials with automation tools used for deployment and configuration. 

Admin credentials must be securely stored and rotated according to the hosting organisation’s security policy. Only authorised personnel should have access. Integration with Vault and GitHub secret management is strongly recommended. 

**API Gateway (APISIX)** 
- Admin access is handled via a single administrator account per instance. 
- Credentials are defined during initial configuration and stored as GitHub repository secrets. 
- The same credentials are used for both manual administration and automation (e.g., GitHub workflows). 
- APISIX supports only one admin account per instance. 

**Identity & Access Manager (Keycloak)** 
- Admin credentials are defined during the Terraform-based initial deployment. 
- Credentials may be updated manually or via Vault integration. 
- No per-user interface for managing admin roles is currently in use; role mapping is managed internally within Keycloak. <is that so> 

**Developer Portal** 
- The Developer Portal does not have a dedicated admin account. 
- Admin-level operations (e.g. disabling users, resetting keys) are performed using backend scripts and service tokens. 

**Key Vault (HashiCorp Vault)** 
- Vault admin access is configured during initial setup. 
- Currently, access is based on a root token, which should be used with caution. 
- The setup supports integration with other IdPs in future to enable more granular admin access and auditing. 

### Managing Users 

User management in MeteoGate is largely automated through the Developer Portal and requires minimal manual intervention from admins under normal circumstances. 

#### User Registration and Provisioning 

- When a user authenticates for the first time (e.g. via GitHub or another IdP), a user account is automatically created in the Identity & Access Manager (Keycloak). 
- Upon requesting an API Key, the Developer Portal: 
  - Creates a user account in the Key Vault 
  - Registers the user as a consumer in each API Gateway instance 
- Each user can have one API Key at a time. The Developer Portal checks for existing API Key and consumer status before issuing a new key. 
- Users are initially assigned to the USER group. 

#### User Roles 

There are three types of users in the system: 
- **USER**: Default group with standard access and rate limits. 
- **EUMETNET_USER**: Granted higher rate limits via API Gateway policies. 
- **ADMIN**: Has rights to manage users and system settings via backend tools. 

Rate limit configuration: 
- For **USERs**, rate limits are defined per API route in the API Gateway configuration. 
- For **EUMETNET_USERs**, a global enhanced rate limit is applied using the API Management
- Tool configuration file (YAML). This setting overrides route-specific limits to allow higher throughput.

#### Manual User Management 

Although automated, user accounts can be managed manually by administrators using backend scripts provided in the [Developer Portal backend](https://github.com/EUMETNET/Dev-portal/tree/main/backend)

Available admin operations include: 
- Enable / disable a user (disabling works by revoking the user’s API Key). 
- Remove a user account. 
_ Promote or demote a user to/from the **EUMETNET_USER** group. 

These operations are handled in the correct technical order to avoid inconsistencies and are intended for exceptional cases or support situations. 

Admins performing these operations must use a valid admin token to authenticate with the backend. 

### Managing API Keys

API Key management in MeteoGate is primarily automated through the **Developer Portal**. End users can request and manage their API Key without administrative intervention. 

#### Automated Management via Developer Portal 
- When a registered user requests an API Key, the Developer Portal: 
    1. Creates a new API Key in the Key Vault
    2. Associates the key with the user’s account
    3. Registers the key in all relevant API Gateway instances as part of the consumer configuration 

- Each user may hold only one API Key at a time. The system checks for existing keys before issuing a new one. 

#### Manual Key Administration 

In exceptional cases (e.g. security incidents or support actions), admins can: 
  - View, revoke, or delete API Keys manually. 
  - Use Vault’s web UI or backend scripts available in GitHub to manage keys. 
Scripts for key management and automation are available in: [EURODEO/Dev-portal GitHub repository](https://github.com/EURODEO/Dev-portal)

### #API Key Validity and Expiry 

Currently, API Keys do not expire automatically. Support for configurable key validity periods is planned for future updates and will be defined in the Developer Portal and Key Vault integration settings. 

API Key lifecycle policies (e.g. renewal, expiration, and deactivation) should follow the Programme’s security requirements and be documented accordingly. 

### Managing IdPs 

Admins can manage the set of trusted IdPs in the Identity & Access Manager (Keycloak) UI. Keycloak supports an extensive list of IdPs out-of-the-box. It also supports any OpenId Connect or SAML 2.0 IdP.  

In Keycloak, the configuration is done in “Identity Providers” in the left pane of the UI. Setting up an IdP also requires setup on the IdP side. 

To add or configure a new Identity Provider, refer to the official Keycloak documentation: [Keycloak Server Admin Guide – Identity Brokering](https://www.keycloak.org/docs/latest/server_admin/index.html#_identity_broker) 

_<to be updated; what is the policy for managing IdPs, e.g. approval?>_

### Syncing User and API Endpoint Data Between Environments 

In deployments where MeteoGate API Gateway instances are hosted in multiple cloud environments (e.g. EWC at ECMWF and EUMETSAT), configuration consistency across instances is essential. 

#### User Synchronisation 

- When a user registers via the Developer Portal, they are automatically registered as a consumer in each deployed API Gateway instance. 
- This ensures that the same API Key can be used across all environments. 
- The Developer Portal backend handles this synchronisation logic. 

See technical implementation in: [EURODEO/Dev-portal GitHub repository](https://github.com/EURODEO/Dev-portal) 

#### Route Synchronisation 

- All Data Supply routes (API endpoints) are defined and maintained using the API Management Tool in GitHub: EUMETNET/api-management-tool-poc 
- By default, routes are added to all Gateway instances, but the system is flexible: routes can also be configured per platform using the platform: field in the YAML configuration. 
- Once committed, a GitHub Actions workflow generates APISIX-compatible configuration files and applies them across environments via the Admin API. 

The entire sync and deployment process is containerised, runs in Kubernetes, and is designed for reliable operation across federated platforms. This architecture ensures seamless multi-cloud availability and simplifies the onboarding of new APIs and users across the MeteoGate federation.

### Key Vault Service Token Renewals 

API Gateway and Developer Portal use service tokens to communicate with Key Vault. These tokens have a maximum TTL of 768 hours (32 days). To prevent token revocation, a Cron job is scheduled to run on the 1st and 15th of each month to reset the token period. 

---

## Processes and Reporting 

This chapter defines the shared operational processes that all MeteoGate Community Capability Operators must follow to ensure a consistent level of service, regulatory compliance, and operational accountability.

These processes form the foundation of coordinated service delivery and support the federated nature of the system. They are aligned with the FEMDI Programme’s Quality-of-Service (QoS) objectives and provide common expectations for how issues are resolved, changes managed, and performance monitored. 

Community Capability Operators may use their own internal IT service management frameworks (e.g. ITIL). However, all processes must meet the minimum requirements defined by MeteoGate to ensure interoperability and oversight across environments. 

Possibly amend with relevant common process diagrams from [42 Common Processes and Ways of Working - MeteoGate.docx](https://tlnt19059.sharepoint.com/:w:/r/sites/FEMDI/ET%20FEMDI/ET%20Working%20folder/Documentation%20working%20area/42%20Common%20Processes%20and%20Ways%20of%20Working%20-%20MeteoGate.docx?d=w34e1ef2e9e3c46f280165abff29f429c&csf=1&web=1&e=CUSvDF)

### Incident Management 

Community Capability Operators must ensure timely identification, logging, communication, and resolution of service incidents. This includes: 

- Coordinating with the MeteoGate Service Desk, which acts as the first point of contact for all stakeholders. 
- Following agreed escalation procedures for high-impact or unresolved issues. 
- Documenting incident resolution steps and timelines. 
- Ensuring lessons learned are recorded and shared if applicable. 

MeteoGate includes custom error pages to improve user experience during service failures. 

**Problem Management** 

In addition to resolving individual incidents, Community Capability Operators are expected to: 
- Identify recurring incidents or systemic issues that indicate underlying problems. 
- Perform root cause analysis and propose long-term corrective actions. 
- Coordinate fixes or infrastructure changes that reduce the likelihood of recurrence. 
- Document known issues and mitigation strategies to support future troubleshooting. 

Problem resolution efforts should be tracked and reported where appropriate, especially for issues affecting multiple environments or stakeholders. 

### Support Requests Management 

The MeteoGate Service Desk, operated by a FEMDI consortium member, is the central contact for all stakeholders. It handles receiving, triaging, and coordinating support requests, ensuring consistent issue management.

**The Service Desk is responsible for:**
- Receiving and logging support requests from users. 
- Performing initial triage and assigning requests to the appropriate Community Capability Operator, Data Publisher, or other responsible party. 
- Coordinating the resolution process and maintaining case oversight. 
- Communicating with end users throughout the support lifecycle. 

**Community Capability Operators are responsible for:** 
- Responding to support requests escalated by the Service Desk that fall within their operational domain (e.g. API Gateway, Developer Portal, Vault). 
- Troubleshooting and resolving technical issues related to the Community Components they host. 
- Tracking the progress and status of support cases, and reporting on response or resolution times if required. 
- Collaborating with Data Publishers, other operators, or technical teams as needed to resolve incidents.

### Change Management 

Changes to infrastructure or configuration (e.g., new routes, IdPs, or platform updates) must follow a documented and controlled change process. 

Community Capability Operators must: 
- Plan and test changes before applying them in production. 
- Use version-controlled configuration (e.g., via GitHub) for transparency. 
- Coordinate with other operators and the MeteoGate Solution Manager for changes that impact shared services.

### Performance Reporting

Operators must submit regular reports (e.g., monthly) to the MeteoGate Programme Manager. These reports enable performance tracking against QoS targets and support transparency across MeteoGate. 

Reports may include: 
- Availability and uptime metrics 
- API Gateway usage statistics 
- Incident and support request summaries 
- Capacity and resource usage 

Guidelines and templates will be provided by the Programme team. Reports are compiled using monitoring tools and logs (see section: [Monitoring](#monitoring-and-observability))

_To be updated with any further requirements/guidance from EUMETNET_

### Data Management 

This section outlines the Operator's data management related responsibilities. 

#### Personal Data 

Personal data is handled according to [MeteoGate Privacy Policy](https://tlnt19059.sharepoint.com/:w:/r/sites/FEMDI/ET%20FEMDI/ET%20Working%20folder/Operating%20Model/FEMDI%20Privacy%20policy.docx?d=w86090b46ea1642dfbdf40e2bc483e503&csf=1&web=1&e=RVrHSQ), EU’s General Data Protection Regulation (GDPR), and other relevant legislation. 

Personal data processed includes: 

- Registered user identities (managed by the Identity & Access Manager) 
- API key metadata (managed by the Key Vault) 
- User access control metadata, stored as consumer objects (managed by API Gateway) 
- Contact information in dataset metadata (provided and managed by the Data Publisher) 

GDPR-related requests (e.g., deletion or correction) should be directed to the MeteoGate Service Desk, which coordinates resolution with responsible parties. 

Community Capability Operators are responsible for: 

- Ensuring that personal data processed by the Community Components they operate (e.g. in Identity and Access Manager, Key Vault, and API Gateway) is stored securely and only accessible by authorised personnel. 
- Supporting GDPR-related processes in their hosted environment (e.g. deletion of user data upon request). 
- Implementing technical and organisational measures to protect personal data in accordance with MeteoGate’s security and privacy requirements.

#### Meteorological and Hydrological Data

Community Capability Operators provide the technical platform for publishing and accessing data but are not responsible for the content or quality of the datasets. Responsibility for the accuracy, structure, and compliance of data and metadata lies with the respective Data Owners and Publishers. 

Community Capability Operators must: 
- Ensure reliable and secure API access to published datasets. 
- Refrain from altering, filtering, or interpreting the published content in any way. 

For more detailed definitions of roles and responsibilities related to data handling, publication, and ownership, refer to the [MeteoGate Data Governance Policy](https://tlnt19059.sharepoint.com/:w:/r/sites/FEMDI/ET%20FEMDI/ET%20Working%20folder/Operating%20Model/Data%20governance%20policy.docx?d=wb938b06e5b694bf1914c4202ee6aaa8a&csf=1&web=1&e=AH2lgh).

> to be updated
