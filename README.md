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

- Clear separation of concerns with dedicated folders for Commands and Queries. Additionally, a 'Shared' folder is introduced, housing subfolders for Data Transfer Objects (Dto) and Specifications

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
|  
|     
|  

```
