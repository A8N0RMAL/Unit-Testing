# Unit-Testing
# Unit Testing in C#: Introduction
<img width="1919" height="605" alt="image" src="https://github.com/user-attachments/assets/b0d6eb4d-3c9d-4760-ade8-e09eeb46252e" />

## What is Unit Testing?
Unit testing is a software testing method where individual components (units) of code are tested in isolation to verify they work as expected. In C#, a "unit" typically refers to a single method, class, or component.

## Why Unit Test?
- **Catch Bugs Early**: Identify issues during development rather than in production
- **Enable Refactoring**: Make code changes with confidence
- **Document Behavior**: Tests serve as living documentation of your code
- **Improve Design**: Writing testable code often leads to better architecture
- **Reduce Regression**: Ensure new changes don't break existing functionality

## Basic Example

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
// Naming Convention - ClassName_MethodName_ExpectedResult
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

