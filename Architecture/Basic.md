---
tags:
  - SOLID
  - CUPID
  - MoSCoW
  - WSJF
  - Kano
---

# #SOLID

**The mnemonic acronym SOLID incorporates a set of software principles of object-oriented design.** Originally introduced by Robert C. Martin in his 2000 paper _“Design Principles and Design Patterns”_, and later turned into an acronym by Michael Feathers, the ingredients of SOLID are:

- S﻿ingle responsibility principle
- O﻿pen-closed principle
- L﻿iskov substitution principle
- I﻿nterface segregation principle
- D﻿ependency inversion principle

# #CUPID

**CUPID is the creation of Dan North, originating as a criticism of the SOLID principles above.**
**CUPID principles have the same basic philosophy as SOLID: when coding, think about the people who will come after you.**

- Composable
- Unix
- Predictable
- Idiomatic
- Domain-based

# #GRASP

**GRASP stands for general responsibility assignment software patterns** and is a collection of, “_nine fundamental principles in object design and responsibility assignment_”, as described by Craig Larman in his book Applying UML and Patterns (1997).

- **Creator** – Who creates an object or a new instance of a class?
- **Information Expert** – What responsibilities can be assigned to an object?
- **Low Coupling** – How are objects connected to each other? How do you support low dependency, low change impact, and increased reuse?
- **Controller** – How are requests delegated from UI layer objects to domain layer objects, including coordinating the system operation?
- **High Cohesion** – How are the operations of elements functionally related? How do you keep objects focused, understandable, and manageable, (including supporting Low Coupling)?
- **Polymorphism** – How do you handle alternative elements based on type? How do you create pluggable software components?
- **Indirection** – Focused on avoiding a direct coupling between two or more elements through the use of intermediate units to handle inter-element communication, so as to avoid a direct connection.
- **Pure Fabrication** – Called a ‘service’ in [domain-driven design](https://www.boldare.com/blog/what-is-domain-driven-design/), this class does not represent anything from the problem domain but is created to ensure High Cohesion and Low Coupling are achieved.
- **Protected Variations** – Designing objects, subsystems, and systems so that variations in these elements does not impact on other elements?

# #Architectures

- Monolithic
  
  • Layered architecture
  
  • Pipeline architecture
  
  • Microkernel architecture

- Distributed
  • Service-based architecture

  • Event-driven architecture

  • Space-based architecture

  • Service-oriented architecture

  • Microservices architecture

# Prioritization Frameworks or Prioritization Techniques

## Moscow
MoSCoW is an acronym for:
- Must have
- Should have
- Could have
- Won’t have (this time)

## WSJF - Weighted Shortest Job First
WSJF is a quantitative prioritization framework commonly used in agile and SAFe® (Scaled Agile Framework) environments. It helps teams decide what to build next by maximizing economic benefit:

WSJF Score = Cost of Delay (CoD) / Job Size

## Kano Model
The Kano Model focuses on understanding customer satisfaction by classifying product features into three main categories:
- Basic needs: Expected by customers; their absence leads to dissatisfaction, but their presence does not increase satisfaction.
- Performance needs: The more of these features are present, the higher the customer satisfaction.
- Delighters (Exciters): Unexpected features that, when present, greatly increase satisfaction, but their absence does not cause dissatisfaction.


# #Link 

[What is SOLID, and why is it more than just an acronym?](https://www.boldare.com/blog/solid-cupid-grasp-principles-object-oriented-design/#what-is-solid,-and-why-is-it-more-than-just-an-acronym?)

# #Course #Learn

- [ ] [Developer to Architect](https://www.developertoarchitect.com/lessons/)
