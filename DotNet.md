##### What is ASP.NET?

ASP.NET is an open-source server-side web application framework designed for web development to produce dynamic web pages. It was developed by Microsoft to allow programmers to build dynamic web sites, web applications and web services.

It was first released in January 2002 with version 1.0 of the .NET Framework, and is the successor to Microsoft's Active Server Pages (ASP) technology. ASP.NET is built on the Common Language Runtime (CLR), allowing programmers to write ASP.NET code using any supported .NET language. The ASP.NET SOAP extension framework allows ASP.NET components to process SOAP messages ( _SOAP (Simple Object Access Protocol) is a protocol specification for exchanging structured information in the implementation of web services in computer networks[1]._)[1].

ASP.NET's successor is ASP.NET Core. It is a re-implementation of ASP.NET as a modular web framework, together with other frameworks like Entity Framework (_Entity Framework (EF) is an object-relational mapper that enables .NET developers to work with relational data using domain-specific objects. It eliminates the need for most of the data-access code that developers usually need to write_[2]). The new framework uses the new open-source .NET Compiler Platform (codename "Roslyn") and is cross platform. ASP.NET MVC, ASP.NET Web API, and ASP.NET Web Pages (a platform using only Razor pages) have merged into a unified MVC 6.[1]

##### Ok, Catch me up on what a framework is again?
In computer programming, a software framework is an abstraction in which software providing generic functionality can be selectively changed by additional user-written code, thus providing application-specific software. A software framework is a universal, reusable software environment that provides particular functionality as part of a larger software platform to facilitate development of software applications, products and solutions. Software frameworks may include support programs, compilers, code libraries, tool sets, and application programming interfaces (APIs) that bring together all the different components to enable development of a project or system.[1]

Frameworks have key distinguishing features that separate them from normal libraries:[1]

- inversion of control:   
In a framework, unlike in libraries or in standard user applications, the overall program's flow of control is not dictated by the caller, but by the framework.[1]
- extensibility:   
A user can extend the framework - usually by selective overriding; or programmers can add specialized user code to provide specific functionality.
- non-modifiable framework code:    
The framework code, in general, is not supposed to be modified, while accepting user-implemented extensions. In other words, users can extend the framework, but should not modify its code.[1]

##### Tell me about Startup.cs
Startup.cs is the application's entry point. It wires up the configuration and services used by the app. ASP.NET calls Configure when the app starts.[3]

Startup.cs file is a replacement of Global.asax file in ASP.NET Web Form application. We know that Global.asax is an optional file to handle Application level event in ASP.NET application. In ASP.NET 5, Startup.cs file has introduce to server same.[4]

Though the purpose is the same, the implementation of Startup.cs file is entirely different. Global.asax file contains mostly application level pre-defined events where Startup.cs file is more about registering services and injection of modules in HTTP pipeline. Startup.cs file contains Startup class which triggers at first when application launches and even in each HTTP request/response.[4]

##### But then what is Configure and ConfigureServices?
ConfigureService method expects implementation of IServiceCollection type as argument which is needed to register the services. ConfigureService method executes before Configuration method because sometimes, it needs to register the service before injecting to HTTP pipeline. IServiceCollection interface has implemented in many classes so that it offers many Add[something] extension methods to register services.

The Config method is used to specify how the application will respond in each HTTP request. Which implies that Configure method is HTTP request specific. __The most typical use of configure method is to inject middleware in HTTP pipeline.__ The configure method must accept IApplicationBuilder parameter with some optional in build services like IHostingEnvironment and ILoggerFactory. Once we add some service in ConfigureService method, it will be available to Configure method to be used. That’s why ConfigureService executes before Configure.

async and wait article: https://msdn.microsoft.com/en-us/magazine/hh456401.aspx.

##### Middleware is waht?/?
software that acts as a bridge between an operating system or database and applications, especially on a network.[6]

##### What is the dependency "Microsoft.AspNetCore.Mvc"
ASP.NET Core MVC is a web framework that gives you a powerful, patterns-based way to build dynamic websites and web APIs. ASP.NET Core MVC enables a clean separation of concerns and gives you full control over markup.
Install-Package Microsoft.AspNetCore.Mvc -Version 1.0.1

##### What is the entity-framework?
Now, we’re going to use an object relational mapping (or ORM) framework called Entity Framework. ORMs like Entity Framework do a lot of the work to turn information from a database into objects that the .NET Framework can understand, and you use those objects to interact with the database instead of interacting with the database directly.

##### What are the square brackets in data Annotations?
The square brackets denote data annotations, which come from the dependencies at the top, and give .NET extra information about the code.

##### Quick Damn, that _IApplicationBuilder UseMvc_ uses some different syntax.
To understand importing the UseMvc extension method and a fluent interface (fluent interface - like method cascading).  From the MDN (and stack overflow):
Extension methods allow developers to add new methods to the public contract of an existing CLR type, without having to sub-class it or recompile the original type. Extension Methods help blend the flexibility of "duck typing" support popular within dynamic languages today with the performance and compile-time validation of strongly-typed languages.[5]

Extension Methods enable a variety of useful scenarios, and help make possible the really powerful LINQ query framework...it means that you can call
``` c#
MyClass myClass = new MyClass();
int i = myClass.Foo();
```
rather than
``` c#
MyClass myClass = new MyClass();
int i = Foo(myClass);
```
This allows the construction of fluent interfaces as stated below.

1 _Wikipedia_  
2 https://www.asp.net/entity-framework
3 Epicodus
4 http://www.codeproject.com/Tips/1069787/Understanding-Startup-cs-file-in-ASP-NET
5 http://stackoverflow.com/questions/846766/use-of-this-keyword-in-formal-parameters-for-static-methods-in-c-sharp
6 Google
