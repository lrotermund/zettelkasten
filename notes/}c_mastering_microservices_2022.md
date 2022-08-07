---
title: "Mastering Microservices 2022"
organizer: "heise Academy"
tags: conference/todo
---

# Mastering Microservices 2022
## Microservices, Monoliths – It's About Modules!
Title: Microservices, Monolithen - Hauptsache Module!
Speaker: [[@Eberhard_Wolff]]

**Today, teams are the biggest challenge for software engineering**
- Not everybody in your team is able to understand everything
	- Within a big [[microservice architecture]] its nearly impossible to understand every [[microservice]]
	- There are natural individual information islands – and that's OK
- Depending on the organization these information islands are an intended result – the concept is called [[Information Hiding]]
	- At Netflix, there is an uncountable amount of microservices
	- Netflix is continuously shipping new features – so they can handle there architecture
 
**What does a typical monolith looks like?**
 - By [[monolith]] is meant a so-called [[deployment monolith]] – so the software is deployed within a single deployment
 - For many developers a [[monolith]] looks like an easy to handle, simple architecture
	 - Typical stats for a monolith deployment
		 - It takes a long time to compile (15 - 60 minutes)
		 - Long startup times up to 15 minutes
		 - Interwoven and integrated with other systems
		 - Really hard to test manual
	 - Considering the typical statistics and facts, they are often the opposite
 - Often more then 90% of a monolithic system is in a dependency cycle, which leads to reciprocal relationship
	 - It look like the modules are divided, but in the end everything depends on the entities 

**Effects of multiple daily deployments**
- Better time to market
- Less burn-out
- No or only little work on the weekend – e.g. for long deployments and manual testing
- The solution to achieve multiple daily deployments are microservices
	- Results in daily testing, release and development

**To many modules on a single architectural layer leads to problems**
- In large classes within a [[monolith]] it is not uncommon to find over 1000 functions
- Its the same when you run 1000 microservices on a single layer

**microservices inherently come with higher risk**
- High risk is equal to high potential
	- Similar to investments where higher risk leads to a higher profit
- Bad designed microservices result in a lot of problems – e.g. too coupled services/ cohesion
- Well designed microservices are easy to understand and simple to deploy

**Well designed microservices due to modules**
- Recommended paper from [[@David_Parnas]]: [information distribution aspects of design methodology – D.L. Parnas](https://cseweb.ucsd.edu/~wgg/CSE218/Parnas-IFIP71-information-distribution.PDF)
- Blog post from [[@Adrian_Colyer]] about the paper with comments from [[@David_Parnas]] about microservices: [Information distribution aspects of design methodology – Adrian Colyer](https://blog.acolyer.org/2016/10/17/information-distribution-aspects-of-design-methodology/)
- Modules as described in the paper
	- As long as a module serves its public interfaces, it can be changed without further ado
	- Data/ information provided by its interfaces will be used by other modules
	- Goal: [[Information Hiding]] – the module should hide as much information as possible
		- Less information leaks to other module, which leads to an easier changeable module
		- Classes with instance variables and public methods are an easy, well-known and fine granular example for [[Information Hiding]]
		- In the early 90th the [[class-responsibility-collaboration]]/ CRC model was developed to represent the classes responsibilities
		- The [[Bounded Context]] is a coarse granular concept from [[}b_Domain_Driven_Design-Eric_Evans|Domain Driven Design]] to represent the infrastructure/ system boundaries
		- The [[Bounded Context]] comes with the [[Bounded Context Canves]], a similar tool to the CRC, developed by the [[@DDD_Crew]]
		- And the [[Bounded Context Canves]] is responsible for microservices
		- Important: The [[Bounded Context Canves]] and the CRC are function-driven and not data-driven.
			- You shouldn't build a system/ module based data, e.g. orders, users, etc.
			- You should build your system/ module "function-driven", e.g. order service, registration service, etc.
		- [[Information Hiding]] is also important for your data storage and messaging
			- Sharing data via your database (schema) or your events is the same as exposing your private class variables
- [[Bounded Context]]
	- A [[Bounded Context]] bounds a model (code) to an [[ubiquitous language]]
	- Its used to structure/ group functionality, e.g.'s
		- invoicing process => invoicing & VAT
		- shipping => tracking & delivery
		- order process => shopping cart & accept orders
	- Every [[Bounded Context]] has its own internal data models
		- A billing address is not the same as a shipping address
		- A change on data (e.g. the user's address) must not be shared with all modules
		- The design decisions are hidden, e.g. data storage
		- Important: [[Bounded Context]]'s are naturally good modules – microservices need to be good modules

**microservices - productivity**
- Expectation: microservices increase/ improve productivity
- What does increase/ improve productivity?
	- Frequent deployments due to small deployments with microservices
- Do microservices matter?
	> What is important is enabling teams to make changes to their products or services without depending on other teams or systems. 
	> - Quote from [[}b_Accelerate:_The_Science_of_Lean_Software_and_DevOps:_Building_and_Scaling_High_Performing_Technology_Organizations-Gene_Kim-Jez_Humble-Nicole_Forsgren|Accelerate: The Science of Lean Software and DevOps: Building and Scaling High Performing Technology Organizations]]
	- microservices = modules & loose coupling
	- microservices enable independent deployments and technology decisions for modules
		- microservices wont fix your organisation
		- You must trust your developers to make these decisions, otherwise, you have none of the advantages of microservices, only the disadvantages, such as complexity.

## Tower of Babel in message-based systems
Title: Turmbau zu Babel in nachrichtenbasierten Systemen
Speaker: [[@Kristian_Kottke]]

**Current project**
- Multiple teams organised around multiple e-commerce [[domain]] services ([[Bounded Context]]'s)
- Domain internal communication [[REST]] (backend and frontend component)
- External communication Kafka (or other [[Message Broker]] systems like Pub/Sub or RabbitMQ) 
	- At the start of the project, Kafka messages were submitted in JSON format
	- Model changes, incl. breaking changes, lead to problems (e.g. string type to array)  

**REST vs. Message Broker**

| REST | Message Broker |
| ----- | ------------- |
| Synchronous | Asynchronous |
| Client-Driven (Req - Resp) | Producer-Driven (event message) |
| Specific message version on Request | Multiple message versions (e.g. old messages in a legacy version) |
| Backward compatibility/ breaking changes => API versioning | Multi version compatibility? |
|  | Future consumer? |
|  | Offset reset? (read all messages again) |

- [[REST]] makes it easier to deal with changes
	- Fixed API versioning
	- Only backward compatibility
	- We don't care about future changes
- With [[Message Broker]]'s, the whole thing is a bit more complicated
	- The producer dispatches an async message within its current version for a consumer in the future (maybe it tooks 2ms to read the message, or it tooks 2 weeks)
	- How should new consumers deal with message formats?
		- Should they just consider the current format (e.g. n+2) or should they also consider old formats (e.g. n & n+1)?
- **Solution requirements**
	- Contracts about message formats (and there syntax) incl. [[Contract Enforcement]]
	- The support for [[Format Evolution]]
		- Compatibility
			- Prevent breaking changes on the format
			- Limit the impact of format changes
	- All this leads to
		- Consumer safety
		- Stability
		- Decoupled producers and consumers – avoid implicit coupling due to the message structure (visible on the basis of simultaneous deployments)

**[[Consumer-Driven Contract]] – CDC**

- via [Pact](https://docs.pact.io/))
	- The consumer defines and publishes contracts to the [[Contract Broker]]
		- The consumer can implement and test its logic against its own contract
	- The producer can test its logic against the consumer contracts
	- In the case of a REST API:
		- You can test message formats or a sequence of messages/ protocols
	- In the case of an async communication with a [[Message Broker]]:
		- You can use the module Message Pact to test *only* the message format
		- Its not intuitive to use an async, producer-driven communication like a [[Message Broker]] for a consumer-driven communication
			- Its **maybe OK** for event messages, e.g. `order submitted`
			- Its **not OK** for command messages, e.g. `submit order`
				- The CDC implementation via message broker feels weird here and is also a bit tricky, because the producer of a message is the consumer of a functionality
- via [Avro](https://avro.apache.org/)
	- Schema-based serialization system (with JSON) => contract
	- Binary data format
	- With a compact encoding and without field information
	- Ships with an optional code generation based on a defined schema
		- However, the generic message wrapper (generic record) can also be used
	- Scheme evolution ([[Format Evolution]]) is an integral functionality
	- Schema Registry
		- Central schema store
		- Enforce compatibility level
			- `Backward`
				- Schema n can read data n-1
				- Changes allowed:
					- Delete fields
					- Add optional fields
					- Change mandatory fields into optional fields
			- `Forward`
				- Schema n can read data n+1
				- Changes allowed:
					- Add fields
					- Delete optional fields
					- Convert optional field into mandatory field
			- `Full`
				- `Backward` + `Forward`
				- Changes allowed:
					- Add optional fields
					- Delete optional fields
					- Convert mandatory field into optional field
					- Convert optional field into mandatory field
			- `*_Transitive`
				- Checks all previous schema versions against the new schema
				- It can be attached to all the previous compatibility levels, e.g. full_transitive
			- `None`
				- Changes allowed:
					- All changes are accepted
	- Consumers build workflow
		1. Generate code based on schema registry
		2. Compile
		3. Test
		4. Build artifacts
	- Workflow runtime
		1. Producer wants to send a message
			1. Send schema to the registry
			2. When the schema includes compatibility level breaking updates, the schema is denied
			3. When the schema is valid the registry returns an id + the data
		2. Consumer wants to receive/ read a message
			1. Checks schema id in local cache
			2. When the id is not cached, then the consumer gets the schema by its id from the registry
			3. Then the schema json is transformed into a generic avro record
			4. The generic avro record can now be mapped into a generated DTO
				- This step can fail to a compatibility mismatch to the schema
	- Tooling
		- Registry
			- Confluent
			- Karaspace
			- Apicurio
		- Library
			- Confluent
			- Apicurio

## Micro Frontends - Decoupling down to the User Interface
Title: Micro Frontends – Entkopplung bis zur Oberfläche
Speaker: [[@Michael_Geers]]
	- [micro-frontends.org](micro-frontends.org)
	- [[}b_Micro_Frontends_IN_ACTION-Michael_Geers]]

- Frontends are usually monoliths
- Not only backends can be modularized via microservices, the same works for frontends
- What is it and why do you do it?
	- Software architecture today
		1. The monolith
			- Includes the frontend, backend and the database
		2. Frontend & Backend
			- Separate frontend and backend (monolithic frontend and monolithic backend)
		3. Microservices
			- Monolithic frontend
			- Aggregation Layer/ Gateway
			- Backend Microservices
	- Definition
		- Websites
			- [Thoughtworks – micro frontends](https://www.thoughtworks.com/radar/techniques/micro-frontends)
			- [Martin Fowler – micro frontends](https://martinfowler.com/articles/micro-frontends.html)
		- Split the browser-based code into micro frontends
		- Each feature is owned, frontend to backend, by a different team
		- Developed, tested and deployed independently
	- Example e-commerce article detail page
		- Team Checkout owns...
			- The basket fragment
			- And the buy button fragment
		- Team Decide owns...
			- The article detail page including...
				- The product
				- The variants
		- Team Inspire owns...
			- The related products fragment
		- The teams are build around [[DDD]]s [[Bounded Context]]
	- Independent systems
		- Cross-functional teams
			- UX/Design
			- Frontend
			- Backend
			- Data Science
			- Operations
			- Product Owner
		- End-To-End responsibility
			- From the database to the UI
		- Self-contained systems
			- Separate systems that function independently of each other
	- Why – what are the advantages?
		- Decoupling, technical and expertise
			- Leads to a faster development and a better time to market
			- Three 8 person teams are more effective then one 24 person team
			- Specialised teams (e.g. backend, frontend, product management, operations...) waist time in meetings for new features
	- Disadvantages
		- Antipattern: micro frontend anarchy
			- Agree on a shared tech stack
			- Prepare starter kits and templates for new teams
- How do you do it?
	- Every page has an owner
		- Responsibilities/ ownerships are assigned along the customer journey, e.g.:
			- Team Inspire
				- home
				- list
			- Team Decide
				- detail
			- Team Checkout
				- basket
				- payment
				- confirm
		- Teams provide features as pages or fragments
		- Integration techniques/ challenges
			- Composition
				- techniques
					- iframe
					- client-side components, e.g. Web Components
					- server-side integration, e.g. SSI, Tailor, Podium
					- AJAX
				- The spotify web player & desktop app were build with iframes until 2019
				- Composition on the server with ESI or SSI
				- Composition within the browser 
					- ... with custom elements (web standard)
						- Build your own html elements with a custom rendering logic
						- Linked to a javascript implementation
					- ... with declarative shadow DOM (browser spec)
						- Combination between custom elements and server-side composition (hydration)
						- Faster rendering due to included markup within the valid but unknown custom elements
						- Not yet supported by all browsers
			- Routing & Page Transitions
				- techniques
					- links
					- client-routing, e.g. application shell
					- server-routing, e.g. frontend-proxy, nginx
				- Links are usually no problem
					- Contracts between teams for a global routing table
				- Links within a SPA
					- Linked SPAs are no problem (hard links)
						- easy to implement
						- low coupling
					- Unified SPAs
						- Special use cases, e.g. no hard links allowed
						- Soft links with app shell (infrastructure component)
							- best user experience
							- Entry point for users
							- Can load, start and stopp SPAs
							- Switch between SPAs
							- Popular framework single-spa
- Practical example
	- Klingel Gruppe (e-commerce)
		- happy-size.de
- 4 tips and tricks
	- Communication patterns
		- URLs -> just links
		- Attributes -> pass data from page to fragments, e.g. sku attribute on the related products fragment
		- Events -> custom events or broadcast channel API
	- Communication smells
		- Communication coordinadion between more then two systems
			- If event one is triggered within fragment A, then event two within fragment B two waits for an event three of fragment C to dispatch an event four back to fragement A via fragment B
		- Global state management
			- Multiple modules use the same data store
				- Coupling!
				- Do not share data across multiple domain modules
					- E.g. customer address ≠ delivery address ≠ invoice address
		- Complex payloads
			- Leads to complexity and coupling!
			- Modules should not share "big" objects
			- Share IDs instead/ ping modules e.g. event "basked item added" -> custom data handling
	- Uniform, shared design system
		- Pattern library
			- This is how element XYZ looks within this application
			- This is how element XYZ reacts within this application
			- Provide via **versioned** package
				- Shared code & coupling! -> trade off
				- Internal open source project
					- No actions required from other teams -> avoids blocking
				- E.g. via NPM
	- Redundancy minimization
		- Smaller tools lead to less redundancy
			- Big tools: angular, vuejs, react
			- Small tools: svelte, hyperapp, preact, lit-html, stencil
		- Solution for big-tool redundancy: shared library/ CDN
			- Non browser native solution: systemjs
			- Works vor multiple tool versions, e.g. vuejs V2 and vuejs V3
		- Another way is decentralised [module-federation](https://module-federation.github.io)
			- One module loads dependencies and other modules with the same dependency just link to the already loaded dependencies
			- Advantage: its decentralised, there is no need to simulate a CDN vor local testng, it just works local
	- Micro frontends and native apps
		- How to handle micro frontends?
			- native:
				- Just do it for the web and ignore the app
					- Full redundancy
					- Big tech stack across teams
				- Download further code
					- A lot redundancy
					- Big, shared tech stack across teams
			- hybride (native shell & webview)
				- Same logic as in the web app
					- Less redundancy
					- Simple tech stack
					- Small native apps -> only frames and bundling
		- Tooling:
			- native integrated (download further code solution)
				- Alibaba: Atlas (Android) & BeeHive (iOS)
				- React Native
			- hybrid
				- React Native
				- ionic Portals

## Tackling cross-cutting concerns within your architecture
Title: Tackling cross-cutting concerns within your architecture
Speaker: [[@Daniel_Kocot]]

- Integration of requirements that are implemented for distributed/ many services across an entire system, e.g.:
	- Logging
	- Tracing
	- Error handling
- [[Cross-cutting concerns]] can be faced with
	- [[Fascade]]s ([[Gang of Four]])
	- An [[API Gateway]] ([[@Chris_Richardson]])
		- Hide all invloved microservices for a call against an API
		- It can be a proxy or its fanning out the request to multiple services
- Infrastructure
	- We need an orchestration for
		- provisioning and deployment of our containers
		- ressource management
		- health monitoring
		- scaling
		- providing mappings to connect to networkings
		- load balancing
	- A way to handle the orchestration is [[Kubernetes]]
	- There are a lot of alternatives to [[Kubernetes]], e.g.
		- AWS Fargate
		- AWS Elastic Container Service/ ECS
		- Azure Container Instances
		- Google Cloud Run
		- Openshift Container Platform
		- Rancher
		- Docker Swarm
		- Nomad
		- Helios
		- Apache Mesos
	- The [[API Gateway]] solution is implemented as an [[Ingress Controller]] within [[Kubernetes]]
		- How does a request looks like within
			- An [[API Gateway]]
				1. The client sends a request against the gateway
				2. The gateway evaluates a set of routing rules
				3. And forwards the request to one or multiple services
			- An [[Ingress Controller]]
				1. The client sends a request against the ingress within a [[Kubernetes]] cluster
				2. The ingress evaluates a set of routing rules
				3. And forwards the request to one or multiple services
				4. Where each of the services forwards the request to one or multiple pods
					- A pod is a set of containers that must run as a unit to function
	- [[Kubernetes]] supports and maintains multiple [[Ingress Controller]]s out of the box
		- AWS
		- GCE
		- nginx
	- The [[Ingress Controller]]s are based on service proxies
		- A [[Service Proxy]] is a network component between the client and the service, it can...
			- add or remove HTTP headers
			- terminate or offload SSL requests
			- Perform URL filtering and content switching
			- Provide content caching
			- Support blue-green deployments and canary testing
		- Service proxies can be
			- Envoy
			- OpenResty
			- HAProxy
	- Every [[API Gateway]] provider brings its owns [[Ingress Controller]]
		- The setup of an [[API Gateway]] within a docker compose environment is comparable/ transferable to the providers [[Ingress Controller]] within an [[Kubernetes]] cluster
	- Ingress / Egress
		- Ingress gateways deal with entering traffic
		- Egress gateways control the exiting traffic
- Cross-cutting concerns context
	- [[API Gateway]]s are usually extendable with plugins, policies or addons
	- Extendable contexts
		- Consumer
		- Route
		- Service
		- Load balancing
-  Cross-cutting concerns in detail
	- Traffic management
		- Access control list – handle them via [[Ingress Controller]]
		- Rate limiting
			- E.g. for services and consumers – consumer X is only allowed to send one request per hour to service Y
	- Security
		- Authorization and authentication
			- Centralized
			- Or additional (e.g. tenant specific)
	- Resilience
		- Circuit breaker (within traffic management/ on a network level)
			- A circuit breaker protects the cluster from stacking requests to unhealthy services, resulting in cascating failures
			- Provide a fallback mechanism when a circuit is opens due to an unhealthy service
		- Timeouts
			- Centralized timeout management
- But how to govern [[Cross-cutting concerns]]?
	- Avoid a wild west of cross-cutting concern tooling within the market place
	- Governance is important/ required
	- Use of OpenAPI extensions/ X-Objects to handle own or vendor needs (x-vendor-..., x-...)
		- Supported by
			- root level
			- info
			- paths
			- operation parameters
			- responses
			- tags
			- security schemas
	- Or use OpenAPI extensions for the configuration of the [[Cross-cutting concerns]]
		- Its a first step that is close to an API definition
	- Establish a [[Cross-cutting concerns]] design library
		- E.g. define, document and standartize the rate limits
		- By using references within the OpenAPI definition
		- Provide referenced defintions with the depoyment of the API
		- But: not every definition should be defined globally
		- Setup API guidelines and make the design library part of it
- Service Mesh
	- An [[API Gateway]]s and [[Ingress Controller]]s are responsible for incoming and outgoing traffic
	- And a [[Service Mesh]] is responsible for the cluster internal traffic
		- Within a cluster there are multiple services
		- And each service consists of a container and a proxy
		- Contrary to the [[API Gateway]], the services are not centralized
			- But every service have a sidecar proxy
		- To determine the differences with the [[API Gateway]], the use cases must be considered
			- [[API Gateway]] / [[Ingress Controller]]
				- APIs as a product (with or without monetization)
				- Layer 7 service communication with security and monitoring across different API products
				- Offer developers complete lifecycle management of the API
				- Transformation of communication protocols (e.g. json -> yaml)
			- [[Service Mesh]]
				- Layer 4/ Layer 7 service communication with security and monitoring within the same API product
				- Secure communication between services (we dont leave the cluster)
- Combining [[API Gateway]]/ [[Ingress Controller]] and [[Service Mesh]]
	1. [[API Gateway]] layer with defined API services
	2. Composite/ integration microservices layer
		- Grouped business logic and [[Service Mesh]] composites per microservice
	3. Core microservices layer
- Governing of [[Service Mesh]]
	- Relies on the team involved
	- Must be stated in developer guidlines
		- Define how the [[Service Mesh]] should or will be used
		- Write documentation and explain everything
	- Governance within [[Service Mesh]]s is not so easy
- [[Event Mesh]] – next big thing?
	- Soft facts
		- Idea of asynchronous [[Service Mesh]]
		- Arising from slow ongoing development of existing [[Service Mesh]]s
		- Heavily marketing driven by RedHAt, SAP and Solace
	- Hard facts
		- Cluster of event brokers
		- Diverse deployment environments
		- Protocol compatibility and translation
			- Depends strongly on the used product
		- Multiple client APIs
			- Each event broker per cluster -> API for clients
		- Classics
			- Scalability
			- Reliability
			- Security
	- But the biggest problem
		- [[Event Mesh]]s are lacking the concept of sidecars
		- [[Event Mesh]]s still need [[Service Mesh]]s to fulfill all needs
		- [[Service Mesh]]s still need to support asynchronous communication
			- E.g. by Envoy filters


## From the "Fun with Microservices" section: Transactions
Title: Aus der Rubrik "Spaß mit Microservices": Transaktionen
Speaker: [[@Lars_Röwekamp]]



## Debugging Distributed Systems
Title: Debugging Distributed Systems
Speaker: [[@Bert_Jan_Schrijver]]



## Testing of microservices and their interaction
Title: Testing von Microservices und deren Zusammenspiel
Speaker: [[@Arne_Limburg]]


***

Conference Organiser:
- [[@heise_Academy]]