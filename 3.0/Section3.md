# 3.0 Cisco Platforms and Development
## 3.1 Construct a Python script that uses a Cisco SDK given SDK documentation
### What is an SDK?
Colection of:
* libraries, 
* APIs, 
* documentation,
* tools, 
* sample code, 
* processes     
for a specific language, with the purpose of making it easier and faster for developers to work with a particular product.

Source for Cisco SDK's: https://developer.cisco.com

## 3.2 Describe the capabilities of Cisco network management platforms and APIs (Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, and NSO)
### Meraki
https://developer.cisco.com/meraki/    
SDKs (all use the Dashboard API):
* Python library python-meraki
    * pip install meraki
    * can simulate POST/PUT/DELETE to preview frist
    * pagination, retries
* Node.js SDK - deprecated March 2020
* Ruby SDK - deprecated March 2020

Multiple APIs:
* Meraki Dashboard API
    * Base endpoint URL : https://api.meraki.com/api/v0
    * you can make direct HTTP requests to dashboard API in any programming language or REST API client
    * v0 is mainly in use. v1 is in beta
    * uses API_KEY for authentication. to be supplied in every request header as "X-Cisco-Meraki-API-Key"
    * 'Accept: application/json'
    * requires an Organization ID and sometimes Network ID to be specified
    * requires API access to be enabled in the web dashboard
    * 
* Captive Portal API
* Scanning API
* MV Sense Camera API
    * Can use REST or MQTT

Webhooks:
* configure webhooks in the web dashboard against specific alerts/events

### DNA Center
https://developer.cisco.com/docs/dna-center/    

#### Multiple APIs:
* Intent API (Northbound)
    * as in, intent-based-networking
    * RESTful
    * JSON
    * Authentication Domain
        * Token required for REST calls
    * Know Your Network Domain
        * Use the Know Your Network REST methods to GET information about clients, sites, topology, devices, and issues; Create (POST) and manage (PUT, DELETE) sites, devices, IP Pools, edge and border devices, and authentication profiles.

    * Site Management Domain
        * Site Design
            * These methods to create and obtain information about provisioned NFV devices.
        * Network Settings
            * Global stuff like global IP pools
            * Device credentials
        * Software Image Management (SWIM)
            * software image and update repository for network devices.
        * Device Onboarding (PnP)
            * device onboarding projects, settings, workflows, virtual accounts, and PnP-managed devices.
            * i.e. Zero-touch provisioning
        * Configuration Templates
            * CLI templates - create, view, edit
    * Connectivity Domain
        * Fabric Wired
        * Non-Fabric Wireless
            * SSID Provisioning
    * Operational Tasks Domain
        * Command Runner
            * read-only commands
        * Network Discovery
            * scan a network using SNMP, SSH etc. and list of known credentials
        * Path Trace
            * flow-analysis between two endpoints on the network.
        * File operations
        * Task
            * information about asynchronous requests
        * Tags
    * Policy (Application Policy) Domain
        * The policies used to translate intent into device configurations
    * Event Management Domain
        * custom notifications for specific events
* Multivendor Support (Southbound)
    * Leverage DNAC's functionality of of creating devices packages for 3rd-party devices
    * Using CLI, SNMP or Netconf
* Events and Notifications (Eastbound)
    * Notifications triggered by DNAC events
    * Note: I don't really understand the difference between this API and the Event Management module of the Intent Management API
* Integration API (Westbound)
    * Integration with 3rd party IT Service Management platforms
    * Service Desk, Change Management, Reporting, etc.
    * supports the IT4IT™ Reference Architecture, including the standards for events, incidents, problems, and request for changes.
* RMA API
    * For swapping out replacement devices
* SDA API
    * For managing the software-defined-access fabric
* Topology APIS
    * Returns L2 and L3 topologies


* DNAC APIs have a well structured and documented API lifecycle
* DNAC has internal IPAM and also has API integrations for following IPAM providers:
    * Infoblox
    * Bluecat

* DNAC supports webhooks
* Header for token authentication = 'X-Auth-Token'
    * Token is received after completing Basic Auth
* 'Content-type': 'application/json'
* DNAC APIs are rate limited to 5 requests per minute

#### SDKs
* Python SDK
    * pip install dnacentersdk
```
from dnacentersdk import api

# Create a DNACenterAPI connection object;
# it uses DNA Center sandbox URL, username and
password

DNAC = api.DNACenterAPI(username="devnetuser", password="Cisco123!", base_url="https://sandboxdnac2.cisco.com")

# Find all devices
DEVICES = DNAC.devices.get_device_list()
```

### ACI
https://developer.cisco.com/site/aci/    
#### About ACI
* The Nexus 9000 has two modes of operation to fit different operational models: NX-OS mode and Application Centric Infrastructure (ACI) mode.
* ACI Mode uses a cluster of 3 controllers (APICs)
    * Application Policy Infrastructure Controllers
* ACI Mode provides spine/leaf architecture 
    * APICs connect to leaf switches
* ACI Mode switches are **only** programmable via the policy engine on the APIC.
    * Configuration is held as XML or JSON on the controller
    * Each switch also holds a copy of the Concrete model
    * "Desired state network" aka "Model driven framework"
* ACI Tenants
    * separate network/failure domains
    * divided into Tenant Networking and Tenant Policy
    * Tenant Networking
        * L2 and L3 connectivity, VRFs, bridge domains, subnets, etc.
    * Tenant Policy
        * Application profiles, end point groups, contracts, filters
        * Examples: Allow webservers in EPG (end point group) Delta to connect to SQL DB servers and apply specific QoS

#### MIM Managemnent Information Model
* MIM is represented in a tree, the MIT
* Nodes in the tree are any physical or logical Management Object, the MO
* MOs use Distinguished Name naming convention 
* Tenants are top-level MOs
#### RESt Interface
* Northbound: REST API, CLI and GUI for operators and developers
* Southbound: OpFlex protocol for managing the Nexus switches
* APIC is designed from day one around APIs
* https://APIC_Host:port/api/{mo|class}/{dn|clas
sname}.{xml|json}?\[options]
    * Content-Type headers are ignored
* Authentication is done via the generic endpoint above, against the aaaLogin.js which returns a Token
    * Token is then supplied in subsequent requests as a cookie (APIC-cookie)
#### APIC SDKs
* Python SDK: **Cobra**
* Python library: **acitoolkit**/ **pyaci**
* Ruby: **ACIrb**
* Puppet and Ansible extensively supported



### Cisco SD-WAN (Viptela)
https://developer.cisco.com/docs/sdwan/    

#### About Cisco SDWAN
The Cisco SD-WAN solution is based on the Viptela acquisition. The components of the solution are: vManage, vSmart, vBond and vEdge.

* vManage
    * Network Management System (NMS) component 
    * provides a "single pane of glass" for Day 0, Day 1 and Day 2 operations
    * centralized provisioning, monitoring, and troubleshooting
    * where policies and templates are defined
    * single tenant ot multi-tenant
    * RBAC is enforce here: roles include operater, netadmin, custom
    * REST API is exposed at the vManage layer

* vSmart
    * main control plane component of the solution
    * fabric discovery, 
    * disseminates control plane information between vEdges 
    * distributes data plane policies to vEdge routers
    * implements control plane policies.

* vBond
    * orchestrator of the fabric
    * orchestrates both the control plane and the management plane 
    * first point of authentication for all the fabric components
    * distributes the list of vSmart and vManage components to the vEdge routers
    * facilitates NAT traversal
    * requires universally reachable (public) IP address by all the fabric components.
    * single and multi-tenant

* vEdge
    * vManage, vSmart, and vBond are virtual and can run in both VMware ESXi and KVM virtualization environments
    * vEdges are the routers that get deployed at all the sites throughout the fabric.
    * establish secure control plane connections with the vSmart controllers
    * proprietary Overlay Management Protocol (OMP) 
    * enforce the desired data plane policies
    * Zero Touch Deployment is supported
    * traditional routing protocols (OSPF, BGP) and (VRRP). 
    * virtual or hardware appliances

* IOS-XE
    * select ISR 1000 series, ISR 4000 series and ASR 1000 series, CSR1000V routers can run as vEdge

#### Cisco SD-WAN REST API
* Used for
    * Monitoring device status
    * Configuring a device (templates)
    * Querying and aggregating device statistics
* API Authentication
    * POST */j_security_check* with content type *x-www-form-urlencoded*
    * user name and password are submitted as *j_username* and *j_password*
    * session token is in the response http cookie, *JSESSIONID={session hash}*
    * use the session cookie in ongoing API requests
* API Security (Cross Site Request Forgery)
    * added in v19.2
    * XSRF token is also needed in addition to the auth token
    * GET */dataservice/client/token* with content type *application/json*
    * *JESSIONID={session hash}* cookie from previous step is required to authenticate
    * XSRF token is in the response body
    * Use the XSRF token along with the JESSIONID cookie for ongoing API requests
* Logout
    * http response code is 302 redirect with location header *https://{vmanage-ip-address}/welcome.html?nocache=*
* Token header example
```
https://{vmanage-ip-address}/dataservice/{api-endpoint-url}
Content-Type: application/json
HTTP Header: "Cookie: JESSIONID={session hash id}" "X-XSRF-TOKEN: {XSRF token}"
```
* It is mandatory to share the same session if multiple API requests are invoked sequentially
    * default session lifespan is 24 hours
    * session inactivity timeout is 30 minutes.
    * maximum concurrent session number is 250
* Base URI
    * https://<vmanage-server>/dataservice
* vManage API categories
    * Administrative and management APIs
        * user, group and tenant management
        * software maintenance
        * backup and restore
        * container management.
    * Alarm and event APIs
        * alarm and event notification config
        * alarm, event, and audit log queries
    * Configuration
        * feature template
        * device template
        * device policy 
        * device certificate
        * device action
        * action status
        * device inventory
    * Device real-time monitoring
        * devices, links, applications, systems
    * Device state, statistics bulk APIs
        * device states
        * aggregated statistics
        * bulk queries
    * Troubleshooting and utility

#### Cisco SDWAN SDKs
* **python-viptela** provides a Python SDK for interacting with Cisco SDWAN (formerly Viptela) vManage
* CLI and Ansible modules leverage the SDK
* *pip install viptela*


### NSO (Network Services Orchestrator) (formerly Tail-f)
https://developer.cisco.com/docs/nso/#!nso-fundamentals    

#### About NSO
* NSO product is a powerful, flexible, extensible toolbox for network automation and orchestration needs
    * For service provider networks
* device abstraction capabilities enable you to automate a wide variety of Cisco and third-party devices, tools, and 
    * devices drivers are called *NED*s (Network Element Drivers) and are used for abstraction
* YANG-based models
* CDB (Configuration DataBase)
    * core of NSO
* Fastmap
    * algorithm to turn YANG models into device-specific config
* NEDs send the configs to devices
    * NETCONF is preferred
        * delivered via SNMP SET or Cisco CLIs
    * for non-NETCONF or non-Cisco devices: custom CLI or REST NEDs can be used

#### NSO APIs
* Northbound API - RESTCONF
    * Refer to swagger docs (200,000 lines of API doc)
    * RESTCONF must be enabled in the ncs.conf configuration file
    * base endpoint is */restconf*
    * basic authentication
    * Request Header: *"Accept: application/yang-data+xml"*

#### NSO SDKs
* Python
    * package name *ncs*
    * can browse YANG models using PYthon dot notation
* Java
* Erlang
    * econfd


## 3.3 Describe the capabilities of Cisco compute management platforms and APIs (UCS Manager, UCS Director, and Intersight)
### UCS Manager
* UCS Manager is the single point of management for a UCS Domain.
* UCS Domains can contain up to twenty blade chassis with each chassis holding up to eight half-width blades.
    * up to one hundred and sixty servers total
* Unifies management of Cisco UCS blade and rack servers, Cisco UCS Mini, and Cisco HyperFlex
* Policy-driven, model-based architecture
* Management Information Model (MIM), Management Information Tree (MIT) etc.
#### UCS Manager SDKs
* Powershell: **UCS PowerTool**
* Python: **ucspython**: *pip install uscmsdk*


### UCS Director
* Provides the foundation for infrastructure as a service (IaaS), including a self-service portal for end users
* Supported by independent hardware and software vendors through open APIs
* automates, orchestrates, and manages Cisco and third-party hardware
#### UCS Director API
* REST API
* Authentication via API tokem
    * Example header is *X-Cloupia-Request-Key: F90ZZF12345678ZZ90Z12ZZ3456FZ789*
* Example
```
The sample JSON-based API URL to retrieve all catalogs using the GET method:
http://<serverip>/ app/api/rest?formatType=json&opName=userAPIGetAllCatalogs&opData={} 
```
#### UCS Director SDKs
* Java
* PowerShell


### Intersight
https://intersight.com/apidocs/introduction/overview/    
* Cloud-hosted management for Cisco UCS and Cisco HyperFlex

#### Intersight API
* API calls to intersight.com:443
* REST API, json only
* Read and write methods
* Example
```
POST https://intersight.com/api/v1/Server/Profiles/5a5f30237a6d67337212e705
Content-Type: application/json
Body:
{
  "Tags": [
    {"Key": "Site",        "Value": "London"},
    {"Key": "Environment", "Value": "Scale"},
    {"Key": "Owner",       "Value": "Bob"}
  ],
}
```

#### Intersight SDKs
* intersight-ansible
* intersight-terraform-modules
* PowerShell
* Python
* ServiceNow plugin

## 3.4 Describe the capabilities of Cisco collaboration platforms and APIs (Webex Teams, Webex devices, Cisco Unified Communication Manager including AXL and UDS # interfaces, and Finesse)

### Webex Teams
* https://developer.cisco.com/site/webex-101/
* Conversations in Webex Teams take place in virtual meeting rooms called spaces. 
* Some spaces live for a few hours, while others become permanent
* Includes messages, video, whiteboard
* Org -> Teams -> Rooms -> Users (membership)
* APIs allow
    * integrations
    * bots
    * admin tasks

#### Webex Teams API Authentication
* Personal Access Tokens
    * authentication header
    * short 12-hour lifetime, used for app development
* Integrations: OAuth 2 token
    * Register integration at https://developer.webex.com/my-apps/new
    * Request permission at https://webexapis.com/v1/authoriza
    * Retrieve the authZ code from the URL
    * 'Scopes' determine what the integrated app can access
        * e.g. spark:rooms_write = manage Rooms, spark:rooms_read = list Rooms
* Guest Issuers
    * special tokens for e.g. visitors to your website that you want to chat with
    * JSON Web Token (JWT)
    * Guests cannot interact with other guests
    * https://developer.Webex.com/my-apps/new/guest-issuer
    * Guest Issuer secret is used to generate a Bearer Token for the guest's session

#### Webex Teams APIs
* Legacy URI: https://api.ciscospark.com
* New URI: https://webexapis.com

* Organizations API
    * Organization = "a set of people"
    * Administrator access only
    * List org names, list org details
    * e.g. to list all Orgs: 
    ```
    GET https://webexapis.com/v1/organizations 
    ```

* Teams API
    * Team = "a group of people who can see the same set of rooms"
    * API to create/delete/rename Teams
    * e.g. to create a new Team:
    ```
    POST https://webexapis.com/v1/teams
    Headers:
    'Authorization' : 'Bearer longstring12234etcDBAFafgr446565fgsfGsN-'
    'Content-Type' : 'application/json'
    Payload:
    { "name" : "The Name Of The New Team"}
    ```

* Rooms API
    * Room = a place where people post messages
    * API to create/delete/rename Rooms
    * e.g. to delete a Room:
    ``` 
    DELETE https://webexapis.com/v1/rooms/RoomIDToDelete
    ```

* Membership API
    * Membership = user who can access a Room
    * API to list/invite/update/delete members
    * e.g. to add a Member to a Room
    ```
    POST https://webexapis.com/v1/memberships
    Headers:
     'Authorization' : 'Bearer longstring12234etcDBAFafgr446565fgsfGsN-'
    'Content-Type' : 'application/json'
    Payload:
    { "roomId" : "Foo234XYZbanana8865743AAABB"
    "personEmail" : "theNewguy@contoso.com"
    "personDisplayName" : "New Guy"
    "isModerator" : "false"
    }  

* Messages API
    * Messages = items posted in a Room. Plaintext, Richtext, file attachment.
    * e.g. to post a new message:
    ```
    POST https://webexapis.com/v1/messages
    Headers:
    'Authorization' : 'Bearer longstring12234etcDBAFafgr446565fgsfGsN-'
    'Content-Type' : 'application/json'
    Payload:
    { "roomId" : "Foo234XYZbanana8865743AAABB"
    "text" : "The text to be posted as a message"
    }  
    ```


* Bots API
    * Bots = chatbots that simulate interacting with a person or trigger actions
    * Bots are similiar to regular users, use @mention
        * Notification bot: some external info gets posted into a Room
        * Controller bot: are invoked from within the Room to trigger an action on some external API
        * Assistant bot: ask it questions
    * Bot Frameworks
        * Flint
        * Botkit

#### Webex Teams SDKs
* Go
* Java
* Node.js
* PHP
* Python (webexteamssdk)
* Android
* Javascript for browsers
* iOS
* Windows
* Widgets

### Webex Devices
* Collaboration devices placed in meeting rooms or desks
    * Webex Board
    * Webex Desk Pro
    * Cisco Telepresence
    * etc
* API is called **xAPI**
#### xAPI
* Commands
* Config
* Status e.g. GET http://device/status.xml
* Events
* Authentication by 
    * HTTP Basic Auth
    * Session Auth
        * POST http://device/xmlapi/session/begin (basic auth)
        * Returns session cookie 'Cookie' : 'SessionId=someverylongstring'
        * Limited numer of sessions, must gracefully log out!
* Can set webhooks alerts via POST


### Cisco Unified Communication Manager (CUCM)
* APIs used for
    * Provisioning
    * Monitoring
    * Serviceability
    * Call routing
#### AXL
* https://developer.cisco.com/docs/axl/#!axl-developer-guide/overview
* Administrative XML API
* is a SOAP API
    * must adhere to XML Schema
    * download the schema from the CUCM 
        * https://CUCM/plugins/axlsqltoolkit.zip
* requests are sent to the Unified CM Publisher server (recommended)
* Authentication by 
    * HTTP Basic Auth
        * Username and password are base64 encoded
        * use a dedicated AXL user account
    * Session cookie
        * JSESSIONIDSSO
        * 'Cookie' : 'JSESSIONIDSSO=someVeryLongstring'
* AKL service must be manually started
* Requests are only https - ensure the server cert is trusted
* Example request XML payload
```
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://www.cisco.com/AXL/API/12.5">
    <soapenv:Header/>
    <soapenv:Body>
    <ns:executeSQLQuery>
        <sql>Select name from device where tkclass = 1</sql>
    </ns:executeSQLQuery>
    </soapenv:Body>
</soapenv:Envelope>
```
* Python
    * Zeep library (generic SOAP client)
    * CiscoAXL library

#### UDS
* User Data Services
* UDS is a REST based API that allows end useres to insert, retrieve, update, and remove their *own user data* from Cisco Unified CM
* Actions to be accomplished using UDS include:
    * Directory Search for Users
    * Manage Call Forward, Do Not Disturb, and Speed Dial settings, including Visual & * Audible Alert preferences
    * Set Language and Locale
    * Subscribe to IP Phone Service Applications
    * Reset PIN/Password credentials
    * Configure Remote Destinations used with Cisco Mobility & Single Number Reach
* XML data format only
    * 'Accept' : 'application/xml'
* GET https://{host}:8443/cucm-uds/user/{userId}


### Finesse
* Browser-based contact center application (Unified Contat Center CCE/CCX)
    * Agent and supervisor desktops
    * Applications built completely on the APIs
* REST API over HTTP or HTTPS
* API Authentication by:
    * HTTP Basic Auth
        * Base64 encoded
    * If single sign on mode enabled
        * Bearer token
#### Finesse Gadgets
* Gadgets allow other JavaScript apps + Finesse to be presented in one desktop
    * OpenSocial
####  Finesse APIs
* https://developer.cisco.com/docs/finesse/#!finesse-overview/finesse-rest-apis
* User - Represents an Agent, Supervisor or Administrator
    * Get User Details
    * Change User state (e.g. Sign In, Sign Out, Ready, Not Ready, etc.)
    * Get User's Dialogs
* Dialog – Represents a call and the participants if the Media Type is voice
    * Get Dialog Details
    * Make a call
    * Take action on a participant (e.g. Answer, Hold, Retrieve, etc.)
    * Take action on a call (e.g. Update call variables, DTMF, Consult, Single Step Transfer, etc.)
    * Take a supervisor action on a call (e.g. Silent Monitor, Barge, Start recording, * etc.)
    * Make outbound related actions (e.g. Accept, close, Reject, Reclassify, Schedule, * Cancel, etc.)
* Media - Represents a user's state in a non-voice Media Routing Domain (MRD) for CCE * deployments
    * Get Media Details
    * Change User state for a MRD (e.g. Sign In, Sign Out, Ready, Not Ready)
    * Change User to Routable/Not Routable
    * Get List of non-voice Media Routing Domains for a User
* Queue - Represents a queue (or skill group in Unified CCE)
    * Get Queue Details
    * Get List of Queues for a User
* Team – Represents a team of Users
    * Get Team Details
* TeamResource - Represents a team configuration based on Team assignments
    * Get Reason Codes (Not Ready, Logout, Wrap Up)
    * Get User Media Properties Layout
    * Get Phonebooks
    * Get Workflows
* SystemInfo - Represents current state of the system
    * System Details (e.g. XMPP Server, PubSub Domains, Node IP Addresses, Status, * Deployment Type, etc.)
* ClientLogs – For sending client-side logging to the Finesse Server
    * Send Client logs to the Finesse server 




## 3.5 Describe the capabilities of Cisco security platforms and APIs (Firepower, Umbrella, AMP, ISE, and ThreatGrid)

### Firepower (NGFW)
 https://developer.cisco.com/firepower    
 * REST APIs communicate with FMC (Firepower Management Center)
     * FMC communicates with FP and FTD devices
* Token-based authentication
    * Login Request Headers should include:
        * 'Authorization': "Basic somelongBase64EncodedStringWhichIsTheAuth="
    * Response headers will include:
        * 'X-auth-access-token': "the-long-token-goes-here"
    * Subsequent requests must use the X-auth-access-token header
* API documentation is in API Explorer
    * Browse to FMC_IP/api/api-explorer
* FMC API mainly deals with CRUD operations on Objects
    * e.g. Create a new Network, Interface, or IP address

### Umbrella (DNS cloud-based security)
* Multiple APIs for different functions
    * Management API
        * Management of the account's orgs, users, network devices
        * https://management.api.umbrella.com/v1/...../Id/...../customerId
    * Reporting API
        * Organisation-level reporting of requests, identities, security activities
    * Console Reporting API
        * MSP-level reporting (i.e. multiple Orgs in a single summary)
    * Network Device Management API
        * Management of Network Devices i.e. permitting those devices that are forwarding DNS requests to Umbrella, and allocating the received requests to the correct Orgs
    * Enforcement API
        * Blocked domain lists, etc.
        * https://s-platform.api.opendns.com
    * Investigate API
        * Query Umbrella database for security events e.g. for a specific domain
        * https://investigate.umbrella.com
* Authentication
    * HTTP-basic authentication (Base64 encoded username and password)
    * Enforcement API also requires customerKey argument 
    * Investigate API requires bearer token

### AMP (Advanced Malware Protection)
* Cloud-based endpoint protection, agents installed on endpoints
* https://api.amp.cisco.com
* API uses
    * Audit logs
    * List events
    * List computers
    * List user activity
    * Search for events across the org
    * List policies
    *List vulnerabilites
* API authentication
    * API credentials are configured in web console
    * Console returns a username and password in form of ClientID and ClientKey
    * Option 1: ID and Key are prepended to the requests
        * e.g. https://clientID:clientKey@api.amp.cisco.com/..../endpoint
    * Option 2: ID and Key are Base64 encoded for use in HTTP basic Auth header
        * e.g. 'Authorization' : 'Basic TheLongBase64EncodedString'


### ISE (Identitity Services Engine/NAC)
* https://developer.cisco.com/docs/identity-services-engine/3.0/
* 2 APIs: Monitoring and External
#### Monitoring API
* REST API
    * XML output
    * XML schema is obtained from the Version API endpoint
* Example endpoint: https://<ISEhost>/admin/API/mnt/Session/MACAddress/<macaddress>
* Categories
    * Session and node data
    * CoA
    * Troubleshooting
* Monitoring API Authentication
    * User must be a member of Super Admin, System Admin, MnT Admin
    * Restricted Monitoring 'persona' - can not be used to access e.g. Policy information
#### External API
* External RESTful Services (ERS) APIs
* REST API on HTTPS, port 9060
* ERS allows external clients to perform CRUD on Cisco ISE resources.
    * e.g. Integration with external visitor management system
* External API Authentication
    * HTTP Basic authentication in request header
    * User must be a member of 'ERS Admin', 'ERS Operator'
* Must enable ERS in the ISE admin console, to open port 9060


### Threatgrid (Malware analysis)
* Now rebranded as SecureX
* https://developer.cisco.com/threat-grid/
* Cloud-based or on-prem
* API used for
    * upload files for analysis aka Submitting Samples
    * querying the database
    * deliver threat intelligence aka Curate Feeds, hourly or daily
* API authentication
    * API key
    * To receive an API key go to the Threat Grid dashboard and click your username in the top right corner. Then click My Account and then Generate API Key.
* API URL is https://panacea.threatgrid.com
* Example Request and Response for submitting a sample
```
POST /api/v2/samples?api_key=12345abcde HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Host: panacea.threatgrid.com
Content-Disposition: form-data; name="sample"; filename="test_file.txt"
Content-Disposition: form-data; name="network_exit"
Content-Disposition: form-data; name="private"
Content-Disposition: form-data; name="vm"

-----
{
  "api_version": 2,
  "id": 5760911,
  "data": {
    "tags": [
      ""
    ],
    "md5": "8f3a3bc8c6ff1a9ebf39e29e31054ddb",
    "private": true,
    "analyzing": true,
    "vm": "win10",
    "submission_id": 876379151,
    "state": "wait",
    "login": "jwick",
    "sha1": "3cebd815a45a3014498cfaa6c224071736f22f61",
    "filename": "safe.pdf",
    "status": "pending",
    "submitted_at": "2020-02-05T21:57:37Z",
    "id": "3c9b42a4dc08e2d61074f21e951446b0",
    "sha256": "73661efe4d40c8e1760052717f3df40ef0db74cfdc0b29f3c7f8bfd7c5b8a1ff",
    "os": ""
  }
}
```

## 3.6 Describe the device level APIs and dynamic interfaces for IOS XE and NX-OS
* Slightly misleading title, this seciton is really about: YANG models, NETCONF, RESTCONF, gRPC
### Model Driven Programmability
* https://tools.ietf.org/html/rfc3535
* RFC3535 = the search for a better data model (SNMP developed in 1980s)
* Which resulted in
    * NETCONF, RFC4741, 2006
    * YANG, RFC 6020, 2010
    * RESTCONF, RFC8040, 2017
    * gRPC (Google, no RFC)
* YANG is the data model
* NETCONF, RESTCONF, gRPC are the *transport protocols*
* Can be used for Northbound or Southbound APIs
#### NETCONF
* Server = "NETCONF Agent"
* Client = "NETCONF Manager"
* Can be used for YANG or non-YANG data
* Typically uses XML format
* Uses **SSH** on **port 830** as underlying communication
#### RESTCONF
* XML or JSON format
* Uses **HTTPS** on **port 443** as underlying transport
#### YANG
* Specifically designed for describing network data models
* Very structured
* Highly typed
* Modules, Containers, Lists, Leafs
* Models created by network vendors or informal groups
* Python Library: pyang
* Example IETF standard model: *ietf-interfaces.yang*

### IOS XE
* https://developer.cisco.com/site/ios-xe/
* NETCONF runs on port 830
* Enable NETCONF with command "netconf-yang"
* Enable RESTOCNF with commands "ip http secure-server" + "restconf"
* NETCONF and RESTCONF connections should be authentication by RADIUS or TACACS+
    * RADIUS VSA "shell:priv-lvl=15"
* Applications can stream data using NETCONF+YANG subscriptions
    * Periodic subscription = sent on configured interval
    * On-change subscription = sent on specific event e.g. interface down
    * Configured by using establish-connection RPC
    * Receivers can be any collector supporting IETF Pub/Sub
* On-box Python
    * iox
    * Guestshell enable (CentOS7)
* Off-box Python
    * NCCLient (NETCONF library)
    * YANG Developer Kit (YDK-py) - contains native Python bindings
```
    from ydk.services import CRUDService
from ydk.providersimport NetconfServiceProvider
from ydk.models.cisco_ios_xeimport Cisco_IOS_XE_native as xe_native

def config(interface):
    """Add config data to interface object."""
    gigabitethernet= interface.Gigabitethernet() 
    gigabitethernet.name = "2/0/10" 
    gigabitethernet.description = "Connected_to_Core_Switch" 
    gigabitethernet.ip.address.primary.address = "15.10.1.1" 
    gigabitethernet.ip.address.primary.mask = "255.255.255.0" 
    ip_add.gigabitethernet.append(gigabitethernet)

interface = xe_native.Native.Interface()  # create object 
config(interface)  # add object configuration
```
* Ansible modules
    * IOS-XE included in core Ansbile install
* Day Zero options
    * Network Plug-N-Play (APIC-EM)
    * ZTP (Download configuration Python Script from TFTP via DHCP options)
    * PXE (Download software boot image)


### NX-OS
* https://developer.cisco.com/site/nx-os/
## 3.7 Identify the appropriate DevNet resource for a given scenario (Sandbox, Code Exchange, support, forums, Learning Labs, and API documentation)
## 3.8 Apply concepts of model driven programmability (YANG, RESTCONF, and NETCONF) in a Cisco environment
## 3.9 Construct code to perform a specific operation based on a set of requirements and given API reference documentation such as these:
### 3.9.a Obtain a list of network devices by using Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, or NSO
### 3.9.b Manage spaces, participants, and messages in Webex Teams
### 3.9.c Obtain a list of clients / hosts seen on a network using Meraki or Cisco DNA Center