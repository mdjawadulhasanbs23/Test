# How to Run the Project Locally
## Prerequisites

Before you begin, ensure that you have the following installed on your machine:

- [.NET SDK](https://dotnet.microsoft.com/download)
- [Visual Studio](https://visualstudio.microsoft.com/) or [Visual Studio Code](https://code.visualstudio.com/) 
- [SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)

# Code Review Checklist
 
### Requirements & Functionality
- [ ] Have the requirements been met? The new feature will not negatively impact existing functionality?
 
### Maintainability & Best Practices
- [ ] Is the code easy to read, proper naming convention followed and code method/class not too long?
- [ ] Is the code not repeated (DRY Principle) and followed single responsibility principle?
- [ ] Relevant Parameters are configurable? (Arch)
- [ ] Are different errors handled correctly? Are errors and warnings logged?
- [ ] Add meaningful Comments and avoid unnecessary comments?
 
### Code Formatting
- [ ] Is the code formatted correctly? Unecessary whitespace removed? Same formatting throughout the code base. Code is fitted to the monitor screen? Properly Aligned?
 
### Performance
- [ ] Optimize code- DB query, Looping, Conditions, Proper data type
 
### Version Controlling
- [ ] Maintain environment and feature branch
- [ ] Proper commit message
- [ ] Pull/merge request
- [ ] Use tagging
 
## How to Run the Project Locally

To run the MSFA App locally, follow these steps:

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-username/dotnet-clean-architecture.git
   
2. **Navigate to the Project Directory**
   ```bash
   cd MSFA23
   
3. **Navigate to the Project Directory**
   ```bash
   dotnet restore

4. **Apply Migrations**
   ```bash
   dotnet ef database update
5. **Build the Solution**
   ```bash
   dotnet build
6. **Run the Application:**
   ```bash
   dotnet run --MSFA23/Host.WebApi
   
## Live Architectural Diagram

View our live architectural diagram [here](https://drive.google.com/file/d/1l9cawdqxZcvz-EPoqxiWRGaz6UPD7yCv/view?usp=sharing).
   
 ## Coding Structure and Guidelines 
**Folder Structure**

- The Clean Architecture project, named MSFA23, adheres to a well-defined structure with a clear separation of concerns. It organizes code into distinct layers such as Core, Migrators, Host, and Infrastructure, following a modular design that enhances maintainability and scalability.

```
MSFA23
├──src
|  ├ Core   
|  |    ├ Application  
|  |    ├ Domain
|  |    ├ Shared
|  |
|  ├ Migrators  
|  |    ├ Migrators.MSSQL  
|  |    ├ Migrators.MySQL
|  |    ├ Migrators.Oracle
|  |    ├ Migrators.PostgreSQL
|  |
|  ├ Host
|  ├ Infrastructure
| 
├──test

```

- Host contains the API controllers and startup logic for an ASP.NET Core application, including the container setup. This is the entry point of the application, and it also contains other static files such as logs, localization JSONs, images, email templates, and most importantly, the configuration files.


```
├── Host
|   ├── Configurations
|   ├── Controllers
|   ├── Email Templates
|   ├── Extensions
|   ├── Files
|   |   ├── Images
|   |   └── Documents
|   ├── Localization
|   ├── Logs
|   └── appsettings.json

```
> Host project depends on Application, Infrastructure, Migration Projects

- This project structure follows a feature-based organization,separation of classes by type is recommended for more complex features, maintaining a clean and organized codebase. It contains application-specific business rules and use cases

```
├── Core
|   ├── Application
|   |   ├── Auditing
|   |   ├── Catalog
|   |   ├── Common
|   |   ├── Dashboard
|   |   ├── Identity
|   |   ├── Area
|   |   ├── Journey
|   |   ├── Order

```
 >Application project depends only on the Core projects which are Shared and Domain.


- Create a 'Command' and 'Query' folder to separate command and query responsibilities in the application layer.
Introduce a 'Shared' folder in the application layer for Data Transfer Objects (DTOs) and Specifications.

```

├──Application
|    ├ Order   
|       ├ Command
|          ├ CreateOrderRequest
|       ├ Query
|          ├ GetOrderListRequest
|       ├ Shared
|          ├ Dtos
|             ├ CreateOrderDto.cs
|          ├ Specifications
|             ├ GetOrderByCustomerIdSpec.cs
|  
|     

```

- Keep similar domain entities within a single parent folder. 

```

├──Domain
|    ├ Area   
|       ├ AreaCategory
|       ├ AreaMaster
|       ├ MarketArea
|       ├ DepotArea

```

 _Why:_

  > Organizing similar domain entities within a single parent folder improves code structure and enhances maintainability. This practice makes it easier to locate and manage related entities, facilitating a more intuitive and efficient development process.

  
 - Dedicate a separate startup file for each independent component within the infrastructure. Register these individual startup files in a common startup file. Also, register middleware in their respective startup files.

```

├──Infrastructure
|    ├ LocationUpdate   
|    |   ├ Location
|    |   ├ LocationHub
|    |   ├ LocationMiddleWare
|    |   ├ Startup
|    |
|    ├ Startup

```
 
 _Why:_

  > This approach enhances modularity, maintainability, and scalability of the project.
  
  
   - Depending on the size of the task use `//TODO:` comments or open a ticket.

 _Why:_

  > So then you can remind yourself and others about a small task (like refactoring a function or updating a comment). For larger tasks use `//TODO(#3456)` which is enforced by a lint rule and the number is an open ticket.
  
  - Always comment and keep them relevant as code changes. Remove commented blocks of code.

  _Why:_

  > Your code should be as readable as possible, you should get rid of anything distracting. If you refactored a function, don't just comment out the old one, remove it.
  
  - Make your names search-able with meaningful distinctions avoid shortened names. For functions use long, descriptive names. A function name should be a verb or a verb phrase, and it needs to communicate its intention.

```
UpdateCustomerLocation() 

```
_Why:_

  > It makes it more natural to read the source code.
  
  
- Removed unused using directives.

_Why:_

> To keep the code clean and improve maintainability. Unused directives can clutter the file and make it harder to identify the actual dependencies of the code.
  

  
  ### API design

_Why:_

> Because we try to enforce development of sanely constructed RESTful interfaces, which team members and clients can consume simply and consistently.

_Why:_

> Lack of consistency and simplicity can massively increase integration and maintenance costs. Which is why `API design` is included in this document.

- We mostly follow resource-oriented design. It has three main factors: resources, collection, and URLs.

  - A resource has data, gets nested, and there are methods that operate against it.
  - A group of resources is called a collection.
  - URL identifies the online location of resource or collection.

  _Why:_

  > This is a very well-known design to developers (your main API consumers). Apart from readability and ease of use, it allows us to write generic libraries and connectors without even knowing what the API is about.

- Always use a plural nouns for naming a url pointing to a collection: `/users`.

  _Why:_

  > Basically, it reads better and keeps URLs consistent. [read more...](https://apigee.com/about/blog/technology/restful-api-design-plural-nouns-and-concrete-names)



- Explain the CRUD functionalities using HTTP methods:

  _How:_

  > `GET`: To retrieve a representation of a resource.

  > `POST`: To create new resources and sub-resources.

  > `PUT`: To update existing resources.

  > `PATCH`: To update existing resources. It only updates the fields that were supplied, leaving the others alone.

  > `DELETE`: To delete existing resources.


- Use a simple ordinal number for a version with a `v` prefix (v1, v2). Move it all the way to the left in the URL so that it has the highest scope:

  ```
  http://api.domain.com/v1/order/3/
  ```

  _Why:_

  > When your APIs are public for other third parties, upgrading the APIs with some breaking change would also lead to breaking the existing products or services using your APIs. Using versions in your URL can prevent that from happening. [read more...](https://apigee.com/about/blog/technology/restful-api-design-tips-versioning)

- Response messages must be self-descriptive. A good error message response might look something like this:

  ```json
  {
  "messages": [
    "string"
  ],
  
  "source": "string",
  "exception": "string",
  "errorId": "string",
  "supportMessage": "string",
  "statusCode": 0
  }
  ```


  _Why:_

  > developers depend on well-designed errors at the critical times when they are troubleshooting and resolving issues after the applications they've built using your APIs are in the hands of their users.

  _Note: Keep security exception messages as generic as possible. For instance, Instead of saying ‘incorrect password’, you can reply back saying ‘invalid username or password’ so that we don’t unknowingly inform user that username was indeed correct and only the password was incorrect._

- Use these status codes to send with your response to describe whether **everything worked**,
  The **client app did something wrong** or The **API did something wrong**.

      _Which ones:_
      > `200 OK` response represents success for `GET`, `PUT` or `POST` requests.

      > `201 Created` for when a new instance is created. Creating a new instance, using `POST` method returns `201` status code.

      > `204 No Content` response represents success but there is no content to be sent in the response. Use it when `DELETE` operation succeeds.

      > `304 Not Modified` response is to minimize information transfer when the recipient already has cached representations.

      > `400 Bad Request` for when the request was not processed, as the server could not understand what the client is asking for.

      > `401 Unauthorized` for when the request lacks valid credentials and it should re-request with the required credentials.

      > `403 Forbidden` means the server understood the request but refuses to authorize it.

      > `404 Not Found` indicates that the requested resource was not found.

      > `500 Internal Server Error` indicates that the request is valid, but the server could not fulfill it due to some unexpected condition.

      _Why:_
      > Most API providers use a small subset HTTP status codes. For example, the Google GData API uses only 10 status codes, Netflix uses 9, and Digg, only 8. Of course, these responses contain a body with additional information. There are over 70 HTTP status codes. However, most developers don't have all 70 memorized. So if you choose status codes that are not very common you will force application developers away from building their apps and over to wikipedia to figure out what you're trying to tell them. [read more...](https://apigee.com/about/blog/technology/restful-api-design-what-about-errors)

- Provide total numbers of resources in your response.
- Accept `limit` and `offset` parameters.
- Pagination, filtering, and sorting don’t need to be supported from start for all resources. Document those resources that offer filtering and sorting.

<a name="api-security"></a>

### API security

These are some basic security best practices:

- Don't use basic authentication unless over a secure connection (HTTPS). Authentication tokens must not be transmitted in the URL: `GET /users/123?token=asdf....`

  _Why:_

  > Because Token, or user ID and password are passed over the network as clear text (it is base64 encoded, but base64 is a reversible encoding), the basic authentication scheme is not secure. [read more...](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

- Tokens must be transmitted using the Authorization header on every request: `Authorization: Bearer xxxxxx, Extra yyyyy`.

- Authorization Code should be short-lived.

- Reject any non-TLS requests by not responding to any HTTP request to avoid any insecure data exchange. Respond to HTTP requests by `403 Forbidden`.

- Consider using Rate Limiting.

  _Why:_

  > To protect your APIs from bot threats that call your API thousands of times per hour. You should consider implementing rate limit early on.

- Setting HTTP headers appropriately can help to lock down and secure your web application. [read more...](https://github.com/helmetjs/helmet)

- Your API should convert the received data to their canonical form or reject them. Return 400 Bad Request with details about any errors from bad or missing data.

- All the data exchanged with the REST API must be validated by the API.

- Serialize your JSON.

  _Why:_

  > A key concern with JSON encoders is preventing arbitrary JavaScript remote code execution within the browser... or, if you're using node.js, on the server. It's vital that you use a proper JSON serializer to encode user-supplied data properly to prevent the execution of user-supplied input on the browser.

- Validate the content-type and mostly use `application/*json` (Content-Type header).

  _Why:_

  > For instance, accepting the `application/x-www-form-urlencoded` mime type allows the attacker to create a form and trigger a simple POST request. The server should never assume the Content-Type. A lack of Content-Type header or an unexpected Content-Type header should result in the server rejecting the content with a `4XX` response.

- Check the API Security Checklist Project. [read more...](https://github.com/shieldfy/API-Security-Checklist)

<a name="api-documentation"></a>

<a name="logging"></a>
  
  
  
