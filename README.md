# SonarQube Cloud Scanning: Demo CSharp Project  
With Unit Testing Coverage

---

## How to Write and Run Unit Tests

### 1. Writing Unit Tests
- All unit tests are located in the `Algorithms.Tests` project, under the `Controllers` folder.
- Each controller in the main project has a corresponding test file, e.g., `ArraysControllerTests.cs` for `ArraysController.cs`.
- Tests use the [xUnit](https://xunit.net/) framework. To add a new test:
  1. Create a new `[Fact]` or `[Theory]` method in the relevant test class.
  2. Use `Assert` statements to verify expected outcomes.

**Example:**
```csharp
[Fact]
public void MyFeature_ReturnsExpectedResult()
{
    // Arrange
    var controller = new MyController();

    // Act
    var result = controller.MyAction();

    // Assert
    Assert.Equal(expected, result);
}
```

### 2. Running Unit Tests Locally
- Ensure you have the .NET 6 SDK installed.
- Restore dependencies and run tests with coverage:
  ```sh
  dotnet restore Algorithms.sln
  dotnet test Algorithms.Tests/Algorithms.Tests.csproj --collect:"XPlat Code Coverage"
  ```
- Coverage results will be output to `Algorithms.Tests/TestResults/` as `coverage.opencover.xml`.

---

## How Code Coverage is Generated and Used by SonarScanner

### 3. Code Coverage Generation
- The `coverlet.collector` package is used to collect code coverage during test runs.
- The coverage file is generated in OpenCover format at:  
  `Algorithms.Tests/TestResults/<GUID>/coverage.opencover.xml`

### 4. SonarScanner & GitHub Actions Integration
- The workflow in `.github/workflows/main.yml` automates build, test, coverage, and SonarCloud analysis.
- **Key steps:**
  1. **Test & Coverage:**  
     Runs `dotnet test` with coverage collection and uploads the coverage artifact.
  2. **SonarCloud Analysis:**  
     Downloads the coverage artifact and runs SonarScanner, which picks up the coverage file via:
     ```
     /d:sonar.cs.opencover.reportsPaths=Algorithms.Tests/TestResults/*/coverage.opencover.xml
     ```
- The SonarCloud analysis step uses the coverage file to report test coverage in the SonarCloud dashboard.

---

## Summary of the CI/CD Flow

1. **Write tests** in `Algorithms.Tests/Controllers/`.
2. **Push code** to GitHub.
3. **GitHub Actions** will:
   - Build and test the solution.
   - Collect and upload code coverage.
   - Run SonarScanner, which uses the coverage file for analysis.

---

