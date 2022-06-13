[[_TOC_]]

# Prerequisite
* [Clean Architecture (aka Onion, Hexagonal, Ports-and-Adapters) ](https://www.youtube.com/watch?v=lkmvnjypENw&t=848s)
* [Onion architecture](https://laptrinhx.com/onion-architecture-4219738984/)
* [Microsoft eShopWeb sample](https://github.com/dotnet-architecture/eShopOnWeb/tree/main/src)

# Service architecture
* The Onion Architecture Approach was chosen to implement all the required use cases and make components scalable, maintainable, and testable.
* Organizes your code in a way that limits its dependencies on infrastructure concerns.
* The component is built using .NET 6 using the C# programming language. 
* The following [coding guidelines](https://dev.azure.com/vavacars/VavaCars/_git/ContactVerifierApi?version=GBmain&path=/src/.ruleset) are used.

# Solution structure
* Abstractions folder should contain interfaces mirroring implementations classes hierarchy 
* Group classes under the same folder when more than 2 classes with the same responsibility/group exist
* Try to avoid the suffix "Service" for all classes, use specific suffixes: client, provider, sender, publisher, uploader etc.. 

## Small microservice structure
```
|- Controllers
|- IntegrationMessageHandlers
|- Models
|- ValidationAttributes
|- ApplicationCore #when small microservice service
|-- Abstractions
|-- Common
|-- Domain
|-- Exceptions
|- Infrastructure  #Contains Asp and Persistence layer infrastructure
|-- Configuration
|-- Filters
|-- Database
|--- Migrations
|--- Repositories
|-- Services
|- HostStartup #WebHost configuration abstraction
|- Program
|- Startup
```

## Microservice structure
### API
```
|- Controllers
|- IntegrationMessageHandlers #Network depends command/events/ messages handlers
|- Grpc
|- Models
|- ValidationAttributes
|- Configuration
|-- Extensions
|--- ServiceCollectionExtensions.cs
|- Filters
|- HostStartup.cs #WebHost configuration abstraction
|- Program.cs
|- Startup.cs
```

### ApplicationCore
```
|- Abstractions
|- Configuration
|-- Extensions
|--- ServiceCollectionExtensions.cs
|-- Options
|--- AppOptions.cs
|- Common  #Common classes not related to specific entity, helpers, utils
|-- Exceptions
|- Domain
|-- Customers
|--- Validators
|--- Events
|--- ValueObjects
|--- Enums
|--- EventHandlers #internal domain events
|--- Specs 
|--- Customer.cs #Aggregate/Entity
|-- Orders
|- Services

```

### Infrastructure
```
|- Configuration
|-- Extensions
|--- ServiceCollectionExtensions.cs
|- Migrations
|- Repositories
|- Services
```
# Component Layers
## API Layer
name convention ProjectName.Api
* ASP NET Web API
* Network access to microservices
* Api contract/implementations
* External message handlers
* Queries abstractions (When using a CQS approach)
   * Micro ORMs like Dapper 
## ApplicationCore Layer
name convention ProjectName.ApplicationCore 
_NOTE: for small microservice can be part of API project, should be a move to independent project when growth_
* DDD patterns
  * Domain entity, aggregates, events
  * Aggregate root, value object
  * Domain services
  * Repository contracts/interfaces
* Validators
* Domain Exceptions
* External Services abstractions

## Infrastructure Layer
name convention ProjectName.Infrastructure 
* Data persistence infrastructure
  * Repository implementation
* Use of ORMs or data access
  * EF Core or any other ORM
  * ADO.NET
  * Any NoSQL database API
* Other infrastructure
  * Logging
  * Search Engine
  * Email, SMS providers
  * External Services implementation

## Tests Layer
* ProjectName.UnitTests
* ProjectName.IntegrationTests
* ProjectName.ComponentTests
* ProjectName.ConsumerContractTests
* ProjectName.ProducerContractTests
`
# Nuget suggestions
* ApplicationCore models can be used as API models if they one-to-one equals 
[Fluentvalidation](https://fluentvalidation.net/) can be used for validations, it will make ApplicationCore models clear from Data annotations
* We can use the guards pattern to validate models [GuardClauses](https://www.nuget.org/packages/Ardalis.GuardClauses/)
* To avoid primitive obsessions we can use [StronglyTypedId](https://www.nuget.org/packages/StronglyTypedId/1.0.0-beta03) for Entities Ids

# Alternative architectures

[Docs ddd-oriented-microservice](https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/ddd-oriented-microservice)

<IMG width="635px"  src="https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/media/ddd-oriented-microservice/domain-driven-design-microservice.png"  alt="Diagram showing the layers in a domain-driven design microservice."/>



 
