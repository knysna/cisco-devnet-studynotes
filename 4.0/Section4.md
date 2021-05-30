# 4.0 Application Deployment and Security

## 4.1 Describe benefits of edge computing
* Reduced latency
* Reduced amount of data to be sent upstream
* Good option for IoT
    * e.g. Process data locally from lots of sensors and send only the relevant summary upstream
* Good option for real-time applications
    * e.g. Factory machinery that needs to be controlled instantly for safety reasons
* Cisco likes to use the term "fog computing" to describe the layer above the edge but below the cloud

## 4.2 Identify attributes of different application deployment models (private cloud, public cloud, hybrid cloud, and edge)
Note that these are the Cisco definitions which could be slightly different to othe sources    

* NIST SP800-145 defines "what is cloud>"

* Private Cloud: a cloud model that provides self service and scalability but the organisation is responsible for every aspect of running the cloud infrastructure
* Public Cloud: a cloud model that provides self service and scalability, and the organisation pays a cloud provider based on usage
* Hybrid Cloud: a cloud model that has an application using some components of private cloud or on-premises infrastructure, and some componenents of public cloud
    * Not the same thing as having application A on-prem and application B in a public cloud
* Edge: As per previous section, the processing is near the sensors for reduced latency

## 4.3 Identify the attributes of these application deployment types

### 4.3.a Virtual machines
* Shared hardware between many machines
* Full operating system on each VM
### 4.3.b Bare metal
* Dedicated hardware and Operating System for each machine/application
### 4.3.c Containers
* Provide only what the application needs (shared base OS)
* Isolated
* Fast deployment
* Small file size

## 4.4 Describe components for a CI/CD pipeline in application deployments
* Source
    * Newly written code
    * Git/repository
* Build
    * Build server reacts to new commits in the repository
    * Compiles code/builds containers
    * Jenkins, Travis CI
* Test
    * Automated tests
        * Units tests, smoke tests, adherence to standards
* Deploy
    * Deploy new container to staging environment for user testing
    * Upon admin approval, deploy the new container to production environment

## 4.5 Construct a Python unit test
* Unit test: low level test of a single function in Python code
    * "unit" = "smallest possible piece"
* Create *assertions* that must be true if the function is working correctly
* Python default library: *import unittest*
    * Tests have classes and functions
    * A function can test multiple assertions
    * A class can include multiple test functions
```
import unittest

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()
```



## 4.6 Interpret contents of a Dockerfile
* Note keyword is 'interpret' not 'create'
* The first word in each line is an Instruction. This is followed by Arguments.
* FROM = the base image to build upon
    * Could be from a public image repository
* RUN = execute the command and include the result in the new build
* CMD = execute the command whenever the container is started
* Sample Dockerfile
```
FROM python
MAINTAINER Your Name and Email Address
WORKDIR /home
COPY ./sample-app.py / .
RUN pip install flask
CMD python /home/cool-app.py
EXPOSE 8080
```

## 4.7 Utilize Docker images in local developer environment

* 3 parts - client, host, registry
* client - CLI utility that communicates with the Docker host
* host - machine running the Docker deamon
    * local or remote from client
* registry - store of container images
    * can be private or public e.g. Docker Hub
### How to use 'docker' command
* *docker [OPTIONS] COMMAND*
* Get help on any command with *docker COMMAND --help*

### How to lauch/use a container
* *run* command = *create* + *start*
* *docker run -it ubuntu bash*
    * If being used for the first time, the Ubuntu container is downloaded
* Make container available (publish port): *-p 8088*
    * Re-map ports: *-p 80:8088*
* Run the container in detached mode *-d*
* Use a custom name instead of the random name: *--name custom-name*
* Mount a volume to the container using *-v file-path*



### Container management
* View running containers: *docker container ls*
* View running and stopper containers: *docker container ls -a*
* View images: *docker image ls* or *docker images*
* Connect to a running containder: *docker container attach [ID]*
* Exit a container but leave it running: *CTRL + P, Q*
* View details: *docker container inspet [ID]*
* Gracefully stop a container: *docker container stop [NAME]*
* Force stop a container: *docker container kill [NAME]*
* Delete a container: *docker container -rm [NAME]*
* Delete all stopped containers: *docker container prune*



### How to create a Docker image
* Build on top of an existing base container image
    * Optionally, select a specific version
* Use a Dockerfile to list all the actions and attributes (see 4.6)
    * Must be named 'Dockerfile'if a path won't be specified
    * Steps are executed in the order written
    * Execute commands directly e.g. *apt-get update*
    * Expose ports and mount volumes as required
* Create many containers from same Dockerfile
* Build: *docker build [OPTIONS] .*
    * the dot is very important, it tells the build that the Dockerfile is in the local directory!
* Push to Docker Hub (repo, account required)
    * Tag the image with username and repo name: *docker image tag [ID] username/reponame:tag*
    *  Push the image to the repo: *docker image push username/reponame:tag*
* Pull from repo: *docker image pull imagename*

## 4.8 Identify application security issues related to secret protection, encryption (storage and transport), and data handling


## 4.9 Explain how firewall, DNS, load balancers, and reverse proxy in application deployment

## 4.10 Describe top OWASP threats (such as XSS, SQL injections, and CSRF)

## 4.11 Utilize Bash commands (file management, directory navigation, and environmental variables)

## 4.12 Identify the principles of DevOps practices