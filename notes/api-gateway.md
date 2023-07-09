If we've multiple microservices & client (UI) wants to create page then they've to know about all microservice.
Problem here is if we change/refactor any microservice then UI also has to change.

What if we can provide one abstraction layer on top of all microservices that communicates with microservices &
give results. This abstraction layer is generally known as edge microservice. This layer is also called api gateway.


how do you start using API gateway?
- list down all external apis
- build microservice that calls internal services (also called api composition)


benefits
- montitoring
- authentication

open source api gateway
- zuul by netflix
- nginx


disadvantage
- additional network hope
- it's little complecated







