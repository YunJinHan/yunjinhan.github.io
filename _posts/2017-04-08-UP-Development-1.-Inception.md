---
layout: post
title: "UP Development - 1.Inception"
date: 2017-04-08 +0900
tags: Object-Oriented-Programming
description: UP Development - 1.Inception
---

UP Development - 1.Inception
==========
Most Project require a short initial step in which the following kinds of questions are explored

1. What is the vision and business case for this project?
2. Feasible?
3. Buy and/or Build?
4. Rough estimate of cost
5. Should we proceed or stop?

### - Artifacts in Inception Phase
**Use-Case Model** : Describe functional requirements, and related non-functional requirements

### - Useful Classification
- Functional requirements vs Non-functional requirements
- Most of the non-functional requirements are collectively called as **Quality Attributes** or **Quality Requirements**
- Functional requirements are explored and recorded in the **Use-Case Model**.
- Other requirements can be recorded in the use cases they relate to, or in the **Supplementary Specifications** artifacts.
- The **Vision** artifact summarizes high-level requirements.
- The **Glossary** encompasses the concept of **Data dictionary** which records requirements related to data, such as validation rules, acceptable values, and so forth.

### - Motivation for Use Cases
- We need a tool to analyze, record, and improve functional requirements
- Developers must know exactly what they are going to implement
- A Client must understand what will be implemented

==> Solution : **Use Case**

### - Goals and Stories
- Clients and End users have **Goals (Needs)** and want the system to help meet them.
- Informally, Use Cases are **Stories of using a system to meet goals**
- The most effective way to capture the **Goals** and system's **Functional requirements** is writing **Use Cases**

### - Actors
- Basic Definition<br>An Actor is anything outside SuD that interacts with SuD.
- Extended Definition<br>An Actor is anything with behavior, including the SuD itself when it calls on the services of other actors
- In case of a person acotr, it represents a **role** that the person plays when interaction with these use cases.

### - Types of Actors
- **Primary Actors**<br>Goals 을 달성 하기 위해 System 을 이용함.
- **Supporting Acotrs**<br>Primary Actor 를 도와주기 위해 System 을 이용함.
- **Offstage Actors**<br>It has an interest in the behavior of the use case, but is not primary or supporting actors

### - Scenarios ( 하나의 특정 사례 )
- A **Scenario ( use case instance )** is a specific sequence of actions and interactions between actors and the SuD to meet a common goal
- It is one particular story of using a system, or one path through the use case.

### - Use Cases
- Informally, Use Case is a collection of related success and failure scenarios that describes actors using a system to support a goal
- **UP Definition** : Use Case is a set of use-case instances, where each instance is a sequence of actions that yields an **Observable Result of Value (Benefits)** to a particular actor
- Use Cases document the behavior ( Functional Requirements ) of the system **From the User's Point of View**
- A software product implements a set of use cases




