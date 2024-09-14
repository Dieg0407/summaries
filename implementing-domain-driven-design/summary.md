# References
* [Implementing Domain-Driven Design by Vaughn Vernon](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)

## Terms
This section will hold terms as well as some short definitions for them.

| Term                | Definition                                                                                          |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Ubiquitous Language | Applicable in a single Bounded Context, it is the shared terms that are used by the team composed of both domain experts and developers. |
| Bounded Context     | It's a boundary in which a domain model is valid and *it's represented by specific software implementation for that model* |
| Context Maps        | A tool to diagram the different Bounded Contexts in a system.                                         |
| Architecture        | In this context, the architecture is the style that surrounds and connects each BC.                  |
| Tactical Modeling   | A technique used to design the internal structure of a software system.                              |
| Strategic Modeling  | A technique used to define the high-level structure and relationships of a software system.          |
| Domain Model        | It's a software representation of the specific business the software is working for.                 |
| Anemic Model        | A model consisinting of only data without any logic |
| Core Domain         | The most important domain in the business were the business core resides |
| Generic/Supporing Sub Domain | Domain models that support the core domain functionality, think of authentication for example |
| Problem Space       | Involves examining subdomains and depetermining the necesities to create a core domain |
| Solution Space      | Is one or more Bounded Contexts with specific software models |
| Context Maps | |
| Shared Kernel       | |
| Smart Anti UI Pattern | |
| Open Host Service | |
| Modules             | Self-contained units of functionality that encapsulate a specific domain concept.                   |
| Aggregates | |
| Entity              | An object that has a unique identity and is distinguishable from other objects.                      |
| Value object        | An object that represents a concept rather than a unique identity.                                   |
| Repository          | A mechanism for encapsulating storage, retrieval, and querying of domain objects.                   |
| Service             | A behavior that does not have a conceptual identity and is used to perform operations.               |
| Domain Events       | Indicate the occurrence of something inside the domain. An event is published, then it's consumed... |

## General rules

* The code should be the design.
* Each application service class should contain a single use case.
* A method should be in charge of a single use case. Ex: instead of a method save(), something like changeName(), 
modifyAge() would be preferred.
* There's one Ubiquitous Language per bounded context.
* Design tests against the domain model, if it can be done emulating it's consumers, then better.
* DDD objective is not to create a big domain model that maps the entire organization, instead we look for
multiple subdomains that added will represent the whole business.
* It's impossible to create a single domain that maps the whole organization.
* A bounded context should encompass all the subdomains where the terms are well understood, or in other words.
Each bounded context should have it's own ubiquitous language.
* A bounded context may fall withing more than one subdomain depending of the situation.
* Modularization is a good practice, but it doesn't fix the linguistic misalignement (Bounded context).
* There's no *correct* DDD model, there are good practices but it's the team who decides which terms are needed
and how they relate to each other (using submodules, contexts and ubiquitous language).
* It's desirable to align subdomains with bounded contexts. Though this is not always possible.
* Context is king. A concept will vary between bounded contexts.
* The concept may vary between contexts but the identifier should remain the same as it moves through the domains.
* A bounded context should be as big as it needs to be to express the ubiquitous language.
* Don't let architectural patterns define the bounded context.
* Just as with domains, ideally a team should handle a Bounded Context. This is so there's no concept leaking 
between contexts as if the team handles more than one, they may confuse the concepts on each context.

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

## Chapter 2: Domains, Subdomains and Bounded Context
`Domain` In this context represents what the organization does and the scenario in which such operations happen.
In general, there won't be a single domain that can represent the whole business, instead a proper representation
will be made of numerous subdomains that can be split into **core, supporting and/or generic.**.

For example, an e-commerce may have as sub domains **Product Catalog, Orders, Invoicing and Shipping**. In conjunction they represent
the `e-commerce` domain model. But they're (or at least should be) independent from each other.

While a subdomain represent a specific are of the business. A `Bounded Context` represents the limit in which the submodule is valid 
and the software components that represent such model. It can be thought of it as the proper software module. Ideally it should be able
to evolve independently from other bounded contexts attached to other subdomains.

Following the e-commerce example. Product Catalog, Orders, Invoicing and Shipping may all be tied down in a single *e-Commerce system*. In such example
there's no bounded context for the Product Catalog, but there's a bounded context for the e-commerce.
This coupling is the cause for problems in the software maintenance, as affecting pieces of the product sub module may end up affecting
the shipping as there's no clear and defined boundary between them.

**Bounded Context** are delimited by the language, for example, the term *Customer* may mean different things in the context of
an the Order submodule and the Product Catalog submodule. For the catalog we may care about the customer preferences, their loyalty, etc.
While on the order submodule we may only need the customer as the actor of a purchase. These differences are the ones that should 
be delimmitted with a Bounded Context. If done correctly, changes in contexts should be properly isolated.

**Core Domain** is the part of the business that is critical for the success of the organization. The core domain should be the one
in which the most DDD focus should be placed on. **Supporting subdomain** is one that is essential for the organization but it's not
core. If the subdomain is generic, like authentication, then it get's labeled as **generic subdomain**. Those last 2 subdomains do not
mean that they're unimportant, what it means is that the business doesn't need to excel on them.

**Strategic Design** in DDD focuses in the high level analysy by using a set of principles and practices focused on:
1. Identifying and defining the business Core Domains and Subdomains.
2. Establishing clear boundaries between different parts of the system (Bounded Contexts).
3. Developing a shared language (Ubiquitous Language) within each Bounded Context.
4. Mapping relationships between different Bounded Contexts and Teams (Context Mapping).
5. Aligning the software architecture with the business strategy and domain structure.

A domain have both a **solution space** and a **problem space**. A **solution space** analyses the subdomains and the core domain, this space
takes charge of determining those subdomains that exist and need to be created to support the core domain. The **problem space** contains the
respective bounded contexts, *a bounded context is represented by a specific solution*. Having a 1-1 relation betwee bounded context and subdomain
is desirable, although not always possible.

For the **problem space** here are some questions to better define it:
1. What is the name/vision for the strategic core domain?
2. What concepts should it include?
3. Which subdomains will give support to the core?
4. Who will work in each area of the domain? Can these teams be assembled?

For the **solution space** here are some questions:
1. What software assets do we have? Which ones do we need to aquire/build?
2. How are all these assets connected?
3. What new integrations will we need?
4. How much effort do the steps above would cost?
5. For each bounded context, which terms have different meanings? Where's the linguistic overlap between the contexts?
6. How are these overlapping concepts shared and transfered between contexts?
7. Which Bounded Context contains the concepts that address the core domain and which tactical patterns are we going to use?

**A bounded context** not only contains the domain, but also the system, an application or a business service. This means
that while it's primary focus is the domain model, it should also contain the means for the external world to interact with the 
model, to be able to translate use cases flows into domain operations. **Don't let architectural patterns affect the definition
of the context**, the boundary is defined by the Ubiquitous Language.