# Unit Testing in C#:

## 1. Intro & First Test
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

## 2. Unit Testing with xUnit and Fluent Assertions

## Overview

This project demonstrates unit testing in C# using the **xUnit** testing framework and **Fluent Assertions** library. The example tests a `NetworkService` class that simulates network operations.

## Packages Required

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

## 3. Testing Objects, IEnumerable, & Dates

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

