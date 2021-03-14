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

### Cisco SD-WAN (Viptela)
### NSO 
## 3.3 Describe the capabilities of Cisco compute management platforms and APIs (UCS Manager, UCS Director, and Intersight)
## 3.4 Describe the capabilities of Cisco collaboration platforms and APIs (Webex Teams, Webex devices, Cisco Unified Communication Manager including AXL and UDS # interfaces, and Finesse)
## 3.5 Describe the capabilities of Cisco security platforms and APIs (Firepower, Umbrella, AMP, ISE, and ThreatGrid)
## 3.6 Describe the device level APIs and dynamic interfaces for IOS XE and NX-OS
## 3.7 Identify the appropriate DevNet resource for a given scenario (Sandbox, Code Exchange, support, forums, Learning Labs, and API documentation)
## 3.8 Apply concepts of model driven programmability (YANG, RESTCONF, and NETCONF) in a Cisco environment
## 3.9 Construct code to perform a specific operation based on a set of requirements and given API reference documentation such as these:
### 3.9.a Obtain a list of network devices by using Meraki, Cisco DNA Center, ACI, Cisco SD-WAN, or NSO
### 3.9.b Manage spaces, participants, and messages in Webex Teams
### 3.9.c Obtain a list of clients / hosts seen on a network using Meraki or Cisco DNA Center