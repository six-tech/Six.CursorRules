---
description: This file provides guidelines for writing effective, maintainable tests using xUnit. Aspire for testing and related tools.
globs: tests/Directory.Build.props, *.Tests.csproj, *.Test.*.csproj, *.cs
alwaysApply: false
---

# Cursor Rules File: Best Practices for .NET Testing

**Role Definition:**
- Test Engineer
- Quality Assurance Specialist
- CI/CD Expert

## General Overview

### Description

Tests should be reliable, maintainable, and provide meaningful coverage. Use xUnit as the primary testing framework,
with proper isolation and clear patterns for test organization and execution.

### Requirements

- Use xUnit as the testing framework
- Ensure test isolation
- Follow consistent patterns
- Maintain high code coverage
- Configure proper CI/CD test execution

## Project Setup

### Unit Test Project Configuration

Configure test projects with the detailed coverlet configuration:
```xml
<Project Sdk="Microsoft.NET.Sdk">
<PropertyGroup>
<TargetFramework>net9.0</TargetFramework>
<ImplicitUsings>enable</ImplicitUsings>
<Nullable>enable</Nullable>
<IsPackable>false</IsPackable>
<IsTestProject>true</IsTestProject>

          <!-- Coverlet Collector Configuration -->
          <UseSourceLink>true</UseSourceLink>
          <CollectCoverage>true</CollectCoverage>
          <CoverletOutputFormat>cobertura;html;json</CoverletOutputFormat>
          <CoverletOutput>$(MSBuildThisFileDirectory)coverage\</CoverletOutput>
          <CoverletOutputDirectory>$(MSBuildThisFileDirectory)coverage\</CoverletOutputDirectory>
          <MergeWith>$(MSBuildThisFileDirectory)coverage\coverage.json</MergeWith>
          <Include>**/*.cs</Include>
          <Exclude>**/*.g.cs,**/*.g.i.cs,**/*.Designer.cs,**/*.AssemblyInfo.cs</Exclude>
          <ExcludeByFile>**/bin/**/*,**/obj/**/*,**/test/**/*</ExcludeByFile>
          <ExcludeByAttribute>ExcludeFromCodeCoverage, Obsolete, GeneratedCodeAttribute, CompilerGeneratedAttribute</ExcludeByAttribute>
          <IncludeTestAssembly>false</IncludeTestAssembly>
          <SingleHit>false</SingleHit>
          <ThresholdType>line,branch,method</ThresholdType>
          <ThresholdStat>total</ThresholdStat>
          <Threshold>80</Threshold>
        </PropertyGroup>
        
        <ItemGroup>
          <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.10.0" />
          <PackageReference Include="xunit" Version="2.9.3" />
          <PackageReference Include="xunit.runner.visualstudio" Version="3.0.2" />
          <PackageReference Include="coverlet.collector" Version="6.0.2">
            <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
            <PrivateAssets>all</PrivateAssets>
          </PackageReference>
        </ItemGroup>
      </Project>
      ```

### Integration Test Project Configuration

For integration tests with services/databases, add .NET Aspire hosting:

```xml

<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>net9.0</TargetFramework>
        <ImplicitUsings>enable</ImplicitUsings>
        <Nullable>enable</Nullable>
        <IsPackable>false</IsPackable>
        <IsTestProject>true</IsTestProject>

        <!-- Coverlet Collector Configuration -->
        <UseSourceLink>true</UseSourceLink>
        <CollectCoverage>true</CollectCoverage>
        <CoverletOutputFormat>cobertura;html;json</CoverletOutputFormat>
        <CoverletOutput>$(MSBuildThisFileDirectory)coverage\</CoverletOutput>
        <CoverletOutputDirectory>$(MSBuildThisFileDirectory)coverage\</CoverletOutputDirectory>
        <MergeWith>$(MSBuildThisFileDirectory)coverage\coverage.json</MergeWith>
        <Include>**/*.cs</Include>
        <Exclude>**/*.g.cs,**/*.g.i.cs,**/*.Designer.cs,**/*.AssemblyInfo.cs</Exclude>
        <ExcludeByFile>**/bin/**/*,**/obj/**/*,**/test/**/*</ExcludeByFile>
        <ExcludeByAttribute>ExcludeFromCodeCoverage, Obsolete, GeneratedCodeAttribute, CompilerGeneratedAttribute
        </ExcludeByAttribute>
        <IncludeTestAssembly>false</IncludeTestAssembly>
        <SingleHit>false</SingleHit>
        <ThresholdType>line,branch,method</ThresholdType>
        <ThresholdStat>total</ThresholdStat>
        <Threshold>80</Threshold>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Aspire.Hosting.Testing" Version="9.3.1"/>
        <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.10.0"/>
        <PackageReference Include="xunit" Version="2.9.3"/>
        <PackageReference Include="xunit.runner.visualstudio" Version="3.0.2"/>
        <PackageReference Include="coverlet.collector" Version="6.0.2">
            <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
            <PrivateAssets>all</PrivateAssets>
        </PackageReference>
    </ItemGroup>
</Project>
```


## Test Structure and Organization

### Test Output and Logging

Use ITestOutputHelper for logging:

```csharp
public class OrderProcessingTests
{
    private readonly ITestOutputHelper _output;

    public OrderProcessingTests(ITestOutputHelper output)
    {
        _output = output;
    }

    [Fact]
    public async Task ProcessOrder_ValidOrder_Succeeds()
    {
        _output.WriteLine("Starting test with valid order");
        // Test implementation
    }
}
```

#### Test Fixtures for Shared State

Use fixtures for shared state:

```csharp
public class DatabaseFixture : IAsyncLifetime
{
    public DbConnection Connection { get; private set; }

    public async Task InitializeAsync()
    {
        Connection = new SqlConnection("connection-string");
        await Connection.OpenAsync();
    }

    public async Task DisposeAsync()
    {
        await Connection.DisposeAsync();
    }
}

public class OrderTests : IClassFixture<DatabaseFixture>
{
    private readonly DatabaseFixture _fixture;
    private readonly ITestOutputHelper _output;

    public OrderTests(DatabaseFixture fixture, ITestOutputHelper output)
    {
        _fixture = fixture;
        _output = output;
    }
}
```


## Test Patterns and Best Practices

### Test Categorization with Traits

Categorize tests using traits for better organization:

```csharp
public class OrderProcessingTests
{
    [Fact]
    [Trait("Category", "Unit")]
    public void CalculateDiscount_ValidInput_ReturnsExpectedDiscount()
    {
        // Unit test implementation
    }

    [Fact]
    [Trait("Category", "Integration")]
    public async Task ProcessOrder_WithDatabase_UpdatesOrderStatus()
    {
        // Integration test implementation
    }

    [Fact]
    [Trait("Category", "Performance")]
    public async Task ProcessOrders_HighVolume_CompletesWithinTimeout()
    {
        // Performance test implementation
    }

    [Fact]
    [Trait("Category", "Security")]
    public void ValidateOrderAccess_UnauthorizedUser_ThrowsSecurityException()
    {
        // Security test implementation
    }
}
```

#### Data-Driven Tests with Theory

Prefer Theory over multiple Facts:

```csharp
public class DiscountCalculatorTests
{
    public static TheoryData<decimal, int, decimal> DiscountTestData =>
        new()
        {
            { 100m, 1, 0m },      // No discount for single item
            { 100m, 5, 5m },      // 5% for 5 items
            { 100m, 10, 10m },    // 10% for 10 items
        };

    [Theory]
    [MemberData(nameof(DiscountTestData))]
    public void CalculateDiscount_ReturnsCorrectAmount(
        decimal price,
        int quantity,
        decimal expectedDiscount)
    {
        // Arrange
        var calculator = new DiscountCalculator();

        // Act
        var discount = calculator.Calculate(price, quantity);

        // Assert
        Assert.Equal(expectedDiscount, discount);
    }
}
```

#### Arrange-Act-Assert Pattern

Follow Arrange-Act-Assert pattern:

```csharp
[Fact]
public async Task ProcessOrder_ValidOrder_UpdatesInventory()
{
    // Arrange
    var order = new Order(
        OrderId.New(),
        new[] { new OrderLine("SKU123", 5) });
    var processor = new OrderProcessor(_mockRepository.Object);

    // Act
    var result = await processor.ProcessAsync(order);

    // Assert
    Assert.True(result.IsSuccess);
    _mockRepository.Verify(
        r => r.UpdateInventoryAsync(
            It.IsAny<string>(),
            It.IsAny<int>()),
        Times.Once);
}
```

#### Fresh Data for Each Test

Use fresh data for each test:

```csharp
public class OrderTests
{
    private static Order CreateTestOrder() =>
        new(OrderId.New(), TestData.CreateOrderLines());

    [Fact]
    public async Task ProcessOrder_Success()
    {
        var order = CreateTestOrder();
        // Test implementation
    }
}
```

#### Resource Cleanup

Clean up resources:

```csharp
public class IntegrationTests : IAsyncDisposable
{
    private readonly TestServer _server;
    private readonly HttpClient _client;

    public IntegrationTests()
    {
        _server = new TestServer(CreateHostBuilder());
        _client = _server.CreateClient();
    }

    public async ValueTask DisposeAsync()
    {
        _client.Dispose();
        await _server.DisposeAsync();
    }
}
```


## CI/CD Integration

### GitHub Actions Workflow

Configure test runs:

```yaml
- name: Test
  run: |
    dotnet test --configuration Release \
                --collect:"XPlat Code Coverage" \
                --logger:trx \
                --results-directory ./coverage

- name: Upload coverage
  uses: actions/upload-artifact@v3
  with:
    name: coverage-results
    path: coverage/**
```

#### Code Coverage Configuration

Enable code coverage:

```xml

<PropertyGroup>
    <CollectCoverage>true</CollectCoverage>
    <CoverletOutputFormat>cobertura</CoverletOutputFormat>
    <CoverletOutput>./coverage/</CoverletOutput>
    <ThresholdType>line,branch,method</ThresholdType>
    <ThresholdStat>total</ThresholdStat>
    <Threshold>80</Threshold>
</PropertyGroup>
```


## Integration Testing with .NET Aspire

### Keycloak Integration Test Example

Use .NET Aspire hosting for integration tests with services and databases:

```csharp
using Microsoft.Extensions.Logging;
using Six.KeycloakAdmin.Extensions;
using Six.KeycloakAdmin.Tests.AppHost;

namespace Six.KeycloakAdmin.Tests.Functional;

/// <summary>
/// Shared test fixture for Keycloak integration tests.
/// This fixture is created once for all tests in the collection and disposed after all tests complete.
/// It manages the lifecycle of the Keycloak container and provides the admin client.
/// </summary>
[PublicAPI]
public class KeycloakTestFixture : IAsyncLifetime
{
    static readonly TimeSpan _defaultTimeout = TimeSpan.FromSeconds(120);
    Aspire.Hosting.DistributedApplication _app = null!;

    /// <summary>
    /// Gets the Keycloak admin client for use in tests.
    /// </summary>
    internal IKeycloakAdminProvider AdminProvider { get; private set; } = null!;

    /// <summary>
    /// Initializes the test fixture by starting the Keycloak container and configuring the admin client.
    /// This method is called once before any tests in the collection run.
    /// </summary>
    /// <remarks>
    /// I didn't find a way to dynamically set the Keycloak base URL using Aspire discoverability,
    /// so we use a fixed port for the Keycloak admin client.
    /// So, the port is fixed with: <c>"DcpPublisher:RandomizePorts=false"</c>
    /// </remarks>
    public async Task InitializeAsync()
    {
        // Arrange
        var cancellationToken = new CancellationTokenSource(_defaultTimeout).Token;
        var appHost =
            await DistributedApplicationTestingBuilder.CreateAsync<Projects.Six_KeycloakAdmin_Tests_AppHost>(
                args: ["DcpPublisher:RandomizePorts=false"], // We disable port randomization for tests
                cancellationToken: cancellationToken);

        appHost.Services.AddLogging(logging =>
        {
            logging.SetMinimumLevel(LogLevel.Debug);
            logging.AddFilter(appHost.Environment.ApplicationName, LogLevel.Debug);
            logging.AddFilter("Aspire.", LogLevel.Debug);
        });

        appHost.Services.ConfigureHttpClientDefaults(clientBuilder =>
        {
            clientBuilder.AddStandardResilienceHandler();
        });

        appHost.Services.AddKeycloakAdmin(options =>
        {
            // With port randomization disabled, we can use the fixed port
            options.BaseUrl = $"http://localhost:{KeycloakAdminDefaults.Port}";
        });

        // Build and start the application
        _app = await appHost.BuildAsync(cancellationToken).WaitAsync(_defaultTimeout, cancellationToken);
        await _app.StartAsync(cancellationToken).WaitAsync(_defaultTimeout, cancellationToken);

        await _app.ResourceNotifications
            .WaitForResourceHealthyAsync(KeycloakAdminDefaults.AspireResourceName, cancellationToken)
            .WaitAsync(_defaultTimeout, cancellationToken);

        // Get the Keycloak admin service and create the KeycloakAdmin client (basic auth)
        using var scope = _app.Services.CreateScope();
        var keycloakAdminService = scope.ServiceProvider.GetRequiredService<IKeycloakAdminService>();

        AdminProvider = keycloakAdminService.GetAdminWithBasicAuth(
            KeycloakAdminDefaults.AdminUsername,
            KeycloakAdminDefaults.AdminPassword);
    }

    /// <summary>
    /// Cleans up the test fixture by disposing the Aspire application and Keycloak container.
    /// This method is called once after all tests in the collection complete.
    /// </summary>
    public async Task DisposeAsync()
    {
        await _app.DisposeAsync();
    }
}

// Using the fixture in test collections
[Collection("Keycloak Collection")]
public class KeycloakIntegrationTests
{
    readonly KeycloakTestFixture _fixture;
    readonly ITestOutputHelper _output;

    public KeycloakIntegrationTests(KeycloakTestFixture fixture, ITestOutputHelper output)
    {
        _fixture = fixture;
        _output = output;
    }

    [Fact]
    [Trait("Category", "Integration")]
    public async Task CreateRealm_ValidInput_CreatesRealmSuccessfully()
    {
        // Arrange
        var realmName = $"test-realm-{Guid.NewGuid()}";

        // Act
        await _fixture.AdminProvider.CreateRealmAsync(new RealmRepresentation
        {
            Realm = realmName,
            Enabled = true
        });

        // Assert
        var realm = await _fixture.AdminProvider.GetRealmAsync(realmName);
        Assert.NotNull(realm);
        Assert.Equal(realmName, realm.Realm);
        Assert.True(realm.Enabled);
    }
}

[CollectionDefinition("Keycloak Collection")]
public class KeycloakCollection : ICollectionFixture<KeycloakTestFixture>
{
    // This class has no code, and is never created. Its purpose is simply
    // to be the place to apply [CollectionDefinition] and all the
    // ICollectionFixture<> interfaces.
}
```

> [!NOTE]
> 
> **Benefits of .NET Aspire for Testing:**
> 
> - Tremendously simplifies both local and CI/CD testing
> - Provides consistent container orchestration
> - Built-in service discovery and health checks
> - Excellent integration with the .NET ecosystem
> - Simplified resource management and cleanup
> - Better development experience with unified tooling


## Best Practices

### Test Integrity

Never change tests to make them pass when the code output is wrong:

> [!WARNING]
> 
> **NEVER modify tests to match incorrect code behavior!**
> 
> If your tests are correctly written and the code produces the wrong output, the problem is in the code, not the tests.
> Tests are the specification - they define the expected behavior. Changing them to match broken code defeats their
> purpose.

##### Wrong Approach: Changing Tests to Match Broken Code

```csharp
// ❌ WRONG: Changing test to match broken code
[Fact]
public void CalculateTotal_WithTax_ReturnsCorrectAmount()
{
    var calculator = new TaxCalculator();
    var result = calculator.Calculate(100m, 0.1m);

    // Wrong: Changing expected value from 110m to 100m because code is broken
    Assert.Equal(100m, result); // This masks a bug in the calculator!
}
```

##### Correct Approach: Fix the Code

```csharp
// ✅ CORRECT: Keep test expectation correct, fix the code instead
[Fact]
public void CalculateTotal_WithTax_ReturnsCorrectAmount()
{
    var calculator = new TaxCalculator();
    var result = calculator.Calculate(100m, 0.1m);

    // Correct: Keep the right expected value and fix the calculator code
    Assert.Equal(110m, result); // 100 + (100 * 0.1) = 110
}
```

### Test Naming Conventions

Name tests clearly:

```csharp
// Good: Clear test names
[Fact]
public async Task ProcessOrder_WhenInventoryAvailable_UpdatesStockAndReturnsSuccess()

// Avoid: Unclear names
[Fact]
public async Task TestProcessOrder()
```

### Assertion Best Practices

Use meaningful assertions:

```csharp
// Good: Clear assertions
Assert.Equal(expected, actual);
Assert.Contains(expectedItem, collection);
Assert.Throws<OrderException>(() => processor.Process(invalidOrder));

// Avoid: Multiple assertions without context
Assert.NotNull(result);
Assert.True(result.Success);
Assert.Equal(0, result.Errors.Count);
```

### Async Operation Handling

Handle async operations properly:

```csharp
// Good: Async test method
[Fact]
public async Task ProcessOrder_ValidOrder_Succeeds()
{
    await using var processor = new OrderProcessor();
    var result = await processor.ProcessAsync(order);
    Assert.True(result.IsSuccess);
}

// Avoid: Sync over async
[Fact]
public void ProcessOrder_ValidOrder_Succeeds()
{
    using var processor = new OrderProcessor();
    var result = processor.ProcessAsync(order).Result;  // Can deadlock
    Assert.True(result.IsSuccess);
}
```

### xUnit Built-in Assertions

Use xUnit's built-in assertions:

```csharp
// Good: Using xUnit's built-in assertions
public class OrderTests
{
    [Fact]
    public void CalculateTotal_WithValidLines_ReturnsCorrectSum()
    {
        // Arrange
        var order = new Order(
            OrderId.New(),
            new[]
            {
                new OrderLine("SKU1", 2, 10.0m),
                new OrderLine("SKU2", 1, 20.0m)
            });

        // Act
        var total = order.CalculateTotal();

        // Assert
        Assert.Equal(40.0m, total);
    }

    [Fact]
    public void Order_WithInvalidLines_ThrowsException()
    {
        // Arrange
        var invalidLines = new OrderLine[] { };

        // Act & Assert
        var ex = Assert.Throws<ArgumentException>(() =>
            new Order(OrderId.New(), invalidLines));
        Assert.Equal("Order must have at least one line", ex.Message);
    }

    [Fact]
    public void Order_WithValidData_HasExpectedProperties()
    {
        // Arrange
        var id = OrderId.New();
        var lines = new[] { new OrderLine("SKU1", 1, 10.0m) };

        // Act
        var order = new Order(id, lines);

        // Assert
        Assert.NotNull(order);
        Assert.Equal(id, order.Id);
        Assert.Single(order.Lines);
        Assert.Collection(order.Lines,
            line =>
            {
                Assert.Equal("SKU1", line.Sku);
                Assert.Equal(1, line.Quantity);
                Assert.Equal(10.0m, line.Price);
            });
    }
}
```

### Avoid Third-Party Assertion Libraries

Avoid using FluentAssertions or similar libraries:

```csharp
// Avoid: Using FluentAssertions or similar libraries
public class OrderTests
{
    [Fact]
    public void CalculateTotal_WithValidLines_ReturnsCorrectSum()
    {
        var order = new Order(
            OrderId.New(),
            new[]
            {
                new OrderLine("SKU1", 2, 10.0m),
                new OrderLine("SKU2", 1, 20.0m)
            });

        // Avoid: Using FluentAssertions
        order.CalculateTotal().Should().Be(40.0m);
        order.Lines.Should().HaveCount(2);
        order.Should().NotBeNull();
    }
}
```

### Assertion Types and Categories

Use proper assertion types:

```csharp
public class CustomerTests
{
    [Fact]
    public void Customer_WithValidEmail_IsCreated()
    {
        // Boolean assertions
        Assert.True(customer.IsActive);
        Assert.False(customer.IsDeleted);

        // Equality assertions
        Assert.Equal("john@example.com", customer.Email);
        Assert.NotEqual(Guid.Empty, customer.Id);

        // Collection assertions
        Assert.Empty(customer.Orders);
        Assert.Contains("Admin", customer.Roles);
        Assert.DoesNotContain("Guest", customer.Roles);
        Assert.All(customer.Orders, o => Assert.NotNull(o.Id));

        // Type assertions
        Assert.IsType<PremiumCustomer>(customer);
        Assert.IsAssignableFrom<ICustomer>(customer);

        // String assertions
        Assert.StartsWith("CUST", customer.Reference);
        Assert.Contains("Premium", customer.Description);
        Assert.Matches("^CUST\\d{6}$", customer.Reference);

        // Range assertions
        Assert.InRange(customer.Age, 18, 100);

        // Reference assertions
        Assert.Same(expectedCustomer, actualCustomer);
        Assert.NotSame(differentCustomer, actualCustomer);
    }
}
```

### Complex Collection Assertions

Use Assert.Collection for complex collections:

```csharp
[Fact]
public void ProcessOrder_CreatesExpectedEvents()
{
    // Arrange
    var processor = new OrderProcessor();
    var order = CreateTestOrder();

    // Act
    var events = processor.Process(order);

    // Assert
    Assert.Collection(events,
        evt =>
        {
            Assert.IsType<OrderReceivedEvent>(evt);
            var received = Assert.IsType<OrderReceivedEvent>(evt);
            Assert.Equal(order.Id, received.OrderId);
        },
        evt =>
        {
            Assert.IsType<InventoryReservedEvent>(evt);
            var reserved = Assert.IsType<InventoryReservedEvent>(evt);
            Assert.Equal(order.Id, reserved.OrderId);
            Assert.NotEmpty(reserved.ReservedItems);
        },
        evt =>
        {
            Assert.IsType<OrderConfirmedEvent>(evt);
            var confirmed = Assert.IsType<OrderConfirmedEvent>(evt);
            Assert.Equal(order.Id, confirmed.OrderId);
            Assert.True(confirmed.IsSuccess);
        });
}
```

# End of Cursor Rules File 