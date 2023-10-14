

### Life before microservices

- All components are part of a single unit.
- Everything is developed, deployed and scaled as 1 unit.
- App must be written with 1 tech stack
- Teams need to be careful to not affect's each other's work.
- 1 single artifact, so deploy the entire applicaiton on each update.
- Longer Time
- Bug in any module can down the app.

### What are microservices?

- split application into smaller, independent services.
- 1 service for 1 job
- self contained & independent
- each microservice has its own version

#### Doubts regarding this
- How to break app?
- How big/small it should be?
- What code goes where?
- How to they communicate?
- How many services do we create?

#### Split based on buisness functionalities

##### For Example -> Shopping app
- products
- user
- shopping-cart
- etc

### Communication of Microservices
#### 1. API Calls
![[Pasted image 20231014114223.png]]

#### 2. via Message Broker
![[Pasted image 20231014114308.png]]

#### 3. K8s (Most Popular Currently)
##### using Service Mesh
![[Pasted image 20231014114359.png]]

### Pros :
![[Pasted image 20231014114608.png]]

#### Cons :

1. Configure the communication b/w services
2. More difficult to monitor with multiple instances of each service distributed across servers.

-> To overcome all this -> K8s + Hashicorp(products) or CNCF prods

#### How to Manage CI/CD Pipeline for Microservices :

##### Monorepo : 
![[Pasted image 20231014120143.png]]
![[Pasted image 20231014120207.png]]
- All projects and teams are affected, if there is some issue.
- Google, Meta uses Monorepos

##### Polyrepo :
- Code is completely isolated.
- Clone and work on them seperately.
- ![[Pasted image 20231014120452.png]]
- Each pipeline for each repo
- ![[Pasted image 20231014120551.png]]
- 


## Task -> Large Microservices based Repos


