# How to Run the Project Locally
## Prerequisites

Before you begin, ensure that you have the following installed on your machine:

- [.NET SDK](https://dotnet.microsoft.com/download)
- [Visual Studio](https://visualstudio.microsoft.com/) or [Visual Studio Code](https://code.visualstudio.com/) 
- [SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)

## Instructions

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
   
   
   
 ## Coding Structure and Guidelines 
**Folder Structure**

- The Clean Architecture project, named MSFA23, adheres to a well-defined structure with a clear separation of concerns. It organizes code into distinct layers such as Core, Migrators, Host, and Infrastructure, following a modular design that enhances maintainability and scalability.

```bash
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

- Create a 'Command' and 'Query' folder to separate command and query responsibilities in the application layer.
Introduce a 'Shared' folder in the application layer for Data Transfer Objects (DTOs) and Specifications.

```bash

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

```bash

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

```bash

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

```bash
UpdateCustomerLocation()  
```
_Why:_

  > It makes it more natural to read the source code.
  
  
- Removed unused using directives.

  _Why:_

  > To keep the code clean and improve maintainability. Unused directives can clutter the file and make it harder to identify the actual dependencies of the code.
