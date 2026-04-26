# Session 5

### Standard Model (Spring MVC with Tomcat, for example)
1. The server (Tomcat, Jetty, etc.) maintains a **thread pool** called the _request processing thread pool_ (in Tomcat it is the HTTP connector **Executor**).
    *   Each incoming HTTP request is picked up by a free thread from the pool.
    *   That thread executes the full flow: filter -> dispatcher servlet -> controller -> service -> repository -> response.
2. When the response is sent, the thread returns to the pool to handle another request.
3. The thread pool size is configurable (in Tomcat, for example, `server.tomcat.threads.max` in [`application.properties`](http://application.properties), and the default is usually 200).
    *   If all threads are busy and more requests arrive, they wait **in the queue** until a thread becomes available.
    *   If the queue fills up, **503 (Service Unavailable)** errors start appearing.
In other words: **each request = 1 thread from the pool**, until the response is sent.
* * *
### Blocking Calls (Traditional MVC)
If inside the Controller you call something blocking (for example, a database query or a synchronous REST call), the thread **stays busy waiting** and cannot serve other requests.
➡️ That is why high-scale systems can saturate quickly if the number of threads is smaller than the number of simultaneous requests.
* * *
### Spring WebFlux (Reactive)
If instead of Spring MVC you use **Spring WebFlux (with Netty or Undertow)**, the model is different:
*   It uses **event loop + non-blocking IO**, so a few threads can serve thousands of requests.
*   Instead of each request holding a thread, the request is "registered" and the thread is released while waiting for the response (for example, a database query or an external HTTP call).
*   This increases scalability, but it requires reactive programming (`Mono`, `Flux`, `subscribe`, etc.).
* * *
### Summary
*   **Spring MVC (Tomcat/Jetty)**: each request uses one thread from the pool -> simple model, but blocking.
*   **Spring WebFlux (Netty/Undertow)**: event-loop + reactive -> more efficient for high concurrency and non-blocking IO.
