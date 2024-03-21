
### Before you begin...

* Your main tool in the field is a **good sense rule**, but there is no guide on how to achieve it exists.

* Every rule of the ruleset could be dropped/avoided/changed **situationally** according 
  to a **good sense rule**.

* Prefer good sense over formal process

# General Rules

1. Absolute Simplicity and Minimalism
    1. Less code
    2. Smaller and shallower CFG
    3. Smaller and shallower DFG
    4. Smaller dependency graph
    5. Smaller scopes
    6. Shorter lifetimes
    7. Smaller and shallower project tree
    8. Fewer concurrency / asynchronity / workers
    9.  Fewer entity classes/types
    10. Fewer entities
    11. Fewer modules
    12. Fewer layers
    13. Fewer services
    14. Fewer dependencies
2.  **SRP** (Single Responsibility Principle)
    1. Works in both directions
        1. Specific module is responsible for one and only one function
        2. Specific function is implemented by only one module
    2. Applies to all levels
       * From small internal modules to services
    3. SRP applied not only to executable/library modules and services
       but also to pure model definitions (ref. 'Data modeling')
3.  **Maximum decomposition level possible**
    1. If you can draw a dividing line, without sacrificing anything - draw it.
    2. But wrong and redundant decomposition is very harmful
4.  Maximum explicity and straightforwardness
    1. Your solutions should be implemented the most straightforward way
    2. (i.e. Dependency Injection treated strictly as an antipattern in this context)
5.  Strongest contract possible
    1. Maximum number of agreements/implications you make should be described in a form of 'non-executable code'.
    2. First applied to API, DAO and DTO
    3. (i.e. method signatures, class/interface/structure definitions, protobuf definitions, etc.)
6.  Dependency hygiene
    1. it's better to write few (even hundreds) lines of code to solve the problem
    than introduce a 3rd party solution, that you will need to learn and depend on, struggling from it’s limitations
    2. Introduce only high quality dependencies
7.  No impractical decisions
    1. If you decision leads to no positive practical effect - avoid it
8.  No blind best-practice following
    1.  Best-practice applied "just in case" or "just because" is a worst-practice.
9.  Beware of the **False Simplicity**
    1. Untyped languages
       * *they are not simpler, they are less specific, thus more complex*
    2. Schema less approaches of any kind
       * *also simply less specific, you always have schema, the question is: is it recorded/enforced or not?*
    3. CRUD methods
       * *loss of specificity*
    4. "Do fast then refactor" approach
       * *know how? do now!*
    5. Dependency Injection
       * *simply makes data/control flow less obvious, thus more complex*
    6. GraphQL
       * *gives you too much freedom; less freedom - more control; harder to optimize* 
10. **Systems developed according to these principles**
    1. Has better stability
    2. Has better maintainability
    3. Requires fewer engineering resources
    4. Gives enginners ability of faster onboarding
    5. Requires low test coverage to be stable
    6. Requires no debugger (and a debugging process at all) to find a problem
        * A mark of a high quality system:  
        You can determine a problem by looking only at a stacktrace and error description to know exact error and reason
    7. Easier to understand and maintain


# Some general notes

- Technologies should not be just comfortable to use for your team
    - It should be comfortable to use for your well-trained team
- Everyone should write code in similar style like others
    - Makes work efficient
    - Speeds up onboarding
- If something could be automated - it should be automated
- If something could be simplified - it should be simplified
    - support costs depends on quality and simplicity
- Dirty hacks during prototyping (do fast then refactor myth) - emerges because of laziness and lack of knowledge. Do it right from the beginning! (if it’s not a presentation mockup)
- Proposition that building a general purpose system is a more complex task is a myth,
  on a distant prospect, general purpose systems is easier to extend and maintain

# Reactive Systems Design (todo)

- All data is versioned
- The only way to obtain the data is via **subscription**
- Mutation returns full modified entity; with all modified dependencies
- Dependencies are not embedded in each other, but returned separately
- On a client side deps are not embedded, but resolved using store/getters
- Backend decides which data and when it provides to client
    - Backend manages data dependencies

# General Architecture

Architecture development precess has nothing in common with technology selection.
Building architecture means defining how things work. And only then you pick right technologies.

Good architecture is future-proof which means most of the improvements has
incremental nature (which means you only write new code with minimal refactoring)

- How to achieve it?
- The process: 
  1. Design
    - Understand the domain
    - Gather requirements
    - Imagine your system after 5 years in production
    - Initial achitecture documentation
  2. Model
    - Benchmarks for predicted corner cases?
    - DTO / API
    - DAO
    - Code Infrastructure
  3. Devlop
    - Tests
    - Implementation
- Architecture developed using deductive approach (top-down)
- No fragmentary changes
  - Fragmentary changes clutters architecture and it becomes a bunch-of-special-cases
- No "special cases"
  - Most changes we bring should be a part of a common well-designed concept.
    - Bad design formula: [work] = [new requirements]
    - Good design formula: [work] = [brand new system design with new requirements] - [current system]
- New requirements never implemented as-is
  - They are going through the architecture development process
  - Otherwise they will become a bunch-of-special-cases
- When designing something you should extend requirements to fit all future requirements but limit your imagination to real cases.
  - This means your job is to find the broadest solution with minimum complexity.
- TODO: Speculative engineering vs Previsionary
- Good architecture is superior to raw performance

2 modes:
1. Good design => Implementation
2. Quick prototype => Validation/testing => Good design => Implementation
   
- Choosing technologies
  - TODO
  - Prefer language/platform-native solutions if possible




## Decomposition, incapsulation and polymorphism

1. Decomposition
   1. **SRP!**
   2. No premature optimization
      1. Optimization could break decomposition, incapsulation and influence architecture
      2. So optimizations should be done on the ready made system
   3. All modules should be homogenuous
      * If module implements domain logic on top of specific database, nothing else here
      * Doing specific data computation? Nothing else here.
      * Doing other kind of computation? same
      * Implements FSM that describes a process? Only FSM impl is here. In a separate module.
    4. Complexity
       1. Complexity management - one of the main purpose of decomposition
       2. Should be isolated
       3. Divide your system in two parts:
          * The simple one
             * Familiar structure/patterns
          * And the complex one
             * Well document complex parts

2. Incapsulation
   1. This is simple
   2. Being properly decomposed, module hides it's complexity behind it's API
   3. If something should not be done, there should be no way to do it

3. Polymorphism
   1. Interface is a polymorphism tool, not an incapsulation tool
   2. No redundant polymorphism
      1. Adds extra layers
      2. Rises complexity
   3. Method overloading makes code less readable/understandable

4. Inheritance
   1. Better avoid it


# API

**Good API is...**

- Should be designed first!
- API is a main thing, being properly designed it will be future-proof. And then you can make changes to internal architecture without breaking compatibility.
- API is **data** layer! (not **presentation**)
    - Presentation logic should influence it as less as possible (until you design presentation management API)
- CRUD is an antipattern
    - Loss of specificity!
        - CRUD
            - Client already knows what it wants to do
            - Then it simplifies it to just Update
            - Then server tries to figure out what client wants (applies awful, fluent, unreadable and unstable heuristics)
        - No CRUD
            - Action call
            - Atomic update
    - No control over which data is actually changed
    - Facing either Data Races or forced MVCC
    - No audit log events or complex and unreliable diff algos
    - Change state ⇒ update ⇒ What we’ve changed here, uh???


# Data Modeling

**Good Data Model is...**

- You should achieve your goals using properly designed DAO, not by using rich query language
    - Good DAO ⇒ simpler queries ⇒ faster system
    - Shift focus from building awesome queries to building awesome data model ;)
- Entities should reflect real world objects
- Entities should be named accordingly to it’s predestination and names should be **subconsciously understandable**
- Similar entities should be merged into single entity if: (TODO)
    - Most of the data are similar
    - Non-similar data could be described using some pattern without sacrificing semantics/readability
    - Handling principle is similar (i.e.: most of methods are identical)
    - Structures are similar
    - Structures could be merged without sacrificing semantics/readability

* Model should be defined in a 'pure' way
    * means in a separate package with no attached methods, etc.
    * i.e.: classical DAO approach is contrary to this case
* Each (sub)domain should maintain it's own data model, and own it
    * Data model should not cross boundaries of it's (sub)domain
    * i.e.: service `messenger` contains `model` package where the domain storage model is defined
    * No other service can access this data directly
    * DAO should not be used as a DTO
* Never misuse data model
  * i.e.: DAO can not be used as DTO
* Reference entities only by ID

# DAL
- Implements only one thing: data access from specific database
- Could be split to modules
- Modules never imports each other
- Specific DAL module accesses only one database
- No polymorphic DALs if not strictly necessary
- 

## Data converters
* Maintain data converters in a separate package
* Keep it purely functional
* Make it not return errors (if possible)

# Data

- More data
    - All data collected should be ingressed
    - All the data ingressed should be saved
    - All the primary data should be saved unaltered
- **Tree data model** is strongly preferred over **Tabular data model** for business domain
    - Why we use XML/JSON/YAML in business domain and not CSV?
    - Human consciousness works with vertices and edges
- No duplicated data
    - Except for denormalization
    - Its fine to denormalize **read-only** data (SAFE), but be careful when making a decision to denormalize **mutable** data (DANGEROUS)


# Service / executable module design

- Routing should be FLAT and declared in entrypoint to be easily reachable and readable
- TODO: error handling / middleware

* Entry point should should reside in a separate library package
    * to be callable from anywhere (i.e. tests)
* Entry point should be as small as possible.
  * It’s only a glue for subordinate entities.
* All service-wide entities should be instantiated in the entry point and passed through the call stack
    * i.e.: logger, data access layer instance, etc
* Proper lifecycle management
    * At the very beginning of the entry point we should have a global Context object instantiated
        * Later this object should be passed through the call stack accompanying the control flow.
        * This object should be nested and passed to every HTTP/gRPC/etc handler call using specific middlewares or http.Server.BaseContext callback in case of HTTP server.
    * Every service’s entry point should handle SIGINT|SIGTERM|SIGKILL
        * When the signal is caught, it should cancel the global context to initiate graceful shutdown
* Graceful shutdown
    * The graceful shutdown should be implemented by every internal process.
    * During the graceful shutdown entry point should wait for them to finish (with timeout).
    * Graceful shutdown process should have a reasonable timeout, to not last forever.
    * System/data integrity should never rely on graceful shutdown
* Handlers
    * Handlers should be thin.
        *Most of the logic should be encapsulated into a subordinate entities.
    * Handlers should be bound to a structs (except some special cases)

# Library design

* When to decompose function into a separate library?
    * If it should be used in another service (even if service does not exists yet)
* Every entity should have a constructor defined as a package level function
    * If there is the only entity in a package, then the constructor function should be named New
    * If there are multiple entities in a package, then constructor functions should be named New%EntityName%

# Technical Debt

- Elimination starts from global deductive architecture reassessment
    - Only when you have the new reference architecture you can start debt elimination
- Remember - sometimes refactoring consumes more time then 80% rewrite

## Refactoring

- Must be contionuous process
- Must be never postponed

# Coding Practices

* Maximum function purity!
  * If method is not pure enough, mutated args goes first; method name must contain '_Mutate' suffix

* Module should have no global state
    * the only exception is tests, they may have DDL-like appearance
* Scopes should have as few symbols as possible (especially at package scope, especially public ones)
    * TODO: explanation
* Declarations should have the narrowest possible scope it could have
    * TODO: explanation

* Top level symbols (functions, structs/classes) should have exceptionally readable names. [-]
    * *No 'common', 'dry', 'kiss', 'pkg', 'types', 'lib', 'meta', etc packages*

* Variables naming
    * Smaller scope => Shorter name
    * No premature variable declaration
        * This widens its scope and lifetime
        * This hardens understanding of its scope and lifetime

* Project Structure
    * Basic/conventional/regulated entities
        * Almost every project contains limited amount of entity types:
            * entrypoint  
              top-level executable package
            * entrypoint package  
              package: 'app'
            * storage data model (DAO)  
              package: 'model'
            * api implementations  
              package: 'api'
            * workers  
              package: 'workers'
            * integrations  
              package: 'integrations'
        * Each conventional entity group should be represented by it's own package at the top level of the repo
        * Each subtype should have a file prefixed with a name  
          i.e.: workers/worker_session_ttl_handler.go and workers/consumer_user_events.go
    * Non-conventional elements --TODO--

## Error handling
* If you need more than 10 minutes to find exact error cause, then something is really bad with your code
* All errors should be handled
* All errors should be reported at the moment they emerge
* System should be designed the way it CAN'T fail silently
* Functions that asks OS to terminate the process should not be used
    * i.e.: Golang's 'os.Exit()' and 'log.Fatal()'.
    * These calls prevents application from calling deinitialisation logic.
* Errors should be typed

## Logging/tracing (TODO)

# Tests

- Developers do manual testing!
  - You developed a feature.
  - You are the author and know the edge cases.
  - Test it briefly in UI.
  - You may find
    - Errors
    - UX inconsistencies
    - Unexpected behaviour
    - etc...

- DDL - better
- BDD - more descriptive
- Tests as docs
- Types:
  - Reference
    - Simple test covering core scenarios used mainly to do "smoke" and demonstrate common usage patterns of a tested system
  - Positive
  - Negative
  - Regression
- No redundant "mocks"
  - When you test your system against mocks, you test mocks
  - If you can do integrated testing instead, do it

# Investigation

- Always start investigation from the place where the problem was detected

# Docs

- No redundant docs
  - Only onobvious things should be documented
- **Entrypoint document**
    - Services
    - Link to DAO model
    - Link to DTO model
    - Hacks list
    - Deployment requirements (envVars, affinity, dependencies, general requirements, etc...)
    - Patterns and it's implementations used projectwide (grouped by repository)
    - Key architecture concepts (streams, ...)
- Code as a docs first
- Only unobvious things should be documented

# Teamwork

- You are nothing without your team. Period.
- Report problems early!
  - Spent 2 days solving a problem alone? You're not a hero, you just wrong.
  - Spent 2 hours solving it with your mate? You are great!
- Discuss everything!
  - 

# Glossary

* **SRP**
    * Single Responsibility Principle
* **CFG**
    * Control Flow Graph
* **DFG**
    * Data Flow Graph
* **DAL**
    * Data Access Layer
* **DAO**
    * aka **Storage Model**
    * Data Access Object, a model used to describe data stored in DBMS.
    * It this guide's context, it's a simple definition, contrary classical DAO pattern implementation
* **DTO**
    * aka **Exchange Model**
    * Data Transfer Object, a plain model used to describe objects that travels via network APIs.
* **BDD**
    * Behaviour Driven Development. In this guide this term is applied only to BDD-styled tests.
* **TDD**
    * Test Driven Development
* **DDL**
    * Domain Definition Language
* CQRS
    * TODO
* **DDD**
    * Dildo Driven Development
* **HaBaDD**
    * HAppypath BAsed Development Disease
* DRY, KISS, SOLID, SOC, YAGNI, BDUF
    * TODODO
