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
* Components
    * Series of automated steps
    * IDE --> Build --> Test --> Deploy
    * CI (Continuous Integration) components
        * Source code (git)
        * Build servers (Jenkins, Travis CI, Drone CI)
    * Often can use a webhook to notify the build server about new commits
    * Docker images used for consistent test environment
        * Can use orchestration tools e.g. Kubernetes
    * Defined tests that the code must pass
        * Unit testing, smoke testing, code coverage, adherence to standards
        * Network tests could use a GNS3/VIRL test network
    * Separate staging and production environments

* Benefits
    * Fewer bugs
    * Automated processes
    * Faster turnaround times (release multiple times a day)
    * Frequent commits - more features
    * More stable

## 5.5 Describe principles of infrastructure as code
* Attempt by networking industry to bring the benefits of automation to networks (already baked in to newer infra like cloud)
    * Fewer handcrafted snowflake networks
    * Ability to roll out additional capacity dynamically and rapidly (elastic)
* Needs machine-readable code - preferablly in a git repository for version control and rollback
* Code for new infrastructure and changes can be reviewed by reading a text file 
* Code can be tested before going into production
* Desired state (declarative) approach - tell the system what the outcome should be, it can figure out the specific steps by itself
* Imperative approach - tell the system exactly what commands to run for each step

## 5.6 Describe the capabilities of automation tools such as Ansible, Puppet, Chef, and Cisco NSO
* These configuration management tools allow for a fleet of devices to be managed, using infrastructure as code
* Ansible is commonly used for networks due to good support for legacy devices. Puppet and Chef are more commonly used for server infrastructure as they require agents to be installed.
* All rely on a runbook/recipe/manifest which contains the instructions
* NSO - see section 3.2 for many details

## 5.7 Identify the workflow being automated by a Python script that uses Cisco APIs including ACI, Meraki, Cisco DNA Center, or RESTCONF
Presumably any question related to this section will have an exhibit to be interpreted.     
Review your basic Python skills and the specific API sections to prepare.

## 5.8 Identify the workflow being automated by an Ansible playbook (management packages, user management related to services, basic service configuration, and start/stop)
Presumably any question related to this section will have an exhibit to be interpreted.     
Understand the files used by Ansible - inventory, playbook, config snippets. 


## 5.9 Identify the workflow being automated by a bash script (such as file management, app install, user management, directory navigation)
Presumably any question related to this section will have an exhibit to be interpreted.     
Review basics of bash scripting.

## 5.10 Interpret the results of a RESTCONF or NETCONF query

## 5.11 Interpret basic YANG models

## 5.12 Interpret a unified diff

## 5.13 Describe the principles and benefits of a code review process

## 5.14 Interpret sequence diagram that includes API calls