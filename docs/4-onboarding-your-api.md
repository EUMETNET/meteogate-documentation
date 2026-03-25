# Steps to Publish Data Through MeteoGate
There are 3 patterns to publish data through Meteogate with more details found in the [documentation](https://eumetnet.github.io/meteogate-documentation/3-publishing-data/)  
 
 Instructions in this repository concentrate on publishing Pattern 2: Managed and Proxied Access Through MeteoGate API Gateway.

## Pattern 1: Using a MeteoGate HVD Service

* Observations (E-SOH)
* Radar (ORD)
* Climate
* Warnings (MeteoAlarm)
  
## Pattern 2: Managed and Proxied Access Through MeteoGate API Gateway

The Data Supply component is registered with an appropriate MeteoGate API Gateway​. Currently, these are deployed on the EWC, in ECMWF and EUMETSAT runtime environments. EUMETNET Members will connect their data services via EWC irrespective of the location of their data (e.g., EWC, public cloud, on-premises).​ ​In the future, if requirements change, MeteoGate API Gateway can be implemented also on public clouds.

The data is requested from and goes through the MeteoGate API Gateway allowing:

 * Managed data requests and data flow e.g., authorization, access control, and rate limiting
 * Collection of insights on data requests and access​.

This pattern is appropriate for the following situations​:

 * There is no suitable HVD service (Pattern 1) for the type of data to be shared.
 * Data is hosted on EWC, or data access APIs will be hosted on EWC.
 * Prefer minimal IT investment while ensuring the data is discoverable, accessible, and compliant with MeteoGate requirements.
 * Want to benefit from improved security, efficiency, manageability, and cost-effectiveness, as MeteoGate handles security, rate limiting, and monitoring.

Read more about the MeteoGate API Gateway features on [MeteoGate Architecture](https://eumetnet.github.io/meteogate-documentation/technical-architecture/).

## Pattern 3: Direct Access
* Use your own API / file hostings and adverise direct links to data
* Add discovery metadata to WIS 2
* Add notifications to WIS 2 (that point directly to your service)

# Instructions on Using Pattern 2
The following sections contain the requirements for your API and detailed steps on publishing your data using Pattern 2.

## Things to Consider Before Using Pattern 2
 * What kind of ratelimiting do you want to enforce for users accessing your API through Meteogate?
 * Do you want the API to be publicly accessible or only to users registered to Meteogate?
 * Does your API require authentication to access?

## API Requirements
 * For best user experience, ensure your API is capable of handling and providing dynamic URLs
    * i.e. if your API returns any URLs back to the API published they should not be hardcoded and instead use the origin URL

## Steps to Publish Your API Through MeteoGate Gateway
1. Make sure you've gone through the considerations and requirements
2. Copy one of the example configurations in the examples folder in this repository which best suits your needs
3. Fill in your API details to the configuration file
    * Any API is ok (WMS, EDR, openapi)
    * For `id` and `route.id` follow the _naming convention_
       * `<country code>-<organization>-<API type/name>-<additional information>` where _additional information_ is optional and all other mandatory
       * e.g. `fi-fmi-edr`, `fi-fmi-timeseries`, `eu-eumetnet-surface-observations`, `fi-fmi-wms-forecast`
       * The `id` will be the route postfix used to access through MeteoGate e.g. `https://api.meteogate.eu/eu-eumetnet-surface-observations`
    * Choose rate limits to users (optional)
       * For unregistered users we enforce _default_ ratelimits
    * Choose to allow anonymous and / or only registered users access
       * By default anonymous access is allowed with the _default_ ratelimits
    * Set upstream API authentication (between MeteoGate API GW and your API) if needed
       * Provide the correct `header` with `headerParam` used for _authentication_ with your API
       * e.g. `Authorization`, `X-API-Key`
    * set CORS header
4. Save the config file
5. Contact Meteogate service desk through [contact form](https://ilmatieteenlaitos.atlassian.net/jira/core/form/83953b5d-a13d-4a66-bb40-7fdfd1a1f2a4)
    * In the message provide the `configuration` as a _code snippet_ which is accessible by pressing the `+` in the description field
    * If your API requires upstream API authentication provide the `API-KEY` in the message as a separate _code snippet_
    * If accessing your API requires _whitelisting_ of *ip block* please state that in the message
6. Wait for somebody from the MeteoGate team to contact you
7. When the onboarding process is complete
    * Test your api through MeteoGate API gateway at `https://api.meteogate.eu/<route_id>`
    * Test registered access by registering your own apikey at https://devportal.meteogate.eu/
    * Test your api with apikey at `https://api.meteogate.eu/<route_id>?apikey=yourapikey` (or put apikey in the header of the request)

### Example 1. Basic Configuration Limiting Unauthenticated Access
```
id: fi-fmi-edr <- Follow the naming convention
version: 1.0.0
platforms: 
  - EUMETSAT
  - ECMWF
routes:
  - route:
      id: fi-fmi-edr <- Follow the naming convention
      endpoint: https://opendata.fmi.fi/edr
      ratelimitAnon:
        requestRate:
          rate: 10 <- Anonymous users are restricted to 10RPS by default
          burst: 20
        quota:
          count: 50 <- Anonymous users are restricted to 50RPH by default
          time_window: 3600
      cors: true
```

### Example 2. Authenticated User Rate Limiting
```
id: fi-fmi-edr
version: 1.0.0
platforms: 
  - EUMETSAT
  - ECMWF
routes:
  - route:
      id: fi-fmi-edr
      endpoint: https://opendata.fmi.fi/edr
      ratelimitAnon:
        requestRate:
          rate: 10
          burst: 20
        quota:
          count: 50
          time_window: 3600
      ratelimitAuth: <- Added limits for authenticated users
        requestRate:
          rate: 60 <- 60 RPS
          burst: 100
        quota:
          count: 500 <- Define the amount
          time_window: 3600 <- Define the time window in seconds
      cors: true
```

### Example 3. Allow API Usage Only to Registered Users
```
id: fi-fmi-edr
version: 1.0.0
platforms: 
  - EUMETSAT
  - ECMWF
routes:
  - route:
      id: fi-fmi-edr
      endpoint: https://opendata.fmi.fi/edr
      apikeyOnly: true <- Only users registered to MeteoGate have access
      ratelimitAuth:
        requestRate:
          rate: 60
          burst: 100
        quota:
          count: 500
          time_window: 3600
      cors: true
```

### Example 4. Upstream Requires Authentication
```
id: fi-fmi-edr
version: 1.0.0
platforms: 
  - EUMETSAT
  - ECMWF
routes:
  - route:
      id: fi-fmi-edr
      endpoint: https://opendata.fmi.fi/edr
      ratelimitAnon:
        requestRate:
          rate: 10
          burst: 20
        quota:
          count: 50
          time_window: 3600
      apikey:
        headerParam: 'Authorization'
        secretName: FI-FMI-EDR-KEY
      cors: true
```

### Example 5. Full Config Combining the Above
```
id: fi-fmi-edr
version: 1.0.0
platforms: 
  - EUMETSAT
  - ECMWF
routes:
  - route:
      id: fi-fmi-edr
      endpoint: https://opendata.fmi.fi/edr
      ratelimitAnon:
        requestRate:
          rate: 10
          burst: 20
        quota:
          count: 50
          time_window: 3600
      ratelimitAuth:
        requestRate:
          rate: 60
          burst: 100
        quota:
          count: 500
          time_window: 3600
      apikey:
        headerParam: 'Authorization'
        secretName: FI-FMI-EDR-KEY
      cors: true
```
