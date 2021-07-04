# 5.0 Infrastructure and Automation
## 5.1 Describe the value of model driven programmability for infrastructure automation
* "Model driven" could refer to: YANG, RESTCONF, NETCONF, telemetry
* Provides a rich and flexible infrastructure for network automation
* Multiple protocols, multiple encoding options, multiple transport options
* Ability to use powerful, standardised libraries e.g. Python ncclient 
* Models are standardised + common, and can provide abstraction between managment tools and the device itself


## 5.2 Compare controller-level to device-level management
* Controller-level
    * Admin does not configure devices directly
    * Admin connects to controller on a Northbound interface, Controller connects to devices on Southbound interface (NETCONFT/SSH/SNMP/etc)
    * Might only give access to subset of features
    * Example: Cisco DNA Center

* Device-level
    * Admins directly configures each device
    * Full access to all features of the device


## 5.3 Describe the use and roles of network simulation and test tools (such as VIRL and pyATS)
### Network Simulation Tools
* Note VIRL = CML (rebranded as Cisco Modeling Labs)
* Old school lab = lots of physical devices in a rack
    * Inflexible, expensive, limited
* New school = simulations like VIRL/CML or GNS3
    * can run same image as the actual devices
    * can spin up many devices on x86 hardware
    * can test multiple simulations
    * save lab config in YAML file (portable to another lab)
    * limited in actual data throughput (every function only done in memory)
    * can be connected to the outisde world i.e. to the real network
* Cisco NSO has a netsim module

### Test tools
* pyATS: python library for testing, written by Cisco and used internally at Cisco
* very large python library (beware when installing)
* Only runs on Linux/Mac or maybe WSL2. not supportined directly in python on Windows
* Note: Genie = pyATS (rebranded as pyATS)
* pyATS Framework
    * defines topologies and connections
* pyATS Library
    * actually several libraries
    * communicates with devices, does config parsing etc.
    * modules for Cisco and non-Cisco
    * takes YAML files for test input
* Use pyATS for 
    * taking snapshots of an entire network
        * e.g. before and after a change for comparison
        * not only snapshots config but also network state e.g. routing tables
    * pushing network changes as part of automation toolset


## 5.4 Describe the components and benefits of CI/CD pipeline in infrastructure automation


## 5.5 Describe principles of infrastructure as code

## 5.6 Describe the capabilities of automation tools such as Ansible, Puppet, Chef, and Cisco NSO

## 5.7 Identify the workflow being automated by a Python script that uses Cisco APIs including ACI, Meraki, Cisco DNA Center, or RESTCONF

## 5.8 Identify the workflow being automated by an Ansible playbook (management packages, user management related to services, basic service configuration, and start/stop)

## 5.9 Identify the workflow being automated by a bash script (such as file management, app install, user management, directory navigation)

## 5.10 Interpret the results of a RESTCONF or NETCONF query

## 5.11 Interpret basic YANG models

## 5.12 Interpret a unified diff

## 5.13 Describe the principles and benefits of a code review process

## 5.14 Interpret sequence diagram that includes API calls