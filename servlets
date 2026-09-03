Java Servlet — Detailed Notes

A Servlet is a Java class that runs on a web/application server and handles client requests and generates responses.

The Servlet API is the foundation behind technologies such as Spring MVC, because Spring MVC ultimately runs on top of the Servlet container when deployed in the traditional servlet-based model.


---

1. What is a Servlet?

A Servlet is a server-side Java component used to handle HTTP requests.

Simple flow:

Client / Browser
       |
       | HTTP Request
       v
Web Server / Servlet Container
       |
       v
   Servlet
       |
       | Business Logic
       v
 Database / Service / Other API
       |
       v
   Servlet
       |
       | HTTP Response
       v
Client / Browser

For example:

GET /users/101

The Servlet receives the request, processes it, and returns something such as:

{
  "id": 101,
  "name": "Vishal"
}


---

2. Why do we need Servlet?

Before Servlets, Java web applications commonly used technologies such as CGI.

Servlets improved this model because the server can keep a servlet instance alive and use multiple threads to process requests rather than creating a new operating-system process for every request.

Without Servlet

Request
   |
Create process
   |
Execute
   |
Destroy process

This can be expensive.

With Servlet

Servlet Container
      |
 Servlet instance
      |
  Thread 1 -> Request 1
  Thread 2 -> Request 2
  Thread 3 -> Request 3

The same servlet instance can handle multiple requests concurrently.


---

3. Servlet Container

The Servlet Container is one of the most important concepts.

Examples:

Apache Tomcat

Jetty

Undertow


The container manages the servlet lifecycle and provides infrastructure such as:

Creating servlet objects

Calling servlet lifecycle methods

Mapping URLs to servlets

Managing request/response objects

Managing sessions

Authentication-related mechanisms

Thread handling

Loading/unloading web applications


Think of it as:

Servlet = Your application component

Servlet Container = Runtime environment that manages Servlet

For example:

Tomcat
                      |
       +--------------+--------------+
       |              |              |
   Servlet A      Servlet B      Servlet C


---

4. Servlet Architecture

A simplified architecture is:

Client
                |
                | HTTP Request
                v
          Web/Application Server
                |
                v
        Servlet Container
                |
          URL Mapping
                |
                v
            Servlet
                |
       +--------+--------+
       |        |        |
      DB      Service   External API
       |        |        |
       +--------+--------+
                |
                v
          HTTP Response
                |
                v
              Client


---

5. Servlet API

The Servlet API provides classes/interfaces required to create servlets.

Modern Jakarta Servlet packages look like:

jakarta.servlet.*
jakarta.servlet.http.*

Older applications commonly use:

javax.servlet.*
javax.servlet.http.*

This distinction is important when migrating old Java/Spring applications.

Important APIs

Servlet
GenericServlet
HttpServlet

ServletRequest
ServletResponse

HttpServletRequest
HttpServletResponse

ServletConfig
ServletContext

HttpSession
Cookie

Filter
ServletException


---

6. Servlet Class Hierarchy

One important hierarchy is:

Object
  |
  v
Servlet
  |
  v
GenericServlet
  |
  v
HttpServlet
  |
  v
YourServlet

For HTTP applications, normally you extend:

HttpServlet

Example:

public class UserServlet extends HttpServlet {

    @Override
    protected void doGet(
            HttpServletRequest request,
            HttpServletResponse response) {

        // process request
    }
}


---

7. Servlet Interface

At the lowest level, the Servlet API defines the Servlet interface.

Conceptually, it provides lifecycle methods:

init()
service()
destroy()

It also has methods related to configuration.

A simplified view:

public interface Servlet {

    void init(ServletConfig config);

    ServletConfig getServletConfig();

    void service(
        ServletRequest request,
        ServletResponse response);

    String getServletInfo();

    void destroy();
}

You generally don't implement this directly.

Instead:

Servlet
   ↓
GenericServlet
   ↓
HttpServlet


---

8. GenericServlet

GenericServlet is protocol-independent.

It provides a convenient base implementation of the Servlet interface.

public class MyServlet extends GenericServlet {

    @Override
    public void service(
            ServletRequest request,
            ServletResponse response) {

    }
}

But for HTTP applications, HttpServlet is normally preferred.


---

9. HttpServlet

HttpServlet is designed specifically for HTTP.

You usually override methods such as:

doGet()
doPost()
doPut()
doDelete()
doPatch()
doHead()
doOptions()

Example:

public class UserServlet extends HttpServlet {

    @Override
    protected void doGet(
            HttpServletRequest request,
            HttpServletResponse response) {

        response.setContentType("text/plain");

        try {
            response.getWriter().write("Hello User");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}


---

10. Servlet Lifecycle

This is a very important interview topic.

Servlet lifecycle:

Servlet Container
               |
               v
       Load Servlet Class
               |
               v
       Create Servlet Object
               |
               v
            init()
               |
               v
       service() / doGet()
               |
               v
       service() / doPost()
               |
              ...
               |
               v
          destroy()

There are three major lifecycle stages:

1. Initialization
2. Request processing
3. Destruction


---

11. init()

Called by the servlet container when the servlet is initialized.

Example:

@Override
public void init() throws ServletException {
    System.out.println("Servlet initialized");
}

Typically used for initialization tasks.

For example:

private Connection connection;

@Override
public void init() {
    // initialize resources
}

Important

init() is normally called once per servlet instance.


---

12. service()

The container invokes service() for incoming requests.

For HttpServlet, the inherited implementation examines the HTTP method and dispatches to the appropriate method.

Conceptually:

HTTP Request
     |
     v
 service()
     |
     +---- GET    -> doGet()
     |
     +---- POST   -> doPost()
     |
     +---- PUT    -> doPut()
     |
     +---- DELETE -> doDelete()

So if you receive:

GET /users

the flow is approximately:

Container
   |
   v
HttpServlet.service()
   |
   v
doGet()

For:

POST /users

it becomes:

Container
   |
   v
HttpServlet.service()
   |
   v
doPost()


---

13. destroy()

When the servlet is being removed from service, the container calls:

@Override
public void destroy() {
    System.out.println("Servlet destroyed");
}

It can be used for cleanup.

Example:

Application stopping
       |
       v
destroy()
       |
       v
Release resources


---

14. Servlet Lifecycle — Interview Answer

A good senior-level answer:

> The Servlet container manages the servlet lifecycle. It loads the servlet class, creates the servlet instance, invokes init() once for initialization, invokes service() for every incoming request, where HttpServlet dispatches the request to methods such as doGet() or doPost(), and finally invokes destroy() when the servlet is taken out of service.




---

15. How URL Mapping Works

A servlet needs to be associated with a URL.

There are two common approaches.

Annotation

@WebServlet("/users")
public class UserServlet extends HttpServlet {

    @Override
    protected void doGet(
            HttpServletRequest request,
            HttpServletResponse response) {

    }
}

Now:

GET /users

can be mapped to:

UserServlet


---

16. web.xml

Older applications commonly use:

<servlet>
    <servlet-name>UserServlet</servlet-name>

    <servlet-class>
        com.example.UserServlet
    </servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>UserServlet</servlet-name>

    <url-pattern>/users</url-pattern>
</servlet-mapping>

This is called deployment descriptor configuration.

Modern applications often use annotations instead.


---

17. HttpServletRequest

HttpServletRequest represents the incoming HTTP request.

Example:

protected void doGet(
        HttpServletRequest request,
        HttpServletResponse response) {

}

You can retrieve:

Query parameters

Headers

Cookies

Request URI

HTTP method

Request body

Session

Attributes



---

18. Reading Query Parameters

Suppose the client sends:

GET /users?id=101

Servlet:

String id = request.getParameter("id");

Result:

101

Multiple parameters:

/users?id=101&name=Vishal

String id = request.getParameter("id");
String name = request.getParameter("name");


---

19. Request Headers

HTTP request:

Authorization: Bearer xyz
Content-Type: application/json

Read them:

String authorization =
        request.getHeader("Authorization");

String contentType =
        request.getHeader("Content-Type");

You can also enumerate headers:

Enumeration<String> headers =
        request.getHeaderNames();


---

20. Request Method

String method = request.getMethod();

Possible result:

GET
POST
PUT
DELETE
PATCH


---

21. Request URI

String uri = request.getRequestURI();

For:

/users/101

it may return:

/users/101


---

22. Request Body

For a JSON request:

POST /users
Content-Type: application/json

{
    "name": "Vishal"
}

A servlet can access the body using:

BufferedReader reader =
        request.getReader();

Or:

InputStream inputStream =
        request.getInputStream();

You then need to parse JSON using a JSON library such as Jackson.


---

23. HttpServletResponse

HttpServletResponse represents the response sent back to the client.

You can control:

HTTP status

Response headers

Cookies

Content type

Response body


Example:

response.setStatus(HttpServletResponse.SC_OK);


---

24. Sending Response

response.setContentType("text/plain");

PrintWriter writer = response.getWriter();

writer.write("Hello World");

Response:

HTTP/1.1 200 OK

Hello World


---

25. JSON Response

For example:

response.setContentType("application/json");

response.getWriter().write(
    "{\"id\":101,\"name\":\"Vishal\"}"
);

Normally, production applications use a JSON serialization library rather than manually creating JSON strings.


---

26. HTTP Status Codes

Servlet provides constants:

response.setStatus(
    HttpServletResponse.SC_OK);

Common statuses:

200 OK
201 CREATED
204 NO_CONTENT

400 BAD_REQUEST
401 UNAUTHORIZED
403 FORBIDDEN
404 NOT_FOUND

500 INTERNAL_SERVER_ERROR

Example:

response.sendError(
    HttpServletResponse.SC_NOT_FOUND,
    "User not found"
);


---

27. Redirect

Servlet can redirect the client:

response.sendRedirect("/login");

The server sends a redirect response, and the browser makes another request.

Conceptually:

Client
  |
  | GET /profile
  v
Servlet
  |
  | 302 Redirect
  v
Client
  |
  | GET /login
  v
Servlet


---

28. Forward

Servlet can also forward internally:

RequestDispatcher dispatcher =
        request.getRequestDispatcher("/login");

dispatcher.forward(request, response);

Difference:

Redirect

Client
   |
   | Request 1
   v
Server
   |
   | Redirect
   v
Client
   |
   | Request 2
   v
Server

Forward

Client
   |
   | Request
   v
Servlet A
   |
   | forward()
   v
Servlet B

The client does not make a second HTTP request.


---

29. Request Attributes

You can store data in the request:

request.setAttribute(
    "username",
    "Vishal"
);

Retrieve:

String username =
    (String) request.getAttribute("username");

Request attributes are useful for passing data between components during the same request.


---

30. ServletConfig

ServletConfig contains configuration specific to a particular servlet.

Think:

ServletConfig
      |
      +--- Configuration for Servlet A

Example:

ServletConfig config =
        getServletConfig();

It can contain initialization parameters.


---

31. ServletContext

ServletContext represents the web application as a whole.

Think:

Web Application
      |
      +------------------+
      |                  |
 Servlet A           Servlet B
      |                  |
      +--------+---------+
               |
        ServletContext

You can obtain it:

ServletContext context =
        getServletContext();

It can be used for application-wide information and attributes.


---

32. ServletConfig vs ServletContext

Very important interview question.

ServletConfig	ServletContext

Specific to one servlet	Application-wide
One configuration per servlet	One context per web application
Servlet initialization parameters	Application-wide attributes/configuration
Servlet-specific	Shared across application


Simple memory trick:

Config  -> Servlet-specific

Context -> Application-wide


---

33. HttpSession

HttpSession is used to maintain state between multiple requests from a client.

HTTP itself is stateless.

For example:

Request 1:
Login

Request 2:
Get Profile

Request 3:
Get Orders

The server needs a way to associate these requests with the same user/session.

Servlet provides:

HttpSession session =
        request.getSession();

Store data:

session.setAttribute(
    "userId",
    101
);

Retrieve:

Integer userId =
    (Integer) session.getAttribute("userId");


---

34. Cookies

A cookie can be created:

Cookie cookie =
        new Cookie("userId", "101");

response.addCookie(cookie);

Client sends the cookie back in subsequent requests.

Conceptually:

Server
  |
  | Set-Cookie
  v
Browser
  |
  | Cookie
  v
Server


---

35. Session + Cookie

A common session flow:

1. Client -> Login

2. Server creates session

3. Server generates session ID

4. Server sends session ID using cookie

5. Browser stores cookie

6. Browser sends cookie on next request

7. Server uses session ID to locate session data

Example cookie:

JSESSIONID=ABC123


---

36. Servlet Thread Safety

This is a very important senior interview topic.

Generally, the container creates one servlet instance and uses multiple threads to process requests.

Servlet Instance
                    |
        +-----------+-----------+
        |           |           |
     Thread 1    Thread 2    Thread 3
        |           |           |
    Request A    Request B    Request C

Therefore, instance variables can be accessed concurrently.

Dangerous

public class UserServlet extends HttpServlet {

    private String username;

    protected void doGet(...) {
        username = request.getParameter("name");
    }
}

Multiple requests can modify:

username

simultaneously.

This can cause race conditions.


---

37. Better Approach

Use local variables:

protected void doGet(
        HttpServletRequest request,
        HttpServletResponse response) {

    String username =
        request.getParameter("name");
}

Local variables belong to the executing thread's stack.

Generally:

Instance mutable state
       ↓
Potential thread-safety problem

Local variables
       ↓
Generally thread-confined


---

38. Servlet Container Thread Model

Suppose 100 requests arrive:

Servlet
   |
   +-- Thread 1  -> Request 1
   +-- Thread 2  -> Request 2
   +-- Thread 3  -> Request 3
   +-- ...
   +-- Thread N  -> Request N

The exact thread-pool implementation depends on the servlet container.

The key concept is:

> A servlet should be designed to safely handle concurrent requests.




---

39. Filters

A Servlet Filter allows you to intercept requests/responses before or after servlet processing.

Example:

Client
  |
  v
Filter
  |
  v
Servlet
  |
  v
Response

Common uses:

Authentication

Authorization

Logging

Request tracing

CORS

Encoding

Security checks


Example:

@WebFilter("/*")
public class LoggingFilter
        implements Filter {

    @Override
    public void doFilter(
            ServletRequest request,
            ServletResponse response,
            FilterChain chain)
            throws IOException, ServletException {

        System.out.println("Before servlet");

        chain.doFilter(request, response);

        System.out.println("After servlet");
    }
}


---

40. Filter Chain

Multiple filters can be configured:

Client
  |
  v
Authentication Filter
  |
  v
Logging Filter
  |
  v
Authorization Filter
  |
  v
Servlet

Each filter calls:

chain.doFilter(request, response);

to continue processing.

If it doesn't call it:

return;

the request can be stopped there.


---

41. Servlet vs Filter

Servlet	Filter

Handles request	Intercepts request/response
Main request processing	Cross-cutting processing
Generates response	Can modify/intercept response
Endpoint-oriented	Middleware-oriented



---

42. Listener

Servlet API also provides listeners for application lifecycle/events.

Examples:

ServletContextListener
HttpSessionListener
ServletRequestListener

For example:

public class AppListener
        implements ServletContextListener {

    @Override
    public void contextInitialized(
            ServletContextEvent event) {

        System.out.println("Application started");
    }

    @Override
    public void contextDestroyed(
            ServletContextEvent event) {

        System.out.println("Application stopped");
    }
}


---

43. Complete Request Flow

Suppose we have:

POST /users
Content-Type: application/json
Authorization: Bearer abc

The flow can be understood as:

Client
                    |
                    | HTTP POST
                    v
             Web Server
                    |
                    v
          Servlet Container
                    |
                    v
              Filter 1
          Authentication
                    |
                    v
              Filter 2
               Logging
                    |
                    v
              URL Mapping
                    |
                    v
               Servlet
                    |
                    v
              doPost()
                    |
                    v
            Read Request Body
                    |
                    v
             Business Logic
                    |
                    v
                Database
                    |
                    v
             Create Response
                    |
                    v
             HTTP Response
                    |
                    v
                  Client


---

44. Servlet and Spring MVC

This is especially important if you're working with Spring MVC.

Traditional Spring MVC architecture:

Client
  |
  v
Servlet Container
  |
  v
DispatcherServlet
  |
  v
HandlerMapping
  |
  v
Controller
  |
  v
Service
  |
  v
Repository
  |
  v
Database

The important point:

> DispatcherServlet is itself a servlet.



For example:

public class DispatcherServlet
        extends FrameworkServlet {
}

And Spring's FrameworkServlet ultimately participates in the Servlet API lifecycle.


---

45. @RestController and Servlet

If you use:

@RestController
public class UserController {

    @GetMapping("/users")
    public List<User> getUsers() {
        return users;
    }
}

You don't normally write:

HttpServlet

yourself.

But underneath the Spring MVC stack, the request is handled through the servlet infrastructure.

Simplified:

HTTP Request
     |
     v
Tomcat
     |
     v
DispatcherServlet
     |
     v
Spring MVC
     |
     v
@RestController
     |
     v
Service

So learning Servlet concepts helps you understand Spring MVC internally.


---

46. Servlet vs REST Controller

Servlet	@RestController

Low-level web API	Higher-level Spring abstraction
You manually process request/response	Spring handles much of it
HttpServletRequest	Usually method parameters
HttpServletResponse	Usually return object
Manual JSON handling possible	Automatic JSON conversion
URL mapping with Servlet APIs	@GetMapping, @PostMapping, etc.


Example Servlet:

protected void doGet(
        HttpServletRequest request,
        HttpServletResponse response) {

    response.getWriter().write("Hello");
}

Spring:

@GetMapping("/hello")
public String hello() {
    return "Hello";
}

Spring makes the programming model simpler, but the underlying web infrastructure is still important.


---

47. Servlet vs JSP

Servlet:

Java code
      ↓
Request processing

JSP:

HTML/view-oriented template
      ↓
Presentation

Historically, MVC applications commonly used:

Controller -> Servlet
                  |
                  v
                 JSP

Modern REST applications usually return JSON rather than JSP views.


---

48. Servlet Exception

Servlet API provides:

ServletException

Example:

throw new ServletException(
    "Unable to process request");

It represents errors related to servlet processing.


---

49. Dispatcher Types

Servlet filters can be applied to different dispatcher types:

REQUEST
FORWARD
INCLUDE
ERROR
ASYNC

This becomes relevant when working with filters and forwarding/error handling.


---

50. Async Servlet Processing

Servlets also support asynchronous processing.

Conceptually:

HTTP Request
     |
     v
Servlet
     |
 startAsync()
     |
     v
Background Processing
     |
     v
Response

Example conceptually:

AsyncContext asyncContext =
        request.startAsync();

This allows the servlet request to be processed asynchronously instead of holding the original request thread for the entire operation.


---

51. Servlet Security

Servlet applications can participate in security mechanisms involving:

Authentication

Authorization

Security constraints

Roles

HTTPS

Cookies

Sessions


For example, a request might flow through:

Client
  |
  v
Security Filter
  |
  +---- authenticated? ---- No ---> 401
  |
 Yes
  |
  v
Authorization
  |
  +---- allowed? ----------- No ---> 403
  |
 Yes
  |
  v
Servlet

In Spring applications, Spring Security usually provides a higher-level implementation around these concepts.


---

52. Important Servlet Objects

You should remember these:

HttpServlet
       |
       +-- doGet()
       +-- doPost()
       +-- doPut()
       +-- doDelete()

HttpServletRequest
       |
       +-- getParameter()
       +-- getHeader()
       +-- getSession()
       +-- getRequestURI()
       +-- getReader()

HttpServletResponse
       |
       +-- setStatus()
       +-- setHeader()
       +-- getWriter()
       +-- sendError()
       +-- sendRedirect()

HttpSession
       |
       +-- setAttribute()
       +-- getAttribute()
       +-- invalidate()

ServletContext
       |
       +-- application-wide information

ServletConfig
       |
       +-- servlet-specific configuration


---

53. Complete Servlet Example

A simple servlet:

@WebServlet("/users")
public class UserServlet extends HttpServlet {

    @Override
    public void init() throws ServletException {
        System.out.println("UserServlet initialized");
    }

    @Override
    protected void doGet(
            HttpServletRequest request,
            HttpServletResponse response)
            throws IOException {

        String id =
            request.getParameter("id");

        response.setContentType("application/json");

        response.getWriter().write(
            "{\"id\":\"" + id + "\"}"
        );
    }

    @Override
    protected void doPost(
            HttpServletRequest request,
            HttpServletResponse response)
            throws IOException {

        String body = request.getReader()
                             .lines()
                             .reduce("", (a, b) -> a + b);

        response.setStatus(
            HttpServletResponse.SC_CREATED);

        response.getWriter().write(
            "User created"
        );
    }

    @Override
    public void destroy() {
        System.out.println("UserServlet destroyed");
    }
}


---

54. What Happens Internally?

For:

GET /users?id=101

roughly:

1. Client sends HTTP request

2. Request reaches Tomcat

3. Tomcat's servlet container receives it

4. Container determines the application/context

5. Container performs servlet URL mapping

6. Container identifies UserServlet

7. If not initialized:
       create UserServlet
       call init()

8. Container invokes service()

9. HttpServlet.service() examines HTTP method

10. GET -> doGet()

11. doGet() reads:
       request.getParameter("id")

12. Application processes request

13. Servlet writes response

14. Container sends HTTP response

15. Servlet remains available for future requests


---

55. Most Important Interview Questions

For a Java/Spring senior interview, focus particularly on these:

Basic

1. What is a Servlet?


2. Why do we need Servlets?


3. What is a Servlet Container?


4. What is Tomcat?


5. What is the Servlet lifecycle?


6. What is HttpServlet?


7. Difference between GenericServlet and HttpServlet.



Request/Response

8. What is HttpServletRequest?


9. What is HttpServletResponse?


10. How do you read query parameters?


11. How do you read request headers?


12. How do you read request body?


13. How do you set HTTP status?


14. Difference between sendRedirect() and forward().



Configuration

15. What is ServletConfig?


16. What is ServletContext?


17. Difference between ServletConfig and ServletContext.


18. What is web.xml?


19. What is @WebServlet?



State

20. Why is HTTP stateless?


21. What is HttpSession?


22. How does JSESSIONID work?


23. What are cookies?



Concurrency

24. Is a servlet thread-safe?


25. How many servlet objects are created?


26. Can multiple requests execute the same servlet simultaneously?


27. Why are mutable instance variables dangerous in a servlet?



Advanced

28. What is a Filter?


29. What is Filter Chain?


30. What are Servlet Listeners?


31. What is async servlet processing?


32. What is DispatcherServlet?


33. How does Spring MVC use Servlets?


34. What happens internally when a request reaches a Spring @RestController?




---

56. The Most Important Mental Model

If you're learning Servlet for Java + Spring MVC, remember this hierarchy:

HTTP
                  |
                  v
          Servlet Container
             (Tomcat)
                  |
                  v
              Servlet
                  |
                  v
        DispatcherServlet
          (Spring MVC)
                  |
                  v
          Handler Mapping
                  |
                  v
             Controller
                  |
                  v
              Service
                  |
                  v
            Repository
                  |
                  v
              Database

And remember the Servlet lifecycle:

Load
          |
          v
       Create
          |
          v
        init()
          |
          v
       service()
          |
     +----+----+
     |         |
  doGet()   doPost()
     |         |
     +----+----+
          |
          v
       destroy()

For your Java 8 → newer Java + Spring 5.3.x migration work, the especially important Servlet topics are javax.servlet vs jakarta.servlet, Servlet container/Tomcat, DispatcherServlet, filters, lifecycle, request threading, ServletContext, and how Spring MVC sits on top of the Servlet API.