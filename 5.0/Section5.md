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
* Presumably any question related to this section will have an exhibit to be interpreted.     
* Review your basic Python skills and the specific API sections to prepare.

## 5.8 Identify the workflow being automated by an Ansible playbook (management packages, user management related to services, basic service configuration, and start/stop)
* Presumably any question related to this section will have an exhibit to be interpreted.     
* Understand the files used by Ansible - inventory, playbook, config snippets. 


## 5.9 Identify the workflow being automated by a bash script (such as file management, app install, user management, directory navigation)
* Presumably any question related to this section will have an exhibit to be interpreted.     
* Review basics of bash scripting.    
* https://linuxconfig.org/bash-scripting-tutorial

## 5.10 Interpret the results of a RESTCONF or NETCONF query
* Reminders
    * RESTCONF is an HTTP-based protocol (443)
    * NETCONF is an SSH based protocol (830 or 22)
    * While NETCONF uses XML encoding only, RESTCONF supports both JSON and XML
    * Both use YANG Models
* Nice NETCONF examples at https://www.juniper.net/documentation/us/en/software/junos/netconf/topics/task/netconf-session-sample.html


## 5.11 Interpret basic YANG models
* Again we should just need to interpret an exhibit.
* Nice YANG decription at https://ultraconfig.com.au/blog/learn-yang-full-tutorial-for-beginners/

## 5.12 Interpret a unified diff
```
--- lao	2002-02-21 23:30:39.942229878 -0800
+++ tzu	2002-02-21 23:30:50.442260588 -0800
@@ -1,7 +1,6 @@
-The Way that can be told of is not the eternal Way;
-The name that can be named is not the eternal name.
 The Nameless is the origin of Heaven and Earth;
-The Named is the mother of all things.
+The named is the mother of all things.
+
 Therefore let there always be non-being,
   so we may see their subtlety,
 And let there always be being,
@@ -9,3 +8,6 @@
 The two are the same,
 But after they are produced,
   they have different names.
+They both may be called deep and profound.
+Deeper and more profound,
+The door of all subtleties!
```

* The header
```
--- from-file from-file-modification-time
+++ to-file to-file-modification-time
```

* The 'hunks'
```
@@ from-file-line-numbers to-file-line-numbers @@
 line-from-either-file
 line-from-either-file…
 ```

* ‘+’ A line was added here to the first file.
* ‘-’ A line was removed here from the first file. 

* When viewing side-by-side diffs:
    * white space  The corresponding lines are in common. 
    * ‘|’ The corresponding lines differ, and they are either both complete or both incomplete.
    * ‘<’ The files differ and only the first file contains the line.
    * ‘>’ The files differ and only the second file contains the line.
    * ‘(’ Only the first file contains the line, but the difference is ignored.
    * ‘)’ Only the second file contains the line, but the difference is ignored.
    * ‘\’ The corresponding lines differ, and only the first line is incomplete. (no newline)
    * ‘/’ The corresponding lines differ, and only the second line is incomplete.  (no newline)

## 5.13 Describe the principles and benefits of a code review process
* See things that unit tests cannot
* Team inclusion across more projects
* Improve quality of code, reduce bugs
* Keep a review checklist so that reviews are always consistent (and add to the checklist as required)
* Ensure that recommendations are implemented (committed) 

## 5.14 Interpret sequence diagram that includes API calls
* Take a diagram and break it down into ordered bullet points
* Example sequence diagram at https://tyk.io/api-design-methodologies/, compares against other diagram types