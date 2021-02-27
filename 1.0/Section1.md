# 1.0 Software Development and Design

## 1.1 Compare data formats (XML, JSON, and YAML)
|Attribute                                  | XML                    | JSON          | YAML          |
|----                                       |----                    |----           |----           |
|Human Readable                         |Worst                   |Good           |Best           |
|Size (storage/bandwidth)                |Largest        |Smaller        |Smaller        |
|Parse-ability                      |Parser/Schema needed    |Native         |OK             |
|Backward compatibility             |Best             |OK             |Worst          |
|Additional comments                |No                  |No             |Yes (#)           |


## 1.2 Describe parsing of common data format (XML, JSON, and YAML) to Python data structures
#### XML using xmltodict
```
https://www.kite.com/python/docs/xmltodict.parse
>>> import xmltodict
>>> doc = xmltodict.parse("""
... <a prop="x">
...   <b>1</b>
...   <b>2</b>
... </a>
... """)
>>> doc['a']['@prop']
u'x'
>>> doc['a']['b']
[u'1', u'2']
```
#### XML using ElementTree
````
https://www.datacamp.com/community/tutorials/python-xml-elementtree
import xml.etree.ElementTree as ET

tree = ET.parse('movies.xml')
root = tree.getroot()

for movie in root.iter('movie'):
    print(movie.attrib)

````
#### JSON using json
```
https://www.w3schools.com/python/python_json.asp
import json

# some JSON:
x =  '{ "name":"John", "age":30, "city":"New York"}'

# parse x:
y = json.loads(x)

# the result is a Python dictionary:
print(y["age"])
```

#### YAML using pyyaml
note the pip installer is called 'pyYAML' but the imported module is 'yaml'    

myfile.yaml
```
---
access_token: hungryhungryhippos
expires_in: 12055933
refresh_token: someotherlongcomplexstring
refreshtokenexpires_in: 3435235

```


```
import yaml

yaml_file = open("myfile.yaml", "r")

pythondata = yaml.safe_load(yaml_file)

print(pythondata['expires_in'])
print("The access token from YAML is: %s" % pythondata['access_token'])

```


## 1.3 Describe the concepts of test-driven development
* Basically, define a test first then write code that will pass the test
* Python library 'unittest'
    * Run 'pythom -m unittest' and get nice pass/fail output
* Python library 'pytest'
    * Assert that if you put value X into a function, the function will return value Y every time
* Run the tests whenever changes are made
* Cisco's Python library 'pyACS' is something different, covered in later section, simulates a network for testing against
 
## 1.4 Compare software development methods (agile, lean, and waterfall)
#### Waterfall
* Old school
* Single, long development cycle with no opportunity for feedback until the very end
* Requirements may have changed by the time something is delivered
#### Agile
* Many, shorter cycles
* More opportunities to obtain feedback and refine requirements
* Better customer outcome
#### Lean
* Similar to agile, add in waste reduction at each review
* Better outcome for your employer as well as the customer


 
## 1.5 Explain the benefits of organizing code into methods / functions, classes, and modules
 
## 1.6 Identify the advantages of common design patterns (MVC and Observer)
 
## 1.7 Explain the advantages of version control
 
## 1.8 Utilize common version control operations with Git
 
### 1.8.a Clone
### 1.8.b Add/remove
### 1.8.c Commit
### 1.8.d Push / pull
### 1.8.e Branch
### 1.8.f Merge and handling conflicts
### 1.8.g diff