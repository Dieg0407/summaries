# References
* [Implementing Domain-Driven Design by Vaughn Vernon](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)

## Terms
This section will hold terms as well as some short definitions for them.

| Term                | Definition                                                                                          |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Ubiquitous Language | Applicable in a single Bounded Context, it is the shared terms that are used by the team composed of both domain experts and developers. |
| Bounded Context     | It's a boundary in which a domain model is valid.                                                    |
| Context Maps        | A tool to diagram the different Bounded Contexts in a system.                                         |
| Architecture        | In this context, the architecture is the style that surrounds and connects each BC.                  |
| Tactical Modeling   | A technique used to design the internal structure of a software system.                              |
| Entity              | An object that has a unique identity and is distinguishable from other objects.                      |
| Value object        | An object that represents a concept rather than a unique identity.                                   |
| Repository          | A mechanism for encapsulating storage, retrieval, and querying of domain objects.                   |
| Service             | A behavior that does not have a conceptual identity and is used to perform operations.               |
| Domain Events       | Indicate the occurrence of something inside the domain. An event is published, then it's consumed... |
| Modules             | Self-contained units of functionality that encapsulate a specific domain concept.                   |
| Strategic Modeling  | A technique used to define the high-level structure and relationships of a software system.          |
| Domain Model        | It's a software representation of the specific business the software is working for.                 |
| Anemic Model        | A model consisinting of only data without any logic |
| Core Domain | The most important domain in the business were the business core resides |
| Generic/Supporing Sub Domain | Domain models that support the core domain functionality, think of authentication for example |

## General rules

* The code should be the design.
* Each application service class should contain a single use case.
* A method should be in charge of a single use case. Ex: instead of a method save(), something like changeName(), 
modifyAge() would be preferred.
* There's one Ubiquitous Language per bounded context.
* Design tests against the domain model, if it can be done emulating it's consumers, then better.

## Chapter 1: Getting started with DDD

This chapter talks about the benefits of DDD at a high level. Some of those are:

* Create a software model that works by imitating the business.
* Increase the shared knowledge across all the members (IT, management, operations, etc.).
* Discover knowledge gaps that the business may have.
* Both the business and tech people will use the same language and concepts.
* Knowledge will be centralized, there should not be knowledge silos.
* DDD can help define inter-team organization relations, this is done by defining clear boundaries. 
**This is known as strategic design.**
* DDD can also help with more technical requirements by making code highly testable, scalable, and allowing
for distributed computing.

It's important to note that using DDD can be overkill in some situations. **DDD is meant to be primarily used in
the most important areas of the business**.

**DDD should be used to simplify, not to complicate.** Use the next table as guidance on whether or not you
want to use DDD for your use case.

| If your project | Should you use it? |
|----------|------------|
| Is fairly data-centric, ex: a simple CRUD operation | There's no need |
| The system is still small, less than 30 operations may be needed and logic is minimal | Still small, not necessary |
| You have more than 30 user stories, flows are more complex, and CRUDs are not simple | Beware, you may need to start using DDD |
| There's a degree of uncertainty in how the application will grow | Use it, devs are known for underestimating complexity | 
| Changes are often expected in the system | Use it, DDD has a set of practices that allow for quick iterations and changes |
| You don't know anything about the domain, nor does your team | Use it, it will help both software development and increase the understanding of the business | 

**Beware of anemic models** Anemic models is model that contains only data and lack behavior or logic.
In other words, they are essentially just containers for data without any meaningful operations or methods 
associated with them.

**The Ubiquitous Language have to be used in the domain to express concepts and business rules.** In order to create such language
you can do the following: 
1. Draw some pictures of the conceptual domain and label them. No need to use a formal modling language like UML.
2. Create a glossary of terms or some documentation that can help the terms and phrases to surface.
3. Review these terms with the rest of the team.
4. Ultimately the language should be expressed in the code, **the code is design and the design is code** so the codebase
should be the ultimate glossary of terms.

**DDD is not heavy.** Using it does not imply lots of ceremony and rigid practices. It values rapid iterations
over the code design. You can use TDD to insert a new value object into the domain core.