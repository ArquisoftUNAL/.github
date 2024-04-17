
## Hi!👋

**Welcome to Habitus Project**

<ul>
  <li>Habitus is a project for encouraging the power of habits among users 💪</li>
  <li>It is developed using microservices architecture 🌈</li>
</ul>

### Component and Connector Diagram
![alt](https://github.com/ArquisoftUNAL/readmeFiles/blob/main/C%26C%20Model.png)

Check out all the other diagrams (e.g Deployment view diagram and Data View Model) in the [Diagram and Documentation repository](https://github.com/ArquisoftUNAL/readmeFiles/tree/main).
### Quality attributes
#### Security

 - User Authentication
 - User Authorization
 - Zero trust
 - Limit exposure: Using private networks for the microservices, message queue and LDAP.
 - Data Encryption
 - Actions statistics: Using GCP one can have a registry of the actions users perform.

#### Interoperability

 - Interoperability between microservices using API Gateway orchestration and reverse proxies.
 - REST is used for communication between API Gateway and microservices. GraphQL is used between frontend and Gateway.
 - There's also a SOAP service for communication between the system and others that know the IP of the reverse proxy connected to that service.
 - Each microservice uses a specific communication tool for communicating with their asociated database.

#### Scalability

 - Microservices architecture
 - Implementation automation using Kubernetes.
 - Horizontal Scalability: Load balancing services.

#### Error handling
- Event based async communication between microservices.
- Testing: Load and stress testing done with Jmeter.


### Testing
Testing was done with Jmeter. An example of a test case is creating a large number of logins (which thus needs to access the `users_ms` microservice and the LDAP server). 
#### Scenario 1
Only one computing instance
| # Users| Response time (ms) | Throughput (trans/min) |
|------------|--------------------------|------------------------|
| 1          | 632                      | 36.8                   |
| 50         | 890                      | 1587.3                 |
| 100        | 1403                     | 2496.9                 |
| 105        | 1564                     | 2457.1                 |
| 110        | 4930                     | 1113.0                 |
| 114        | 16700                    | 383.1                  |
| 115        | 58959                    | 115.1                  |
| 125        | -                        | -                      |

The knee-point is obtained at 115 concurrent logins when the first error is identified. 

#### Scenario 1
Scaled to 3 replicas.
| # Users| Response time (ms) | Throughput (trans/min) |
|------------|--------------------------|------------------------|
| 1          | 541                      | 38.9                   |
| 50         | 889                      | 1588.1                 |
| 100        | 1280                     | 2631.6                 |
| 120        | 1523                     | 2853.7                 |
| 130        | 1730                     | 2857.1                 |
| 135        | 18470                    | 416.0                  |
| 136        | 104940                   | 77.0                   |
Here the knee-point is obtained at 136 concurrent logins where an error is identified. This replication technique improved the response time in 18.42%.
