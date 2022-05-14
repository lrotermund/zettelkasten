---
title: "Mastering Microservices 2022"
organizer: "heise Academy"
tags: conference/todo
---

# Mastering Microservices 2022
## Microservices, Monolithen - Hauptsache Module!
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
	- Quote from [[}b_Accelerate:_The_Science_of_Lean_Software_and_DevOps:_Building_and_Scaling_High_Performing_Technology_Organizations-Gene_Kim-Jez_Humble-Nicole_Forsgren|Accelerate: The Science of Lean Software and DevOps: Building and Scaling High Performing Technology Organizations]]
	- microservices = modules & loose coupling
	- microservices enable independent deployments and technology decisions for modules
		- microservices wont fix your organisation
		- You must trust your developers to make these decisions, otherwise, you have none of the advantages of microservices, only the disadvantages, such as complexity.

## Turmbau zu Babel in nachrichtenbasierten Systemen
Speaker: [[@Kristian_Kottke]]



## Micro Frontends – Entkopplung bis zur Oberfläche
Speaker: [[@Michael_Geers]]



## Tackling cross-cutting concerns within your architecture
Speaker: [[@Daniel_Kocot]]



## Aus der Rubrik "Spaß mit Microservices": Transaktionen
Speaker: [[@Lars_Röwekamp]]



## Debugging Distributed Systems
Speaker: [[@Bert_Jan_Schrijver]]



## Testing von Microservices und deren Zusammenspiel
Speaker: [[@Arne_Limburg]]


***

Conference Organiser:
- [[@heise_Academy]]