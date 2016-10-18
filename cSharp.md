# cSharp
## Documentation
- C# Styling *https://github.com/dotnet/corefx/blob/master/Documentation/coding-guidelines/coding-style.md*
- Nancy Documentataion: *https://github.com/NancyFx/Nancy/wiki/Documentation*
- Razor Documentation: *http://haacked.com/archive/2011/01/06/razor-syntax-quick-reference.aspx/*
- ADO.NET Explanation: https://en.wikipedia.org/wiki/ADO.NET
- ADO.NET Documentation: https://msdn.microsoft.com/en-us/library/e80y5yhx(v=vs.110).aspx
- MSDN Documentation on testing: https://msdn.microsoft.com/en-us/library/hh694602.aspx

## Creating a Csharp Program
Make a file that ends with .cs.  Make sure a runtime is loaded on computer, like Mono).  Here is basics needed for each file.
```csharp
using System;

class Goodbye
{
  static void Main()
  {
    //Everything godes here, like:
    Console.WriteLine("Goodbye World");
  }
}
```

## Creating a C# project with Nancy
You may need to check for the last version
```console
$ dnvm install latest
$ dnvm upgrade
$ apm install language-cshtml //needs node.js
```
Common starting folders
```console
Modules
Views
Content
Tests
Objects
> Startup.cs
> project.json
> readme.md
> xunit.runner.json
> Modules \ HomeModule.cs
> Objects \ [aClass].cs
> Objects \ Database.cs
> Views \ index.cshtml
> Tests \ [aClass]Tests.cs
> Content \ styles.css
> Content \ scripts.js
```

###### Common namespaces

```cs
using System; //The System namespace contains fundamental classes and base classes that define commonly-used value and reference data types, events and event handlers, interfaces, attributes, and processing exceptions.
using System.IO; //The System.IO namespace contains types that allow reading and writing to files and data streams, and types that provide basic file and directory suppor
using System.Collections.Generic; //The System.Collections.Generic namespace contains interfaces and classes that define generic collections, which allow users to create strongly typed collections that provide better type safety and performance than non-generic strongly typed collections.
using System.Data; //The System.Data namespace provides access to classes that represent the ADO.NET architecture. ADO.NET lets you build components that efficiently manage data from multiple data sources.
using System.Data.SqlClient; //The System.Data.SqlClient namespace is the.NET Framework Data Provider for SQL Server.
using Microsoft.AspNet.Builder; //Identity extensions for IApplicationBuilder. Defines a class that provides the mechanisms to configure an application’s request pipeline.
using Nancy; // Nancy is a lightweight, low-ceremony, framework for building HTTP based services on .NET and Mono. Read lots more at their documentation.
using Nancy.Owin; //OWIN defines a standard interface between .NET web servers and web applications. The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.
using Nancy.ViewEngines.Razor; //The Razor engine in Nancy is a custom implementation built around the Razor syntax parser. Please note that the implementation may have differences to the implementation used by ASP.NET MVC.

```


## SetUp a new project
To start a c# program using Nancy (as set up by Epicodus), you need to create a json file that pass a set of dependencies.  The name for this file is **project.json.**  When **> dnu restore** is typed into the command line, it will create a larger json file necessary for the compiler.  This includes telling the program to find the Startup.cs and provides a set of namespaces. File **xunit.runner.json** sets Xunit testing dependencies (needed if you want each test one at a time--like when running tests in databases)

###### project.json
```json
{
  "dependencies": {
    "Microsoft.AspNet.Server.Kestrel": "1.0.0-*",
    "Microsoft.AspNet.Owin": "1.0.0-*",
    "Nancy": "1.3.0",
    "Nancy.ViewEngines.Razor": "1.3.0",
    "xunit": "2.1.0",
    "xunit.runner.dnx": "2.1.0-rc1-*"
  },
  "commands": {
    "kestrel": "Microsoft.AspNet.Hosting --server Microsoft.AspNet.Server.Kestrel --server.urls http://localhost:5004",
    "test": "xunit.runner.dnx"
  },
  "frameworks": {
    "dnx451": {
      "frameworkAssemblies": {
        "System.Data": "4.0.0.0"
      }
    }
  }
}
```
###### xunit.runner.json
```json
{
  "parallelizeAssembly" : false,
  "parallelizeTestCollections" : false
}
```

When making a project with Nancy, Nancy looks for a file called **Startup.cs** .  This file will use these namespaces in Startup.cs.  
```cs
using System;
using System.IO;
using System.Collections.Generic;
using Microsoft.AspNet.Builder;
using Nancy;
using Nancy.Owin;
using Nancy.ViewEngines.Razor;
```

In the file **Startup.cs** and inside the "primary" project namespace, you will need to add Startup class to set up Nancy working correctly.  If you will be connecting to databases, include another class (conventionally named *DBConfiguration*).

###### Startup.cs  
```cs
//Remember to:
// - Update ProjectCore namespace.
// - Update database connection information.

namespace ProjectCore
{
  public class Startup
  {
    public void Configure(IApplicationBuilder app)
    {
      app.UseOwin(x => x.UseNancy());
    }
  }
  public class CustomRootPathProvider : IRootPathProvider
  {
    public string GetRootPath()
    {
      return Directory.GetCurrentDirectory();
    }
  }
  public class RazorConfig : IRazorConfiguration
  {
    public IEnumerable<string> GetAssemblyNames()
    {
      return null;
    }

    public IEnumerable<string> GetDefaultNamespaces()
    {
      return null;
    }

    public bool AutoIncludeModelNamespace
    {
      get { return false; }
    }
  }
  public static class DBConfiguration
  {
    // Epicodus DataBase Information (Microsoft sequl server)
    // Server type: Database Engine
    // Server name: (localdb)\MSSQLLocalDB
    // Authentication: Windows Authentication

    public static string dataSource = "Data Source=(localdb)\\mssqllocaldb"; // Data Source identifies the server.
    public static string databaseName = "database"; // Initial Catalog is the database name
    //Integrated Security sets the security of the database access to the Windows user that is currently logged in.
    public static string ConnectionString = ""+dataSource+";Initial Catalog="+databaseName+";Integrated Security=SSPI;";
  }
}
```

Additionally, the HomeModule.cs with Nancy should have the code.  The beauty of Nancy is the ease of loading a "View", as seen below. Please note that a index.cshtml file is needed.  

###### HomeModule.cs
```csharp
using Nancy;
// Remember to:
// - Update ProjectCore namespace
namespace ProjectCore
{
  public class HomeModule : NancyModule
  {
    public HomeModule()
    {
      Get["/"] = _ => {
        return View["index.cshtml"];
      };
    }
  }
}

```
From here it is common to put in the database connection information into the objects folder in a file called *Database.cs*. The line *ProjectCore* connects back to the class that we set up in *Startup.cs*.  

###### Database.cs
```csharp
using System.Data;
using System.Data.SqlClient;
// Remember to:
// - Update ProjectCore namespace

namespace ProjectCore
{  
  public class DB
  {
    public static SqlConnection Connection()
    {
      SqlConnection conn = new SqlConnection(DBConfiguration.ConnectionString);
      return conn;
    }
  }
}
```



###### [AClass].cs
```csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
// Remember to:
// - Update ProjectCore namespace

namespace ProjectCore.Objects
{
  public class AClass
  {
    // //Overrides are for typcasting
    //   public override bool Equals(System.Object otherKitten)
    // {
    //   if (!(otherKitten is Kitten))
    //   {
    //     return false;
    //   }
    //   else
    //   {
    //     Kitten newKitten = (Kitten) otherKitten;
    //     return this.GetName().Equals(newKitten.GetName());
    //   }
    // }
    //
    // public override int GetHashCode()
    // {
    //      return this.GetName().GetHashCode();
    // }
  }
}
```
Update ClassTests.cs file.

###### ClassTests.cs
```c#
using System;
using Xunit;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;

namespace Tests
{
  public class Testing : IDisposable
  {
    public ToDoTest()
    {
      string dataSource = "Data Source=(localdb)\\mssqllocaldb"; // Data Source identifies the server.
      string databaseName = "database"; // Initial Catalog is the database name
      //Integrated Security sets the security of the database access to the Windows user that is currently logged in.
      DBConfiguration.ConnectionString = ""+dataSource+";Initial Catalog="+databaseName+";Integrated Security=SSPI;";
    }


    public void Dispose()
    {
      // Item.DeleteAll();
    }
  }
}

```

## Background on cSharp

From Epicodus LEsson cSharp.
> To have access to C#, we need to install an implementation of the .NET Framework. This will give us and what is called the Common Language Runtime. We recommend using Mono. It's open-source and even better, it's cross-platform.

> C#, pronounced see sharp, began development in the late 90's. Released as a part of the .NET Framework initiative in July 2000, C# is part of a family of computer languages which evolved out of the C/C++ Programming Language, which originated in the late 60's at Bell Labs.

> Today, C# is the object-oriented component of the ASP.NET Framework. Microsoft Chief Architect and C# team leader Anders Hejlsberg described it as an iteration and improvement of the C++ language. Hejlsberg touted C# as "the first component-oriented language in the C/C++ family." As a result, it is " a simple, modern, general-purpose, object-oriented programming language" and to be "economical with regard to memory and processing power requirements".

From Wikipedia
> The Common Language Runtime (CLR), the virtual machine component of Microsoft's .NET framework, manages the execution of .NET programs. A process known as just-in-time compilation converts compiled code into machine instructions which the computer's CPU then executes.[1] The CLR provides additional services including memory management, type safety, exception handling, garbage collection, security and thread management. All programs written for the .NET framework, regardless of programming language, are executed by the CLR. All versions of the .NET framework include CLR.

> A compiler is a computer program (or a set of programs) that transforms source code written in a programming language (the source language) into another computer language (the target language), with the latter often having a binary form known as object code.

## Basic Methods / Code Snippets

### Set up a Namespace and Class
```csharp
namespace NameOfNamespace
{
  attribute class NameOfClass
  {

  }
}
```
Example
```csharp
namespace MyProject
{
  public class Project
  {
    //Everything goes here  
  }
}

```


#### How to create a property inside a class

```csharp

attributes _propertyName;
```
Example
```csharp
private string _input1;

//properteis should always have the attribute private

```

#### How to create a Setter inside a class

```csharp
attributes NameOftheFunction( attribute prameter)
{
 _property = parameter;
}
```
Example
```csharp
public void SetInput1 (string aString)
{
  _input1 = aString;
}
//Attribute examples are: public, private, void, string, int, static
```
#### How to write a Getter inside a class

```csharp
attributes AnyName ()
{
  return _property
}
```
Example
```csharp
 public string GetInput1 ()
 {
   return _input1
 }
```

#### How to create an instance of a class

```csharp

ClassName newIstanceName = new ClassName ();
//Attribute examples are: public, private, void, string, int, static
```
Example
```csharp
Project newProject = new Project ();
```

#### How to use a method in a class on an instance of the class

```csharp

newIstanceName.MethodName(parameters?);
//Attribute examples are: public, private, void, string, int, static
```
Example
```csharp
newProject.SetInput1("hello");
```
#### Make a DB Connection
`DB.Connection()` is a method in the database.cs file.

```cSharp
  List<Task> allTasks = new List<Task>{};

  SqlConnection conn = DB.Connection();
  conn.Open();

  SqlCommand cmd = new SqlCommand("SELECT * FROM tasks;", conn);
  SqlDataReader rdr = cmd.ExecuteReader();

  while(rdr.Read())
  {
    int taskId = rdr.GetInt32(0);
    string taskDescription = rdr.GetString(1);
    Task newTask = new Task(taskDescription, taskId);
    allTasks.Add(newTask);
  }

  if (rdr != null)
  {
    rdr.Close();
  }
  if (conn != null)
  {
    conn.Close();        
  }
  return allTasks;
```
