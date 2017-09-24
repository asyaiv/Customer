# Customers System API #
Customers System - Interview challenge

Customers system was build as an interview process challenge. The system is an exemplar of the functions that are required in order to implement minimum viable product.

**PLEASE READ BEFORE THE FIRST RUN:**

To witness synchronization in action copy and save the output of **GET** immediately after the first run. In 3 minutes after the first load the system will perform synchronization during which new records will be added and customer custID = 666 address will update. 



#### Table of Contents ####

[System Requirements](https://github.com/asyaiv/Customer#system-requirements)

[Assessment Criteria](https://github.com/asyaiv/Customer#assessment-criteria)

[Implementation Details](https://github.com/asyaiv/Customer#implementation-details)

[Version 2 Considerations](https://github.com/asyaiv/Customer#version-2-considerations)


___
___
## System Requirements ##

#### Resource: ####
- Customers 

#### Resourse Properties: #### 
- First name
- Last name
- Address

#### Methods: ####
- List customers
- Create a new customer
- Update an existing customer
- Delete an existing customer

#### Project details: ####
- Input/Output in JSON format 
- RAML API specification
- Published on Github repository

#### Use-cases: ####
1. Periodic sync of customers using the API.
2. Mobile application retrieval and update to customer's details.
___
## Assessment Criteria ##

Two main deliverables will be assessed primarily on the following criteria. 

#### RAML: ####
The RAML deliverable will be assessed against the following design considerations
- Best practices for RESTful API design
- Use of builtin HTTP protocol features (methods, status codes, headers, query parameters)

#### MuleSoft Anypoint Studio:  ####
The MuleSoft deliverable will be assessed against the following implementation considerations:
- Good coding practices
- Consideration of error handling
- Inclusion of some unit tests 
- Use of DataWeave
___
## Implementation Details ##

The approach to developing the system was rather exploratory, some design decisions were driven by desire to learn and experiment rather than provide the most optimal production-ready solution. 

#### MySQL Database table ####
- **custID** - Unique Identifier (string)
- **firstName** - Fist Name (string)
- **lastName** - Last Name (string)
- **street** - Street number and street name (string)
- **city** - City name (string)
- **postCode** - Postal Code (string)
- **updated** - Last updated (autogenerated timestamp - string) 

#### 4 Methods ####
- **GET**: 
⋅ Retrieves all customers, returns a JSON collection
⋅ Retrieves a specific customer by CustID using *URI-PARAMETERS* 
- **POST** Takes a JSON to create a customer
- **PATCH** Updates customers address using *QUERY-PARAMETERS*
- **DELETE** Deletes a customer based on CustID passed via *QUERY-PARAMETERS*
 
##### Solution considerations: #####
The four methods use various input delivery mechanisms to demonstrate various use of HTTP parameters. However a better practice is a uniform strategy for data input. The strategy should consider system requirements and future use.While designing API it is recommended to declare media types, effectively use description fields and examples.   

#### Status Codes ####
- **200** - Successful request (OK)
- **201** - Record created
- **204** - Request completed, nothing to return
- **400** - Invalid request

##### Solution considerations: ##### 

The minimal set of HTTP codes has been used to demonstrate effective use of the protocol, however in a production system additional statuses are a must, including **404**, **500** (used in the Master System) and others. 

#### Synchronization ####
One way synchronization has been implemented using Anypoint Studio *POLL* and *HTTP Request component*.
In order to implement simplistic - one way synchronization with a master system a supporting system has been developed. 

The "Master System" has only **GET** method available and provides a list of customers in JSON format ([Master system output](http://mastersystem.cloudhub.io/api/clients))

The two systems have slight differences in the data models and the synchronization was achieved using **DataWeave** to transform the array of object into the format that is required by the database. Because the differences are minor an alternative to complete this would be using database query manipulations. 



##### Solution considerations: ##### 

Synchronization presented in the Customers system is overly simplistic and only aims to display minimal functionality. The records are updated in the client system from the master system and do not consider time stamps in this version. Usually enterprise solutions would employ more elegant mechanisms to ensure bi-directional sync, for example like watermarks and locks). However various implementations exist depending on requirements and other limitations.
___
### Future Release - Wish List ###

##### Error Handling #####

Currently Error Handling is limited and only implemented though HTTP status codes. In general an error handling strategy would employ at least two approaches: exception handling and input validation not only to improve user experience but to protect the system from malicious inputs. 

##### End-to-End Testing #####

Current version does not provide unit tests. A testing strategy needs to be developed and delivered.

##### Authorization #####

This version does not implement any form of authorization. In the future version a simple authorization could be implemented using OAuth2 for example using GitHub authorization.


