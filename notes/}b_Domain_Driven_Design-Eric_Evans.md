---
title: "Domain Driven Design"
author: "Eric Evans"
tags: literature/todo
aliases:
  - "Domain Driven Design"
  - "DDD"
  - "ddd"
---

# Domain Driven Design

## Putting the Domain Model to Work
- What is a domain?
	- A [[domain]] is a subject area which is applied to a software program
	- Every software program relates to activities, workflows, interactions and interests of its users
	- A [[domain]] represents a model, which is a simplification of a real world or an imaginary problem. E.g. a map is representation, a model of the world that fulfills the requirements of its users
	- Examples:
		- A booking software is a [[domain]] that gets real people on an airplane (physical world)
		- A core banking system is a [[domain]] that handles the abstract money and finances for a bank (imaginary world)
		- A [[domain]] has normally nothing to do with software unless its a source-code control system like git

- A [[domain model]] is more then a diagram or a drawn abstraction, its all about the idea that could be conveyed by a diagram, by some lines of code, or by some words in a sentence – the medium doesn't matter

## Communication and the use of language
- When people communicate, e.g. speak, they don't use the language of the [[domain model]]
- Its a good way to refine your domain model by speech
	- Playing around with words and sentences is the most valuable tool to sharpen and concertize the [[ubiquitous language]]. Its as important as engaging your virtual/ spatial reasoning by drawing diagrams
	- Describe scenarios of the [[domain]] out loud within the borders of the model
	- Try to find an easier way describe the scenario each time you combine concepts of the [[domain]] without breaking the rules of the model
- The human brain seems to be specialized in dealing with complexity in spoken language
	- Book Ref: [[}b_The_Language_Instinct-Steven_Pinker]]
- By drawing a simple [[UML diagram]] with a handful of objects while discussing an issue, the participants are more focused
	- Diagrams are a true part of a discussion. They are easy changeable while discussing the model
	- Its easy to add some spoken words to the diagram and to discuss the change to the model or to revert it
	- Keep diagrams small and minimal, otherwise they fail in communicating or explaining the object model – they overwhelm the readers with detail and they lack a meaning 
	- Diagrams can show design constrains but not specifications in every detail
- Instead of annotating diagrams with a lot of text, you should add selective and simplified diagrams to your documents – the model is not the diagram

***

Author:
- [[@Eric_Evans]]
