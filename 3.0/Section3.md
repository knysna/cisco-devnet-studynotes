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

### Webex Devices

### Cisco Unified Communication Manager (CUCM)
#### AXL
#### UDS

### Finesse


## 3.5 Describe the capabilities of Cisco security platforms and APIs (Firepower, Umbrella, AMP, ISE, and ThreatGrid)
## 3.6 Describe the device level APIs and dynamic interfaces for IOS XE and NX-OS
## 3.7 Identify the appropriate DevNet resource for a given scenario (Sandbox, Code Exchange, support, forums, Learning Labs, and API documentation)
## 3.8 Apply concepts of model driven programmability (YANG, RESTCONF, and NETCONF) in a Cisco environment
## 3.9 Construct code to perform a specific operation based on a set of requirements and given API reference documentation such as these:
### 3.9.a Obtain a list of network devices by using Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, or NSO
### 3.9.b Manage spaces, participants, and messages in Webex Teams
### 3.9.c Obtain a list of clients / hosts seen on a network using Meraki or Cisco DNA Center