What is reactive?
- Not async programming
- It's mostly for user event. When user clicks on button code should react to that by registering function.

Why do we care? (For backend)
- Server side development
- Request comes in
- We do processing
- we return response
- it's not reactive. Someone waits till above steps are completed
- Modern appplication
- High data scale
- High usage scale
- cloud based scale
- How to scale up?
- vertical - hard limit
- horizontal - multiple servers
- Before scaling optimizing code is better solution (scaling unoptimized code is bad)
- reosurce utilization
- code optimization
- unnecessarily sequenctial
- Piling up waiting threads in servers
- We code like
- it's single request. same code could be handling multiple request same time.
- we write stateless method. state is trasferred to DB when we have in memory var. we run into concurrency issue.     
- Concurrency API
- Future & CompletableFuture
- problem
- too much code
- error handling messy
- it's still sync after all - why because controller/service needs to return concrete object to work it can't return future object
- Reactive Programming
- much simpler
- reusable flexible functions
- not for smaller project


Detour
- Java Streams
- code without loop -> java streams
- sequence of data
- main focus on computations
- vs collection which focus on storage (how to store, how to retrive)



Reactive Programming
- iterator pattern - decouples contaier & algo
- observer pattern - notifies observsers
- difference b/w above two is who has control of data
- reactive combines above two... code is not actively asking give me next data but it submits it's observer & when data is available observer will be called
- reactive can be sync. - reactive way of programming means you just telling data is available to the subcriber now wether it's sync or async it depends on subscriber.
- why it is so adaptive & why ppl are moving to this? - because you don't have to change how you code all idle thread handling is done by fwks.
- java 9 Flow Interface - doesn't provide impl
- projectreactor (impl)
-  flux - 0 or n items (publisher)
-  mono - 0 or 1 items (publisher)
-  backpressure - incoming event are too fast processing is too slow. you can tell incoming request hey slow down! (for flux only)
- what subscriber can expect
- An item
- Done event (complted event) - it's terminal event
- failure event - it's teminal event
- backpressure - way of communicating rate to publisher - we're not pulling data. Source ensuring rate of data at which it will publish.
- mostly in rest API we don't need to worry about backpressure because either we send object or no object that can be handled by mono
- how to handle error?
- unresponsive
- you can set timeout.
- how to get actual value from wrapped object?
- if we directly try to access value then it will be blocking. then what's the point?
- better way?
- go reactive all the way. so only caller (spring fwk) is blocking.
- restaurant analogy
- reactive web servers - netty.
- these web servers doesn't wait for mono to be completed (basically your http request is not waiting to be completed). these servers keep
mapping of request to mono & utilise idle threads. when mono is completed it sends response to client. From this we get scaling.  
- Operators (to operate on mono & flux without blocking) - helps trasform a stream in a non-blocking way
- filter
- map
- take - only take specified elements
- log - log details in stdout
- defaultIfEmpty
- flatMap
- distinct
- distincUntilChanged
- count
- collectList
- buffer
- Return values
- just like stream
- subscribe - turn on
- Error handling
- terminal event
- original sequence does not continues
- error lambda (just like catch)
- doOnError - just tell what to do but it will pass error to next step.
- onErrorContinue - do not terminate in case of error
- onErrorResume - replace original flux provide your own flux
- doFinally (== finally)


learn reactive spring boot using webflux
https://reflectoring.io/getting-started-with-spring-webflux/
