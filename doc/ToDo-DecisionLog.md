# ToDo Application Decision Log

The purpose of this document is to record the architectural decisions made and the reasoning behind them. For every component there will be an analysis regarding

* Scalability
* Security
* Performance
* Cost
* Availability & SLA

The application is made up of the following components

* Web UI
* Identity provider
* API
* Database

## Web UI

*Decision*: The web UI is implemented as a Single Page Application (SPA).

* This enables a better user experience because of lower interaction times compared with traditional apps.
* The application does not need to be crawled by web indexers like Google. This means there are no constraints on how the page rendering is done

*Decision*: The web UI is hosted on Azure Storage as a static website with an Azure CDN.

* Designing the Web UI as a SPA means that it can be hosted *statically* meaning that there is no code running on a server and all files that are part of the Web UI just need to be delivered to the browser. The most cost-effective and scalable way to do this is to use Azure Storage Static Websites.
* The addition of a CDN brings two benefits:
  * Quicker delivery of the SPA files to the browser from Edge locations closer to the User's location
  * The ability to use HTTPS. This is currently not supported with Azure Storage Static Websites, so a CDN becomes necessary

### Web UI Quality Attributes

* Scalability
  * Azure CDN is the component that will serve requests even if the underlying infrastructure experiences an outage. Therefore only Azure CDN will be considered here
  * Azure CDN has global scalability built in.
* Security
  * Azure CDN does not run any code directly, therefore only the control plane needs to be considered.
  * The Azure CDN Control Plane is integrated with the Azure Portal which is considered secure
* Performance
  * Azure CDN is designed for performance. It is a global network of edge locations that can handle massive amounts of requests.
* Cost
  * The cost for Azure CDN is consumption based.