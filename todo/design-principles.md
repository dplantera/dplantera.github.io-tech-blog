Rules
RULE 1	always implement business logic against types/schemas of the current service
Problem	
Rest or Kafka APIs change more often than the business logic. When a Rest API changes and it's types is used in the service for the business logic, then the business logic changes and hence the whole service.
We can not distinguish which code is actually unique code of the problem domain and which is just mapping chore between "design layers" like between transport (presentation) and domain (application) layers.

Solution	
For business logic always use at least the types of the service Rest API. 
Define one file/module "domain" where you import the Rest API types and only re-export those types the business/domain logic actually cares about.
Overwrite type references in the "domain" module if the domain evolves differently than it's API.
<todo: add ref to a mapping of current modules to the layers transport/domain. (transport: controller, clients, kafka). (domain: services, decorator, filter, permission checks, mapping to views like summary for the FE) 

Example	

RULE 2	define domain model/objects as types not schemas nor classes
Problem	
In the domain layer we want to see the business logic ( the logic which actually produces value).
We want static code analysis that can validate the correctness of the code.
We want to avoid duplicating types which are already defined/generated because then we need to change the domain model everytime the API changes even the type we only pass through and never touch in the service.

Solution	
#Rule 1 already states that we need a single module where we import/export domain types. In this module we define our domain model types through types and schemas we already have like Rest Types and Schemas.
We use common Typescript "magic" to access nested types from a parent and to manipulate them. We only write/copy the whole types for ourselves if we are lacking on knowledge and add a todo to highlight a rewrite when we know better how to express the type. 
We define the schemas for the domain through the Rest API schemas. If there are divergences we extend and overwrite them while reducing duplication as much as possible.  
<todo: ref the Rule where it states that we need runtime validation to make sure, that the data is shaped as the types>

Example	





RULE 3	define schemas (zod) only for "entities"  
Problem	
We need to make sure that only known fields are stored in the Database. If we store a new field in the DB and it is e.g. required in the API, then we need to provide default values in the Database. Hence, we maybe need to provide db-migrations
We need to detect needed db-migrations when changes occure. Changes in the schema need to be visible enough for us or for reviewer. 

Solution	
Autogenerate a schema (e.g. zod) from the service API and store it in a single file  "/domain/entities/entity-schemas.ts"
We re-export only the schemas, we want actually store in the Database in a respective file "domain/entities/some-entity.ts"
We maybe overwrite and redefine schemas in "domain/entities/some-entity.ts" if the service evolves in a different speed then the API 
If we update the API, we also update the "entity-schemas.ts" and decide then if the change needs an db-migration or if we handle it somehow differently e.g. overwriting/removing it in "some-entity.ts" because we do not want to store it but calculate it.

Example	





RULE 4	name modules not files or folders!
Problem	
https://dplantera.github.io/techblog/blog/2022/08/28/nodejs-a-reasonable-file-naming
We may split up a file into multiple files in a respective directory. If the file and the directory are named differently because of convention, then all references to that file need to be updated as well. 
We should be able to exchange a file module with a folder module without changing anything else in the project.
We do not think in files and folders, so we should not name them us such, we should name modules.
In NodeJs we work with modules and modules have the semantic feature that they could be exchanged. We should transport this feature when writing our projects.

Solution	
https://dplantera.github.io/techblog/blog/2022/08/28/nodejs-a-reasonable-file-naming
We name files and folders with the same naming convention

Example	





RULE  5	parse and validate requests and response of a service' Rest API and Kafka API
Problem	
In Typescript we do not have Types at runtime. We could use Classes but we choice not to do so. 
We need to make sure, that our Types at compile time match what we can excpect at runtime.
We need to make sure, that our Service only leaves data we defined in our API (Otherwise, we may leak information).

Solution	
Parse/Validate all data inbound into the service: Controller (kafka, rest), HttpClients (Rest responses of other services)

Example	





RULE 6	work in modules! you can use to describe the service to someone else
Problem	
There are many solutions how to architecture or design a service. All have somehow in common that they try to stick things together what belongs together according to the "Model" they describe.
Terminologies like Components, Layers or UseCase etc. all are nice Words but they most likey do not exists in our programming language. We have "modules" though.
Files are referenced across multiple folder (deep imports). 
It becomes unclear fast what the service actually does when everything is used everywhere and code relate to API mapping etc is placed in the same directory as service methods etc. 

Solution	
Put everthing related to API mapping in the same module with the code which calls the api (controller, client, kafka-consumer, kafka-producer) etc.   
Do not leak "Api" types inside the service. Rest or Kafka Api is mentally located at the boarder of the service. We should somehow define this boarders. (FP and PSS introduces Server and Integration as the boarders)  
Do not leak DB specifica like "_id" into the domain logic.
Service, domain, usecase, decorater, resultFilter and such are most likely business logic - those need to receive and return only domain types. 
