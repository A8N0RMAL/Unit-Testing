# Unit Testing in C#:

## 🎯 1. Intro & First Test
<img width="1919" height="605" alt="image" src="https://github.com/user-attachments/assets/b0d6eb4d-3c9d-4760-ade8-e09eeb46252e" />

## What is Unit Testing?
Unit testing is a software testing method where individual components (units) of code are tested in isolation to verify they work as expected. In C#, a "unit" typically refers to a single method, class, or component.

## Why Unit Test?
- **Catch Bugs Early**: Identify issues during development rather than in production
- **Enable Refactoring**: Make code changes with confidence
- **Document Behavior**: Tests serve as living documentation of your code
- **Improve Design**: Writing testable code often leads to better architecture
- **Reduce Regression**: Ensure new changes don't break existing functionality

## First Test

### Production Code

```csharp
public class WorldsDimbsetFunction
{
    public string ReturnsPikachuIfZero(int num)
    {
        if (num == 0)
            return "PIKACHU!";
        else
            return "Squirtle";
    }
}
```

### Test Code
```csharp
// Naming Convention - ClassName_MethodName_ExpectedBehavior
public static void WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString()
{
    try
    {
        // Triple A Pattern -> Arrange - Act - Assert

        // Arrange -> Get variables, classes, functions
        int num = 0; // variable 
        WorldsDimbsetFunction worldsDimbset = new WorldsDimbsetFunction(); // class

        // Act -> Execute the function
        string result = worldsDimbset.ReturnsPikachuIfZero(num);

        // Assert -> Verify the returned value
        if(result == "PIKACHU!")
            Console.WriteLine("PASSED: WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString");
        else
            Console.WriteLine("FAILED: WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString");
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex);
    }
}
```

### Calling the Test
```csharp
// In program.cs
WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString();
```

### Output
```
PASSED: WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString
```
This example demonstrates a basic unit test implementation using the Arrange-Act-Assert pattern without any external testing frameworks.

---

## 🎯 2. Unit Testing with xUnit and Fluent Assertions

## Overview

This project demonstrates unit testing in C# using the **xUnit** testing framework and **Fluent Assertions** library. The example tests a `NetworkService` class that simulates network operations.

### 📦Required NuGet Packages:

To set up this testing environment, you need to install these NuGet packages:

### Main Testing Framework
```xml
<PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.8.0" />
<PackageReference Include="xunit" Version="2.6.3" />
<PackageReference Include="xunit.runner.visualstudio" Version="2.5.4" />
```

### Assertions Library
```xml
<PackageReference Include="FluentAssertions" Version="6.12.0" />
```

### Install via .NET CLI
```bash
dotnet add package Microsoft.NET.Test.Sdk
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
dotnet add package FluentAssertions
```

## Code Structure

### Production Code
```csharp
namespace NetworkUtility.Ping
{
    public class NetworkService
    {
        public string SendPing()
        {
            return "Success: Ping Sent!";
        }

        public int PingTimeout(int a, int b)
        {
            return a + b;
        }
    }
}
```

### Test Code
```csharp
using FluentAssertions;
using NetworkUtility.Ping;

namespace NetworkUtility.Tests.PingTests
{
    public class NetworkServiceTest
    {
        [Fact]
        public void NetworkService_SendPing_ReturnString()
        {
            // Arrange
            var pingSendService = new NetworkService();

            // Act
            var result = pingSendService.SendPing();

            // Assert
            result.Should().NotBeNullOrWhiteSpace();
            result.Should().Be("Success: Ping Sent!");
            result.Should().Contain("Success", Exactly.Once());
        }

        [Theory]
        [InlineData(3, 4, 7)]
        [InlineData(6, 4, 10)]
        public void NetworkService_PingTimeout_ReturnInt(int a, int b, int expected)
        {
            // Arrange
            var pingTimeoutService = new NetworkService();

            // Act
            var result = pingTimeoutService.PingTimeout(a, b);

            // Assert
            result.Should().Be(expected);
            result.Should().BeGreaterThanOrEqualTo(8);
            result.Should().NotBeInRange(-100, 6);
        }
    }
}
```

## Test Results Analysis
<img width="1503" height="354" alt="image" src="https://github.com/user-attachments/assets/5f19da1e-a15d-4c7e-9681-834bfd79ccf2" />

Based on the test output:

### Test Summary
- **Total Tests**: 3
- **Passed**: 2
- **Failed**: 1
- **Skipped**: 0
- **Duration**: 280 ms

### Detailed Results

| Test Name | Status | Duration | Error Message |
|-----------|--------|----------|---------------|
| `NetworkService_SendPing_ReturnString` | Passed | 2 ms | - |
| `NetworkService_PingTimeout_ReturnInt` (with data 6,4,10) | Passed | 14 ms | - |
| `NetworkService_PingTimeout_ReturnInt` (with data 3,4,7) | **Failed** | 108 ms | Expected result to be greater than or equal to 8, but found 7. |

### Issue Identified

The test failure shows a problem with our test logic, not the production code. The test expects all results from `PingTimeout` to be greater than or equal to 8, but when we pass (3, 4), the method correctly returns 7, which violates our test condition.

## Understanding the Test Patterns

### 1. Fact vs Theory
- **`[Fact]`**: Used for tests that don't require parameters
- **`[Theory]`**: Used for parameterized tests with `[InlineData]` attributes

### 2. Arrange-Act-Assert Pattern
- **Arrange**: Set up test objects and data
- **Act**: Execute the method being tested
- **Assert**: Verify the outcome matches expectations

### 3. Fluent Assertions
Fluent Assertions provides a more readable syntax:
```csharp
// Traditional Assert
Assert.Equal("Success: Ping Sent!", result);

// Fluent Assertions
result.Should().Be("Success: Ping Sent!");
```

## Fixing the Failing Test
The test failure indicates our test logic is incorrect, not the production code. We need to Fix the test condition:

**Fix the test condition** to pass the test that equals to 7:
```csharp
// Remove or modify the problematic assertion
result.Should().Be(expected);
result.Should().BeGreaterThanOrEqualTo(7); // 7 instead of 8 
result.Should().NotBeInRange(-100, 6);
```

## Running Tests

### Visual Studio Test Explorer
1. Build your solution
2. Open Test Explorer (Test > Windows > Test Explorer)
3. Run all tests or select specific tests to run

### Command Line
```bash
dotnet test
```

### Specific Test Project
```bash
dotnet test [path-to-test-project]
```

---

## 🎯 3. Testing Objects, IEnumerable, & Dates

## Overview

This guide covers unit testing three common data types in C#: `DateTime`, custom objects, and `IEnumerable` collections using xUnit and Fluent Assertions.

## Production Code

```csharp
using System;
using System.Collections.Generic;
using System.Net.NetworkInformation;

public class NetworkService
{
    public DateTime LastPingDate()
    {
        return DateTime.Now;
    }

    public PingOptions GetPingOptions()
    {
        return new PingOptions()
        {
            DontFragment = true,
            Ttl = 1
        };
    }

    public IEnumerable<PingOptions> MostRecentPings()
    {
        IEnumerable<PingOptions> pings = new[]
        {
            new PingOptions { DontFragment = true, Ttl = 1 },
            new PingOptions { DontFragment = false, Ttl = 2 },
            new PingOptions { DontFragment = false, Ttl = 3 }
        };
        return pings;
    }
}
```

## Test Implementation

### 1. Testing DateTime

```csharp
[Fact]
public void NetworkService_LastPingDate_ReturnDate()
{
    // Arrange - No specific setup needed
    
    // Act - Execute the method
    var result = _pingService.LastPingDate();
    
    // Assert - Verify the DateTime result
    result.Should().BeAfter(1.January(2020));
    result.Should().BeBefore(30.January(2030));
}
```

**Explanation:**
- Tests that the returned date is within a reasonable range (2020-2030)
- Uses Fluent Assertions' `BeAfter()` and `BeBefore()` methods
- **Common Issue**: If the test fails with "Expected result to be before <2030-01-30>, but found <current date>", it means your system date is beyond 2030

### Test Output 
<img width="1882" height="850" alt="1  Test Date" src="https://github.com/user-attachments/assets/4a1dd953-bcc7-4f4b-a357-077c2b0cd58f" />
<img width="1895" height="894" alt="1 1 Test Date" src="https://github.com/user-attachments/assets/a3cdd75e-29f9-4d2e-b0a5-21d1ec54fd62" />



### 2. Testing Objects

```csharp
[Fact]
public void NetworkService_GetPingOptions_ReturnPingOptions()
{
    // Arrange - Create expected result
    var expectedResult = new PingOptions
    {
        DontFragment = true,
        Ttl = 1
    };
    
    // Act - Execute the method
    var result = _pingService.GetPingOptions();
    
    // Assert - Verify object properties and type
    result.Should().BeOfType<PingOptions>();
    result.Should().BeEquivalentTo(expectedResult);
    result.Ttl.Should().Be(1);
}
```

**Explanation:**
- `BeOfType<PingOptions>()` ensures the returned object is of the correct type
- `BeEquivalentTo(expectedResult)` compares object property values (structural equality)
- Individual property assertions (`Ttl.Should().Be(1)`) provide specific validation

### Test Output 
<img width="1915" height="860" alt="2  Test Object" src="https://github.com/user-attachments/assets/2dd0e873-3a1f-476c-ac13-47e475d20e81" />
<img width="1919" height="959" alt="2 1 Test Object" src="https://github.com/user-attachments/assets/40b21529-2542-4620-8af7-af9fa518f463" />
<img width="1919" height="956" alt="2 2 Test Object" src="https://github.com/user-attachments/assets/c792bfbd-f0f4-4fde-8d70-36a8b18b8ace" />


### 3. Testing IEnumerable Collections

```csharp
[Fact]
public void NetworkService_MostRecentPings_ReturnRecentPings()
{
    // Arrange - Create expected object to find in collection
    var expectedResult = new PingOptions
    {
        DontFragment = false,
        Ttl = 3
    };
    
    // Act - Execute the method
    var result = _pingService.MostRecentPings();
    
    // Assert - Verify collection contents
    result.Should().BeOfType<PingOptions[]>();
    result.Should().ContainEquivalentOf(expectedResult);
    result.Should().Contain(png => png.DontFragment == true);
}
```

**Explanation:**
- `BeOfType<PingOptions[]>()` ensures the return type is correct
- `ContainEquivalentOf(expectedResult)` checks if collection contains an object with matching properties
- `Contain(png => png.DontFragment == true)` uses a predicate to find specific elements

### Test Output 
<img width="1919" height="964" alt="3  Test Ienumerable" src="https://github.com/user-attachments/assets/85d56dda-8a36-47fb-b418-6423ed727773" />
<img width="1919" height="905" alt="3 1 Test Ienumerable" src="https://github.com/user-attachments/assets/e699cd41-5c2d-4c03-a955-f59eb9dbf1c0" />

---

## 🎯 4. Unit Testing with Mocking using FakeItEasy
### Introduction to Mocking

Mocking is a technique used in unit testing to isolate the code being tested by replacing dependencies with simulated objects (mocks). This ensures that:
- Tests run quickly and consistently
- External dependencies don't affect test results
- You can test specific scenarios without real implementations

### 📦Required NuGet Packages:
```xml
<PackageReference Include="FakeItEasy" Version="7.3.1" />
```

### Install via Package Manager Console:
```powershell
Install-Package FakeItEasy
```


## 🔍 Code Explanation

### 1. Interface Definition (`IDNS.cs`)
```csharp
public interface IDNS
{
    bool sendDNS();
}
```
- Defines a contract for DNS functionality
- Allows for dependency injection and mocking

### 2. Concrete Implementation (`DNSService.cs`)
```csharp
public class DNSService : IDNS
{
    public bool sendDNS()
    {
        return true; // Real implementation
    }
}
```
- Actual implementation that would make real DNS calls
- In production, this would contain real network logic

### 3. Main Service (`NetworkService.cs`)
```csharp
public class NetworkService
{
    private IDNS _dNS;
    
    public NetworkService(IDNS dNS)
    {
        _dNS = dNS; // Dependency injection
    }
    
    public string SendPing()
    {
        var dnsSuccess = _dNS.sendDNS(); // Using dependency

        if (dnsSuccess)
            return "Success: Ping Sent!";
        else
            return "Failed: Ping not sent!";
    }
}
```
- **Constructor Injection**: Accepts `IDNS` interface as dependency
- **Loose Coupling**: Depends on abstraction, not concrete implementation
- **Testable**: Easy to mock the DNS dependency

### 4. Unit Test Class (`NetworkServiceTest.cs`)
```csharp
public class NetworkServiceTest
{
    private readonly NetworkService _pingService;
    private readonly IDNS _dNS;
    
    public NetworkServiceTest()
    {
        // Create fake dependency
        _dNS = A.Fake<IDNS>(); // Using FakeItEasy
        
        // Inject fake dependency into service
        _pingService = new NetworkService(_dNS);
    }
    
    [Fact]
    public void NetworkService_SendPing_ReturnString()
    {
        // Arrange -> Setup mock behavior
        A.CallTo(() => _dNS.sendDNS()).Returns(true);

        // Act -> Execute the method
        var result = _pingService.SendPing();

        // Assert -> Verify results
        result.Should().NotBeNullOrWhiteSpace();
        result.Should().Be("Success: Ping Sent!");
        result.Should().Contain("Success", Exactly.Once());
    }
}
```

## 🧪 Test Execution & Debugging
### Expected Output in Autos Window:
<img width="1917" height="795" alt="Screenshot 2025-08-25 150432" src="https://github.com/user-attachments/assets/1fc05110-c657-4429-a69c-1f6037a7d809" />

## 🎯 What This Test Achieves

### **Isolation**
- The test doesn't use the real `DNSService`
- No actual network calls are made
- Tests are fast and reliable

---

## 🎯 5. ASP.NET Core MVC Controllers
## Overview

This document covers unit testing strategies for the `ClubController` using the FakeItEasy mocking framework. The tests validate controller behavior without external dependencies.

## Test Architecture

### Dependencies
- **IClubRepository**: Data access operations
- **IPhotoService**: Image handling operations
- Fake implementations isolate controller logic

### Test Class Structure
```csharp
public class ClubControllerTests
{
    private readonly ClubController _clubController;
    private readonly IClubRepository _clubRepository;
    private readonly IPhotoService _photoService;

    public ClubControllerTests()
    {
        _clubRepository = A.Fake<IClubRepository>();
        _photoService = A.Fake<IPhotoService>();
        _clubController = new ClubController(_clubRepository, _photoService);
    }
}
```

## Unit Tests

### 1. Index Action Test
```csharp
[Fact]
public void ClubController_Index_ReturnsSuccess()
{
    // Arrange
    var clubs = A.Fake<IEnumerable<Club>>();
    A.CallTo(() => _clubRepository.GetAll()).Returns(clubs);
    
    // Act
    var result = _clubController.Index();
    
    // Assert
    result.Should().BeOfType<Task<IActionResult>>();
}
```

**Test Analysis:**
- Mocks repository `GetAll()` method to return fake club collection
- Verifies controller returns expected `Task<IActionResult>` type
- No actual database calls occur during test execution

### 2. Detail Action Test
```csharp
[Fact]
public void ClubController_Detail_ReturnsSuccess()
{
    // Arrange
    var id = 1;
    var club = A.Fake<Club>();
    A.CallTo(() => _clubRepository.GetByIdAsync(id)).Returns(club);
    
    // Act
    var result = _clubController.DetailClub(id, "RunningClub");
    
    // Assert
    result.Should().BeOfType<Task<IActionResult>>();
}
```

**Test Analysis:**
- Mocks repository `GetByIdAsync()` with specific ID parameter
- Validates controller returns proper async result type
- String parameter "RunningClub" supports SEO-friendly routing

## Test Outputs

**Successful Test Execution:**
```
ClubController_Index_ReturnsSuccess [PASS]
ClubController_Detail_ReturnsSuccess [PASS]
```

**Test Failure Example:**
```
ClubController_Index_ReturnsSuccess [FAIL]
Expected type: Task<IActionResult>
Actual type: Task<ViewResult>
```

---

## 🎯 6. MVC Entity Framework with In-Memory Database
## Overview

This documentation covers unit testing strategies for the `ClubRepository` using Entity Framework Core's in-memory database provider. The tests validate data access layer operations without requiring a physical database.

## Test Architecture

### In-Memory Database Setup
```csharp
private async Task<ApplicationDbContext> GetDbContext()
{
    var options = new DbContextOptionsBuilder<ApplicationDbContext>()
        .UseInMemoryDatabase(databaseName: Guid.NewGuid().ToString())
        .Options;
    var databaseContext = new ApplicationDbContext(options);
    databaseContext.Database.EnsureCreated();
    
    // Seed test data if needed
    if(await databaseContext.Clubs.CountAsync() < 0)
    {
        for (int i = 0; i < 10; i++)
        {
            databaseContext.Clubs.Add(new Club() { /* ... */ });
            await databaseContext.SaveChangesAsync();
        }
    }
    return databaseContext;
}
```

**Key Components:**
- **UseInMemoryDatabase**: Creates isolated database instances for each test
- **Guid.NewGuid().ToString()**: Ensures unique database names to prevent test interference
- **EnsureCreated()**: Creates the database schema

## Unit Tests

### 1. Add Operation Test
```csharp
[Fact]
public async void ClubRepository_Add_ReturnsBool()
{
    // Arrange
    var club = new Club() { /* test data */ };
    var dbContext = await GetDbContext();
    var clubRepository = new ClubRepository(dbContext);

    // Act
    var result = clubRepository.Add(club);

    // Assert
    result.Should().BeTrue();
}
```

**Test Analysis:**
- Creates a new Club entity with complete data structure
- Uses isolated in-memory database context
- Verifies Add operation returns true (success)
- Tests the complete repository Save() mechanism

### 2. GetByIdAsync Operation Test
```csharp
[Fact]
public async void ClubRepository_GetByIdAsync_ReturnsClub()
{
    // Arrange
    var id = 1;
    var dbContext = await GetDbContext();
    var clubRepository = new ClubRepository(dbContext);

    // Act
    var result = clubRepository.GetByIdAsync(id);

    // Assert
    result.Should().NotBeNull();
    result.Should().BeOfType<Task<Club>>();
}
```

**Test Analysis:**
- Tests retrieval of entity by primary key
- Includes related Address data via Include() in repository
- Validates return type and non-null response

### 3. GetAll Operation Test
```csharp
[Fact]
public async void ClubRepository_GetAll_ReturnsList()
{
    // Arrange
    var dbContext = await GetDbContext();
    var clubRepository = new ClubRepository(dbContext);

    // Act
    var result = await clubRepository.GetAll();

    // Assert
    result.Should().NotBeNull();
    result.Should().BeOfType<List<Club>>();
}
```

**Test Analysis:**
- Tests complete collection retrieval
- Verifies return type as List<Club>
- Ensures method doesn't return null

### 4. Delete Operation Test
```csharp
[Fact]
public async void ClubRepository_SuccessfulDelete_ReturnsTrue()
{
    // Arrange
    var club = new Club() { /* test data */ };
    var dbContext = await GetDbContext();
    var clubRepository = new ClubRepository(dbContext);

    // Act
    clubRepository.Add(club);
    var result = clubRepository.Delete(club);
    var count = await clubRepository.GetCountAsync();

    // Assert
    result.Should().BeTrue();
    count.Should().Be(0);
}
```

**Test Analysis:**
- Tests complete CRUD cycle: Add → Delete → Verify
- Uses GetCountAsync() to confirm deletion
- Validates both operation success and data state change

### 5. GetCountAsync Operation Test
```csharp
[Fact]
public async void ClubRepository_GetCountAsync_ReturnsInt()
{
    // Arrange
    var club = new Club() { /* test data */ };
    var dbContext = await GetDbContext();
    var clubRepository = new ClubRepository(dbContext);

    // Act
    clubRepository.Add(club);
    var result = await clubRepository.GetCountAsync();

    // Assert
    result.Should().Be(1);
}
```

**Test Analysis:**
- Tests aggregate function CountAsync()
- Verifies exact count after insertion
- Validates EF Core's counting mechanism

### 6. State-Based Query Test
```csharp
[Fact]
public async void ClubRepository_GetClubsByState_ReturnsList()
{
    // Arrange
    var state = "NC";
    var club = new Club() { 
        Address = new Address() { State = "NC" } 
    };
    var dbContext = await GetDbContext();
    var clubRepository = new ClubRepository(dbContext);

    // Act
    clubRepository.Add(club);
    var result = await clubRepository.GetClubsByState(state);

    // Assert
    result.Should().NotBeNull();
    result.Should().BeOfType<List<Club>>();
    result.First().Title.Should().Be("Running Club 1");
}
```

**Test Analysis:**
- Tests LINQ Where() filtering with string comparison
- Validates related entity navigation (Address.State)
- Includes property-level assertion for precise validation

## Test Outputs

**Successful Test Execution:**
```
ClubRepository_Add_ReturnsBool [PASS]
ClubRepository_GetByIdAsync_ReturnsClub [PASS]
ClubRepository_GetAll_ReturnsList [PASS]
ClubRepository_SuccessfulDelete_ReturnsTrue [PASS]
ClubRepository_GetCountAsync_ReturnsInt [PASS]
ClubRepository_GetAllStates_ReturnsList [PASS]
ClubRepository_GetClubsByState_ReturnsList [PASS]
```

**Test Failure Example:**
```
ClubRepository_GetCountAsync_ReturnsInt [FAIL]
Expected: 1
Actual: 0
```

---
