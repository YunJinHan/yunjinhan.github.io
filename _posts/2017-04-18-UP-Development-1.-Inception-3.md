---
layout: post
title: "UP Development - 1.Inception (Cont'd) 2"
date: 2017-04-18 +0900
tags: Object-Oriented-Programming
description: UP Development - 1.Inception (Cont'd) 2
share: true
comments: true
---

UP Development - 1.Inception (Cont'd)
=====
### - Main Success Scenario
- It describes typical success path that satisfies the interests of the stakeholders
- It records three kinds of actions
	- interaction between actors
	- validation
	- state change by the system
- Step one of UseCase normally indicates the trigger event that strats the UseCase
- **Defer all conditional and branching to the Extensions section** ( 모든 확장부분의 분기와 조건은 미룬다 )

### - Extensions ( or Alternative Flows )
- Extensions indicate all the other scenarios or branches, both success and failure
- Extension has two parts **Condition** and **The Handing**
- **Write the condition as something that can be detected by the system or an actor**
<pre>
Example )
Extensions:
3a. Invalid identifier:
&nbsp;&nbsp;1.System signals error and rejects entry.
3b. There are multiple of same item category and tracking unique item identity not important (e.g., 5 packages of veggie-burgers):
&nbsp;&nbsp;1.Cashier can enter item category identifier and the quantity. 
3-6a. Customer asks Cahier to remove an item from the purchase:
&nbsp;&nbsp;1.Cashier enters the item identifier for removal from the sale.
&nbsp;&nbsp;2.System displays updated running total.
</pre>

- At the end of extension handing, by default the scenario merges back with the main success scenario, unless the extension indicates otherwise ( such as by halting the system )
- If a particular extension is quite complex, a separate use case may be written
<pre>
Example )
7b. Paying by credit:
&nbsp;&nbsp;1.Customer enters their credit card information.
&nbsp;&nbsp;2.System requests payment validation from external Payment Authorization Serive system.
&nbsp;&nbsp;2a. System detects failure to collaborate with external system.
&nbsp;&nbsp;&nbsp;&nbsp;1.System signals error to Cashier.
&nbsp;&nbsp;&nbsp;&nbsp;2.Cashier asks Customer for alternate payment. 3. ...
*a. At any time, System crashes:
In order to support recovery and correct accounting, ensure all transaction sensative state and events can be recovered at any step in the scenario.
&nbsp;&nbsp;1.Cahier restarts the System, log in, and requests recovery of prior state.
&nbsp;&nbsp;2.System reconstructs prior state.
</pre>

![screenshot]({{ site.url }}/assets/scenarioLevel.png)

--> **논리적으로 계속해서 자기 자신을 하나의 Action Step 으로 가진 Use Case 를 만들수 있다.**


### - Special Requirement
- Non-functional requirements, quality attributes, and constraints which relates specifically to a use case are recorded with the use case
<pre>
Example )
Special Requirements:
-Touch screen UI on a large flat panel monitor. Text must be visible from 1 meter. 
-Credit authorization response within 30 seconds within 90% of time.
-Language internationalization on the text displayed.
-Pluggable business rules to be insertable at steps 2 and 6.
</pre>

### - Technology and Data Variations List
- This section records technical variations in how something must be done, but not what
- This section also records variations in data scheme
- This section should not contain multiple steps to express varying behavior for different cases. If that is necessary, say it in the Extensions section
<pre>
Example )
Technology and Data Variations List:
3a. Item identifier entered by laser scanner or keyboard.
3b. Item identifier may be any UPC, EAN, JAN, or SKU coding scheme.
7a. Credit account information entered by card reader or keyboard.
7b. Credit payment signature captured on paper receipt. But within two years, we pre-dict many customers will want digital signature capture.
</pre>

### - Key Four Concepts that apply to every sentence in UseCaese and to the UseCase as a whole
- **Level : Why do we want this goal? ( 어느 정도의 Goal Level 이냐 )**
- **Scope : Which system boundary do we mean? ( System 의 Boundary 가 어디까지냐 )**
- **Detail : Do we describe intent, or action detail?**
- **Primary Actor : Who has the goal?**

### - What is a useful level to express use cases for application requirements analysis? ( 어플리케이션 요구사항 분석을 UseCase로 표현하기 위해 어떤 레벨이 효과적인가? )
- Guideline : The **EBP(OR User Goals ) Use Case**
- **EBP : Elementary Business Processes**

### - Common Mistake with Use Cases
- Use Case is a relatively large end to end Description that typically indcludes many steps or transactions
- It is normally an individual step or activity in a process

### - Every Sentence at every level is a Goal. Use Cases are one sentence style repeated
![screenshot]({{ site.url }}/assets/goal.png)

**HOW : Goal Level 낮아짐**<br>**Why : Goal Level 높아짐**

### - Primary Actors and Goals at different Boundaries
![screenshot]({{ site.url }}/assets/primaryActors.png)

<br><br>

Basic Procedure
-----
1. **Choose the system boundary**
2. **Identify the primary actors**
3. **For each primary actor, identify their user goals**
4. **Define use cases that satisfy user goal; name them according to their goal**


#### Step 1 : Choosing the System Boundary
- Choosing the right system boundary helps to find what is outside, i.e., primary actors and supporting actors.

#### Step 2/3 : Finding Primary Actors and Goals
- What computers, subsystems and people will drive our system?
	- An actor is anything with behavior.
- What does each actor need our system to do?
	-  Each need shows up as a trigger to our system.
- A List of use cases, a sketch of the system
	- Short, fairly complete list of usable system function

##### Reminder Questions to Find Actors
Primary actors and supporting actors are always external to SuD. To find actors, look for things in the categories of people, other software, hardware devices, data stores, or networks

#####  Reminder Questions to Find Goals
- In the UP, a use case is always started by a primary actor. But, it is sometimes useful to initiate use cases from inside the system
- Actor-based
	- Identify the actors related to a system or organization
	- For each actor, identify the process they initiate or participate in.
- Event-based
	- Identify the external events that a system must respond to.
	- Relate the events to actors and use cases.

![screenshot]({{ site.url }}/assets/actorGoalList.png)

##### Ranking Use Cases
- Use cases need to be ranked, and high ranking use cases need to be tackled early
- Ranking criteria
	- **Significant impact on the core architectural design**
	- Risky, time-critical, or complex functions
	- Risky technology
	- Primary line of business process


#### Step 4 : Define Use Cases
- In general, define one EBP-level (or user goals) use case for each user goal

##### How to do it
1. For each Use Case : Write the simple case - **Goal Delivers**
	- The main success scenario, the happy day case. ( 각각의 UC 에 대해 정해야 함 )
		- Easiest to read and understand, Everything else is a complication on this.
	- Capture each actor’s intent and responsibility, from trigger to goal delivery
		- Say what information passes between them, Number each line<br><br>RESULT : **Readable description of system’s function**

2. Write failure conditions as extensions
	- Usually, each step can fail.
	- Note the failure condition separately, after the main
success scenario
	- RESULT : **list of alternate scenarios**

3. Follow the failure till it ends or rejoins
	- Recoverable extensions rejoin main course.
	- Non-recoverable extensions fail directly.
	- Each scenario goes from trigger to completion
	-  RESULT : **Complete use case**


### Essential VS Concrete Use Cases
- **Essential use cases are expanded use cases that are expressed in an ideal** ( 무엇을 할 것인가 )
- **Concrete use cases concretely describes the process in terms of its real current design** ( 무엇을 어떻게 할 것인가 )
- **처음 Request 분석 : Essential ( Reusable )**<br>**나중 점점 진행 : Concrete 으로 진행 ( non Reusable )**

<br><br>

Use Case Diagram
-----
- A use case diagram is a diagram that shows a set of **use cases** and **actors** and their **relationships**
- UML modeling elements
	- **Use Cases**
	- **Actors**
	- **Dependency, generalization, and association relationships**

![screenshot]({{ site.url }}/assets/UseCaseDiagram.png)


### Use Cases Within a Development Process
- **Analysis phase**
	- Write expanded essential use cases for those currently being tackled, if not already done
- **Design phase**
	- Write real use cases for those currently being tackled, if not already done


Use Cases - Why?
----
**User Actor 의 Goal 과 System Actor 의 Functional Requirement 를 파악하기 위해 Use Case 사용**

- To give concrete examples of what we are supposed to be implementing
- A basis for an agreement between the clients and the developers
- A tool to make vague requirements more concrete
- **A source for object analysis** ( Domain Analysis 의 Basic Step - Source 역할 )
- **A source for the functionality of the system**
- **A unit for iterative & incremental software development**
- **The first version of the user’s guide** ( Use Case == User's Guide )
- The basis of system testing
