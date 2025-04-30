# 2. Discovering and Accessing Data

This section explains how to find and access meteorological and hydrological data through the MeteoGate system. It describes the tools available for discovering datasets, subscribing to notifications, and retrieving data via APIs. The section also provides guidance on registering as a MeteoGate user, authentication, API Keys, and how to use the data in your own systems or workflows. Whether accessing data through the API Gateway or directly from a Data Supply component, this section will help Data Consumers make effective use of MeteoGate’s services.

## Terms and Conditions on Using Data

The MeteoGate Participation Management policy outlines the agreements Data Consumers must respect. In addition, the MeteoGate Data Governance policy includes Data Consumer Fair Use.

Data Consumers must agree to the following MeteoGate Terms of Use:

Open data can be accessed freely, but consumers must follow data usage rights and use data within terms of the licenses as specified in the metadata. In addition, consumers must comply with the Data Governance policy re. fair use. 
-	Any abusive behaviour towards other users or staff members will result in a warning, suspension or ban
-	Users must comply with the law of their country when using MeteoGate
-	Any attempt to gain entry to locked or restricted areas will result in a permanent ban
-	Any unlawful activity on the website may result in referral to law enforcement agencies.

> Add link. Include data access on API Gateway, Data Supply and Data Explorer.

## Discovering Data

The first step in utilizing MeteoGate to access data is to find data that suits your needs.

###	Using the Data Explorer to discover data

You can browse and search for datasets available through MeteoGate using the MeteoGate Data Explorer. It is a lightweight web application that can be used to explore the available API endpoints, and the datasets and parameters available through them. The Data Explorer supports both anonymous and authenticated access to data.

The Data Explorer uses metadata and data from Global Discovery Catalogue and MeteoGate-compliant Data Supply components to provide detailed information on what data is available through MeteoGate. It also pulls in supplementary station information from OSCAR/Surface. This allows you to evaluate the suitability of data.

When you have found the data that you wish to use, you can copy the appropriate API endpoint URL from Data Explorer and use it to access the data.

> authentication? API Key; add link

### Searching the Global Discovery Catalogue

Alternatively, you can browse the datasets available through MeteoGate and WIS 2.0 using the WMO WIS 2.0 Global Discovery Catalogue. It is a web application that uses discovery metadata to provide information on what data is available. Global Discovery Catalogue is part of the MeteoGate ecosystem, but is managed by WMO Members to meet WIS 2.0 requirements, not by the FEMDI Programme.

You can search for datasets using keywords, geographic area of interest, temporal information, or free text. The Global Discovery Catalogue provides high-level information (title, description, keywords, spatiotemporal extents, data policy, licensing, contact information) and URL links of the datasets.

See the Guide to WIS 2.0 for more information on using the Global Discovery Catalogue.

> add link

### Subscribing to notifications about availability of new data

You can subscribe to receive notifications about new and updated data and metadata. You can choose the API endpoints, datasets, and topics for which you want to receive notifications. The notifications also include URL that can be used to access the newly added data. Notifications are especially useful in utilizing real-time data.

Notifications are published on Data Supply components and republished at the WMO WIS 2.0 Global Broker. You can subscribe to notifications at multiple Global Brokers to ensure resilient communication. The Global Broker is part of the MeteoGate ecosystem, but is managed by WMO Members to meet WIS 2.0 requirements, not by the FEMDI Programme.

You can subscribe to specific WIS2 topics to receive notifications on the data types you are interested in. Topic structure follows the WIS 2.0 Topic Hierarchy. The Message Broker Protocol (MQTT 3.1 or MQTT 5) is used to convey the notifications. The notifications are formatted according to WIS 2.0 Notification standard.

See the Guide to WIS 2.0 for more information on subscribing to notifications.
 
## Accessing Data

Once you have found a dataset that fits your needs, you can access it either anonymously or as a registered user, depending on the data policy. This section explains how to register for API access (if required), use API Keys, and retrieve data through MeteoGate API Gateway or directly from Data Supply services.

### Registering to MeteoGate and creating API Keys 

Most of the data accessible through MeteoGate is open to anonymous users and does not require registering as a MeteoGate user. But when required by a data policy and in line with EU regulation, the MeteoGate API Gateway requires authentication from the user using an API Key.  Users can also register to receive higher Quality of Service than anonymous users.

Users must register in the MeteoGate Developer Portal to obtain an API Key.

Users from EUMETNET Members can receive priority access for official duty by registering as EUMETNET Member users.

The API Key (which is a string of characters) is appended to the HTTP header of the data access request or alternatively provided as an URL parameter.

**Registering as normal user**

Navigate to MeteoGate Developer Portal and log in with your preferred identity provider. If required, input missing account information such as first and last name. You are then taken to the Developer Portal front page.

**Registering as EUMETNET Member user**

Navigate to MeteoGate Developer Portal and log in with your organisation’s identity provider (e.g. Azure AD). The identity provider should provide your work email address, showing that you are affiliated with a EUMETNET Member. If required, input missing account information such as first and last name. You are then taken to the Developer Portal front page.

Send email to the MeteoGate helpdesk with a request for priority EUMETNET Member access, including the email address you used to register to the Developer portal. The MeteoGate support agent will verify that you are affiliated with a EUMETNET Member and add you to the appropriate EUMETNET Member group.

**Creating API Key**

You can obtain a new API Key by clicking the ‘Create API Key’ button in the Developer Portal. The API Key (which is a string of characters) is subsequently shown.

**Deleting API Key**

If there is a need to revoke your API Key (for example, the API Key has been compromised), you can delete the API Key by clicking the ‘Delete API Key’ button in the Developer Portal.

**Viewing available API endpoints (i.e. routes)**

You can view the API endpoints (i.e. routes) available through the MeteoGate API Gateways by clicking the ‘Show routes’ button in the in the Developer Portal.

###	Registering as a local user on the Data Supply 

There are Data Supply components that require users to register, but which handle authentication and access control locally. You cannot use a MeteoGate API Key to access data on these Data Supply components but need to create a local user account and authenticate following the Data Provider’s instructions.

###	Accessing data through MeteoGate API Gateway

MeteoGate API Gateways provide managed access to the local data sources that are available through MeteoGate. The API Gateways manage data requests and data access flow by e.g., providing cybersecurity, priority access, request limiting, and rate limiting measures. The API Gateway also collects usage data. There are several API Gateways available. The Data Publisher chooses which API Gateway instance they want to use. It is also possible to publish data directly, without using the API Gateway.

Data is accessed by sending an access request to the API. Data can be accessed with a web browser, a specialised software program, or programmatically (i.e. from software code).

Note that these instructions apply specifically to OGC-API-EDR type of API.

**What is an access request?**

Accessing data begins with formulating a data access request. It is basically a text string send to the MeteoGate API Gateway. It includes the URL for the particular API endpoint and parameters. The API Key (which is also a text string) must be included for restricted data.

A data access request may like this, for example:

```https://apisixdev.eumetnet-femdi.eumetsat.ewcloud.host/fi/edr/collections/ecmwf/instances/20240911T000000/locations/100683?&datetime=2024-09-11T06:00Z/2024-09-12T09:00Z&parameter-name=Temperature&crs=CRS:84&f=GeoJSON```

In the example, the parts of the access request are as follows:

API endpoint: ```https://apisixdev.eumetnet-femdi.eumetsat.ewcloud.host/fi/edr/collections/ecmwf/instances/20240911T000000/locations/100683```

In the example, the request is targeting a specific dataset (ECMWF data), for a particular date and time (```2024-09-11T00:00:00```) and at a specific location (```100683```). The API endpoint specifies the API base URL including the type of API (```https://apisixdev.eumetnet-femdi.eumetsat.ewcloud.host/fi/edr```), collection or dataset (```ecmwf```), instance of a collection (in this case, data taken at a particular time: ```20240911T000000```), specific location within the collection instance (in this case, a station id: ```100683```). You can see from the base URL, that it point to the MeteoGate API Gateway.

Parameters: ```&datetime=2024-09-11T06:00Z/2024-09-12T09:00Z&parameter-name=Temperature&crs=CRS:84&f=GeoJSON```

Parameters are often specified after a ? symbol in the access request. In the example, the parameters request temperature data for the time range between September 11, 2024, 06:00 UTC, and September 12, 2024, 09:00 UTC. The data should be returned in GeoJSON format, using CRS:84 as the coordinate system.

**How to find the API**

First, it is necessary to identify the API from which the data will be retrieved. The collection metadata for the API in question specify how the access request should be formulated. For example, it specifies which collections (datasets) and parameters are available.

There are several ways to find the API that can provide data for your needs:

1.	MeteoGate Data Explorer (recommended, see above)
2.	WMO WIS2 Global Discovery Catalogue (see above)
3.	Route list in MeteoGate Developer Portal (see above). Note that the route list includes only Data Supply components that are accessed through MeteoGate API Gateway(s).
5.	Notification message (see above)
6.	Search for datasets on a web search engine, e.g. Google Dataset Search
7.	Data Supply (e.g. WIS2Box UI)

The API base URL looks like this, for example: ```apisixdev.eumetnet-femdi.eumetsat.ewcloud.host/fi/edr```

> Bulk download? different link?

**How formulate the access request**

There are several ways to formulate the access request:

1.	Use an API query tool (e.g. Swagger), if available
    - Browse available collections and parameters from an API using specialist software, which formulates access request according to your preferences
    - Copy the resulting access request
2.	Copy the URL link from a notification message
    - In case you have subscribed to notifications about particular data from the Global Broker, the notification message includes a direct URL link to the data you have subscribed to
4.	Formulate manually using metadata from API
    1. Access the API collection URL to get collection metadata, which provides information on available collections and parameters (e.g. ```https://apisixdev.eumetnet-femdi.eumetsat.ewcloud.host/fi/edr/collections```)
    2. Browse collection metadata to find collections and parameters to data that are available from that API.
    3. Decide what data you want to get (e.g. collection type, collection, type of data, time, geographic extent, output formats, other parameters)
    4. Formulate the access request by combining the API base URL, collection, and parameters.

> follow formats specified in metadata, e.g. date and time

**How to send the access request**

Data can be accessed with a web browser, a specialised software program, or programmatically (i.e. from software code). Data access happens behind the scenes. There is no UI for the MeteoGate API Gateway.

If using a web browser, just input the access request to the address bar and press enter. If the request was input correctly, a response is opened with the requested data (typically in JSON format).

You can also access data from software code using e.g. Python by sending the access request as a GET request to the API.

In addition, there are specialised software programs that can be used to access and display data. MeteoGate Data Explorer is an example of such a solution.

> add examples. code libraries mentioned?

**How to use data**

Use data according to terms and conditions and license.

> reference to terms of use. what else? how to use in code etc.)

**How to access Data Supply components requiring authentication**

For API endpoints that require authentication using API Key, the API Key is appended to the HTTP header of the data access request or alternatively provided as an URL parameter. Metadata should state whether the endpoint requires authentication.

### Accessing data directly from through Data Supply APIs

Some Data Supply APIs are not accessed through the MeteoGate API Gateway(s), but directly. You can still find these APIs by the same means as the APIs that are accessed through the MeteoGate API Gateway(s), for example using the Data Explorer or WIS 2.0 Global Discovery Catalogue.

Follow the instructions available from the particular Data Provider to access these APIs. You should find the required links or information on the Data Provider from the metadata or a landing page available from the API base URL. Some Data Supply components may require authentication.

> to be updated
