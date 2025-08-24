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

### First Test
```csharp
// Code to test
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

// Unit test
// Naming Convention - ClassName_MethodName_ExpectedBehavior
public static void WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString()
{
    try
    {
        // We need Triple A -> Arrange - Act - Assert

        // Arrange -> Go get your variables, classes, functions, whatever you need.
        int num = 0; // variable 
        WorldsDimbsetFunction worldsDimbset = new WorldsDimbsetFunction(); // class

        // Act -> Execute the function.
        string result = worldsDimbset.ReturnsPikachuIfZero(num);

        // Assert -> whatever is returned is it what you want?
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

// Calling it from program.cs
WorldsDimbsetFunctionTest.WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString();

// Output:
PASSED: WorldsDimbsetFunction_ReturnsPikachuIfZero_ReturnString

```
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

