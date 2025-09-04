---
description: This file provides guidelines for writing clean, maintainable, and idiomatic C# code with a focus on functional patterns and proper abstraction.
globs: *.cs
alwaysApply: false
---

# Cursor Rules File: C# Coding Style Guide

Role Definition:
- C# Language Expert
- Software Architect
- Code Quality Specialist

## 1. Overview & General Principles

### Core Philosophy
C# code should be written to maximize readability, maintainability, and correctness while minimizing complexity and coupling. Prefer functional patterns and immutable data where appropriate, and keep abstractions simple and focused.

### Requirements
**- NEVER: Place sensitive information in the generated code (e.g. passwords, API keys, personal information, etc.)**
- Write clear, self-documenting code
- Keep abstractions simple and focused
- Minimize dependencies and coupling
- Use modern C# features appropriately
- Prefer LINQ and lambda expressions for collection operations
- Use descriptive variable and method names (e.g., `IsUserSignedIn`, `CalculateTotal`)
- Use object-oriented and functional programming patterns as appropriate
- Use C# 13+ features when appropriate (e.g., record types, pattern matching, null-coalescing assignment, collection expressions)

## 2. Project Setup & Configuration

### Enable Nullable Reference Types
Always enable nullable reference types in your project:
```xml
<PropertyGroup>
  <Nullable>enable</Nullable>
  <WarningsAsErrors>nullable</WarningsAsErrors>
</PropertyGroup>
```

### Visual Style Rules
- **DO NOT** use 'private' keyword anywhere to avoid visual noise
- Use C#'s expressive syntax (e.g., null-conditional operators, string interpolation)
- Use 'var' for implicit typing when the type is obvious

```csharp
// Good: No private keyword for members
public class OrderProcessor
{
    readonly ILogger<OrderProcessor>? _logger;
    string? _lastError;
    
    public OrderProcessor(ILogger<OrderProcessor>? logger = null)
    {
        _logger = logger;
    }

    void SomePrivateMethod() {}
}

// Good: private keyword for private classes
class OrderProcessor
{
}
```

## 3. Type System & Data Modeling

### Record vs Class Guidelines
- Prefer records for data types with value semantics
- Make classes sealed by default
- Use value objects to avoid primitive obsession

```csharp
// Good: Immutable data type with value semantics
public sealed record CustomerDto
{
    public string Name { get; init; }
    public Email Email { get; init; }
    
    public CustomerDto(string name, Email email)
    {
        Name = name;
        Email = email;
    }
}

// Good: Sealed by default
public sealed class OrderProcessor
{
    // Implementation
}

// Only unsealed when inheritance is specifically designed for
public abstract class Repository<T>
{
    // Base implementation
}
```

### Value Objects
Use strong typing with value objects to avoid primitive obsession:

```csharp
// Good: Strong typing with value objects
public sealed record OrderId
{
    public Guid Value { get; init; }
    
    public OrderId(Guid value)
    {
        Value = value;
    }
    
    public static OrderId New() => new(Guid.NewGuid());
    public static OrderId From(string value) => new(Guid.Parse(value));
}

// Avoid: Primitive types for identifiers
public class Order
{
    public Guid Id { get; set; }  // Primitive obsession
}
```

### Performance Types
Use readonly structs for small immutable value types:

```csharp
// Good: Small value type as readonly struct
public readonly struct Point
{
    public double X { get; }
    public double Y { get; }
    
    public Point(double x, double y)
    {
        X = x;
        Y = y;
    }
    
    public static Point Origin => new(0, 0);
}

// Avoid: Small value types as classes (heap allocation)
public sealed class Point
{
    public double X { get; init; }
    public double Y { get; init; }
}
```

### Enums and Constants
Use explicit backing types and proper naming:

```csharp
// Good: Explicit backing type for known range
public enum OrderStatus : byte
{
    Pending = 0,
    Processing = 1,
    Completed = 2,
    Cancelled = 3
}

// Good: Flags enum with explicit values
[Flags]
public enum Permission : int
{
    None = 0,
    Read = 1,
    Write = 2,
    Delete = 4,
    Admin = Read | Write | Delete
}

// Good: Compile-time constant
public const int MAX_RETRY_ATTEMPTS = 3;

// Good: Static readonly for runtime constants
public static readonly TimeSpan DefaultTimeout = TimeSpan.FromSeconds(30);

// Good: Static readonly for complex objects
public static readonly ImmutableHashSet<string> ValidStatuses = 
    ["pending", "processing", "completed"]; // Using collection expression
```

## 4. Language Features & Syntax

### Naming Conventions
- Use **PascalCase** for class names, method names, and public members
- Use **camelCase** for local variables and private fields
- Use **UPPERCASE** for constants
- Prefix interface names with "I" (e.g., 'IUserService')

```csharp
// Good: PascalCase for public API
public class OrderProcessor
{
    public decimal TotalAmount { get; set; }
    public bool IsProcessingComplete { get; set; }
    
    public async Task<bool> ProcessOrderAsync(OrderId orderId)
    {
        return await ValidateAndProcessOrderAsync(orderId);
    }
}

// Good: camelCase for locals and private members
public class OrderService
{
    readonly IOrderRepository orderRepository;
    string lastProcessedOrderId;
    
    public async Task ProcessAsync()
    {
        var pendingOrders = await orderRepository.GetPendingOrdersAsync();
        var processedCount = 0;
        
        foreach (var currentOrder in pendingOrders)
        {
            await ProcessSingleOrderAsync(currentOrder);
            processedCount++;
        }
    }
}

// Good: Interface naming
public interface IOrderRepository
{
    Task<Order?> GetByIdAsync(OrderId id);
    Task<IReadOnlyList<Order>> GetPendingOrdersAsync();
}
```

### Modern C# Syntax
Use var for implicit typing and modern language features:

```csharp
// Good: Obvious types
var customer = new Customer("John", "john@example.com");
var orders = GetOrdersFromDatabase();
var isValid = ValidateOrder(order);

// Good: Keep explicit types when not obvious
IOrderRepository repository = GetRepository();
decimal totalAmount = CalculateComplexTotal(orders);

// Good: Null-conditional operators
var customerName = order?.Customer?.Name ?? "Unknown";
var orderCount = customer?.Orders?.Count ?? 0;

// Good: Pattern matching with switch expressions
var discount = customer.Tier switch
{
    CustomerTier.Premium => 0.2m,
    CustomerTier.Gold => 0.1m,
    CustomerTier.Silver => 0.05m,
    _ => 0m
};

// Good: Target-typed new expressions
Order order = new(orderId, customerInfo, orderLines);
List<string> statuses = ["pending", "processing"];
```

### Collection Expressions
Always use collection expressions instead of new():

```csharp
// Good: Collection expressions
var numbers = [1, 2, 3, 4, 5];
var names = ["John", "Jane", "Bob"];
var orders = [order1, order2, order3];
var validStatuses = ["pending", "processing", "completed"];
var emptyList = <string>[];
var singleItem = [order.Id];

// Avoid: Traditional constructors
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var names = new[] { "John", "Jane", "Bob" };
var orders = new List<Order> { order1, order2, order3 };
```

### Pattern Matching
Use pattern matching effectively over nested if statements:

```csharp
// Good: Clear pattern matching
public decimal CalculateDiscount(Customer customer) =>
    customer switch
    {
        { Tier: CustomerTier.Premium } => 0.2m,
        { OrderCount: > 10 } => 0.1m,
        _ => 0m
    };

// Good: Complex pattern matching
public static string FormatOrderStatus(Order order) => order switch
{
    { Status: OrderStatus.Pending, IsRush: true } => "⚡ RUSH - Pending",
    { Status: OrderStatus.Processing, EstimatedCompletion: var eta } => $"Processing (ETA: {eta:HH:mm})",
    { Status: OrderStatus.Completed } => "✅ Completed",
    _ => order.Status.ToString()
};

// Avoid: Nested if statements
public decimal CalculateDiscount(Customer customer)
{
    if (customer.Tier == CustomerTier.Premium)
        return 0.2m;
    if (customer.OrderCount > 10)
        return 0.1m;
    return 0m;
}
```

### Range Indexers
Prefer range indexers over LINQ when appropriate:

```csharp
// Good: Using range indexers with clear comments
var lastItem = items[^1];  // ^1 means "1 from the end"
var firstThree = items[..3];  // ..3 means "take first 3 items"
var slice = items[2..5];  // take items from index 2 to 4 (5 exclusive)

// Avoid: Using LINQ when range indexers are clearer
var lastItem = items.LastOrDefault();
var firstThree = items.Take(3).ToList();
var slice = items.Skip(2).Take(3).ToList();
```

### Boolean Logic
Do not use ! in true/false checks as it can be easily missed:

```csharp
// Good: Clear true/false checks
if (isActive == false)
{
    // Do something when not active
}

// Avoid: Using ! for negation
if (!isActive)
{
    // Do something when not active
}
```

### Symbol References
Always use nameof operator for refactor-safe references:

```csharp
// Good: Using nameof for parameter names
public void ProcessOrder(Order order)
{
    if (order is null)
        throw new ArgumentNullException(nameof(order));
}

// Good: Using nameof in logging
public class OrderProcessor
{
    readonly ILogger<OrderProcessor> _logger;
    
    public async Task ProcessAsync(Order order)
    {
        _logger.LogInformation(
            "Starting {Method} for order {OrderId}",
            nameof(ProcessAsync),
            order.Id);
            
        try
        {
            await ProcessInternalAsync(order);
        }
        catch (Exception ex)
        {
            _logger.LogError(
                ex,
                "Error in {Method} for {Property} {Value}",
                nameof(ProcessAsync),
                nameof(order.Id),
                order.Id);
            throw;
        }
    }
}

// Avoid: Hard-coded string references
public void ProcessOrder(Order order)
{
    if (order is null)
        throw new ArgumentNullException("order"); // Breaks refactoring
}
```

## 5. Code Structure & Organization

### Functional Patterns
Prefer pure methods and separate state from behavior:

```csharp
// Good: Pure function
public static decimal CalculateTotalPrice(
    IEnumerable<OrderLine> lines,
    decimal taxRate) =>
    lines.Sum(line => line.Price * line.Quantity) * (1 + taxRate);

// Good: Behavior separate from state
public sealed record Order
{
    public OrderId Id { get; init; }
    public List<OrderLine> Lines { get; init; }
    
    public Order(OrderId id, List<OrderLine> lines)
    {
        Id = id;
        Lines = lines;
    }
}

public static class OrderOperations
{
    public static decimal CalculateTotal(Order order) =>
        order.Lines.Sum(line => line.Price * line.Quantity);
}

// Avoid: Method with side effects
public void CalculateAndUpdateTotalPrice()
{
    this.Total = this.Lines.Sum(l => l.Price * l.Quantity);
    this.UpdateDatabase();
}
```

### Extension Methods
Use extension methods appropriately for domain-specific operations:

```csharp
// Good: Extension method for domain-specific operations
public static class OrderExtensions
{
    public static bool CanBeFulfilled(this Order order, Inventory inventory) =>
        order.Lines.All(line => inventory.HasStock(line.ProductId, line.Quantity));
}
```

### Dependency Management
Minimize constructor injection and prefer composition:

```csharp
// Good: Minimal dependencies
public sealed class OrderProcessor
{
    readonly IOrderRepository _repository;
    
    public OrderProcessor(IOrderRepository repository)
    {
        _repository = repository;
    }
}

// Good: Composition with interfaces
public sealed class EnhancedLogger : ILogger
{
    readonly ILogger _baseLogger;
    readonly IMetrics _metrics;
    
    public EnhancedLogger(ILogger baseLogger, IMetrics metrics)
    {
        _baseLogger = baseLogger;
        _metrics = metrics;
    }
}

// Avoid: Too many dependencies
public class OrderProcessor
{
    public OrderProcessor(
        IOrderRepository repository,
        ILogger logger,
        IEmailService emailService,
        IMetrics metrics,
        IValidator validator)
    {
        // Too many dependencies indicates possible design issues
    }
}
```

### Generic Constraints
Use meaningful generic constraints:

```csharp
// Good: Constrained generics
public static T ProcessEntity<T>(T entity) 
    where T : class, IValidatable, new()
{
    entity.Validate();
    return entity;
}

// Good: Struct constraint for value types
public static bool IsDefault<T>(T value) where T : struct =>
    EqualityComparer<T>.Default.Equals(value, default);

// Good: Multiple constraints
public static TResult Transform<TInput, TResult>(TInput input)
    where TInput : class, IConvertible
    where TResult : class, new()
{
    var result = new TResult();
    // Transform logic
    return result;
}
```

### Testing Considerations
Design for testability using pure functions:

```csharp
// Good: Easy to test pure functions
public static class PriceCalculator
{
    public static decimal CalculateDiscount(
        decimal price,
        int quantity,
        CustomerTier tier) =>
        // Pure calculation
}

// Avoid: Hard to test due to hidden dependencies
public decimal CalculateDiscount()
{
    var user = _userService.GetCurrentUser();  // Hidden dependency
    var settings = _configService.GetSettings(); // Hidden dependency
    // Calculation
}
```

## 6. Performance & Memory Management

### String Performance
Optimize string operations for performance:

```csharp
// Good: String interpolation
var message = $"Order {orderId} processed at {timestamp:yyyy-MM-dd}";

// Good: StringBuilder for multiple operations
var builder = new StringBuilder(capacity: 256);
foreach (var item in items)
{
    builder.AppendLine($"Item: {item.Name} - {item.Price:C}");
}
return builder.ToString();

// Avoid: String concatenation
var message = "Order " + orderId + " processed at " + timestamp.ToString("yyyy-MM-dd");

// Avoid: Multiple string concatenations
var result = string.Empty;
foreach (var item in items)
{
    result += $"Item: {item.Name} - {item.Price:C}\n"; // Creates new string each time
}
```

### Span<T> and Memory<T> for High-Performance
Use Span<T> and Memory<T> for hot-path operations:

```csharp
// Good: Zero-allocation string parsing
public static bool TryParseOrderId(ReadOnlySpan<char> input, out OrderId result)
{
    result = default;
    if (input.Length != 36) return false;
    
    return Guid.TryParse(input, out var guid) && 
           (result = new OrderId(guid)) != default;
}

// Good: Efficient array slicing without allocation
public static decimal CalculateSum(ReadOnlySpan<decimal> values)
{
    decimal sum = 0;
    foreach (var value in values)
        sum += value;
    return sum;
}

// Good: Memory<T> for async scenarios
public async Task ProcessDataAsync(Memory<byte> buffer)
{
    await ProcessChunkAsync(buffer[..100]);  // First 100 bytes
    await ProcessChunkAsync(buffer[100..]);  // Remaining bytes
}

// Good: Span-based string splitting for performance
[MethodImpl(MethodImplOptions.AggressiveInlining)]
public static void ParseOrderData(ReadOnlySpan<char> data, List<OrderLine> lines)
{
    var remaining = data;
    while (remaining.Length > 0)
    {
        var commaIndex = remaining.IndexOf(',');
        var line = commaIndex >= 0 ? remaining[..commaIndex] : remaining;
        
        if (TryParseOrderLine(line, out var orderLine))
            lines.Add(orderLine);
        
        remaining = commaIndex >= 0 ? remaining[(commaIndex + 1)..] : ReadOnlySpan<char>.Empty;
    }
}
```

### Collection Performance
Choose appropriate collection types and avoid unnecessary allocations:

```csharp
// Good: List<T> for frequent additions/random access
var orders = new List<Order>(capacity: expectedCount);

// Good: HashSet<T> for unique items and fast lookups
var processedIds = new HashSet<OrderId>();

// Good: Dictionary<K,V> for key-value lookups
var orderCache = new Dictionary<OrderId, Order>();

// Good: ImmutableArray<T> for small, frequently accessed collections
var statusCodes = ImmutableArray.Create(200, 201, 204);

// Good: Work with IEnumerable when possible
public static bool HasValidOrders(IEnumerable<Order> orders) =>
    orders.Any(o => o.IsValid);

// Avoid: Unnecessary materialization
public static bool HasValidOrders(IEnumerable<Order> orders) =>
    orders.ToList().Any(o => o.IsValid); // Unnecessary allocation
```

### LINQ Performance
Use LINQ efficiently and prefer lambda expressions:

```csharp
// Good: Single enumeration
var validOrders = orders.Where(o => o.IsValid).ToList();
var count = validOrders.Count;
var firstOrder = validOrders.FirstOrDefault();

// Good: Count property when available
if (list.Count > 0) // O(1)

// Good: Any() for existence checks
if (orders.Any(o => o.IsRush)) // Stops at first match

// Good: LINQ with lambda expressions
var expensiveOrders = orders
    .Where(o => o.Total > 1000)
    .OrderByDescending(o => o.Total)
    .Select(o => new { o.Id, o.Total, o.Customer.Name });

// Good: Complex filtering with LINQ
var rushOrdersByStatus = orders
    .Where(IsRushOrder)
    .GroupBy(o => o.Status)
    .ToDictionary(g => g.Key, g => g.ToList());

static bool IsRushOrder(Order order) => order.IsRush && order.Priority > 5;

// Avoid: Multiple enumeration
var validOrders = orders.Where(o => o.IsValid);
var count = validOrders.Count(); // Enumerates again
var firstOrder = validOrders.FirstOrDefault(); // Enumerates again

// Avoid: Count() method for collections
if (list.Count() > 0) // O(n) for IEnumerable

// Avoid: Count() for existence checks
if (orders.Count(o => o.IsRush) > 0) // Counts all matches
```

### Hot Path Optimization
Use AggressiveInlining and optimize frequently used methods:

```csharp
// Good: Inline small methods in hot paths
[MethodImpl(MethodImplOptions.AggressiveInlining)]
public static bool IsValidOrderId(Guid id) => id != Guid.Empty;

[MethodImpl(MethodImplOptions.AggressiveInlining)]
public static decimal CalculateTax(decimal amount, decimal rate) => amount * rate;

// Good: Reuse arrays/buffers
static readonly char[] Separators = [',', ';', '|'];

public string[] SplitOrderData(string data) =>
    data.Split(Separators, StringSplitOptions.RemoveEmptyEntries);

// Good: Use ArrayPool for temporary arrays
public void ProcessLargeData(ReadOnlySpan<byte> data)
{
    var buffer = ArrayPool<byte>.Shared.Rent(1024);
    try
    {
        // Process with buffer
    }
    finally
    {
        ArrayPool<byte>.Shared.Return(buffer);
    }
}
```

### Immutable Collections
Use System.Collections.Immutable effectively:

```csharp
// Good: Immutable collections in records
public sealed record Order
{
    public OrderId Id { get; init; }
    public ImmutableList<OrderLine> Lines { get; init; }
    public ImmutableDictionary<string, string> Metadata { get; init; }
    
    public Order(OrderId id, ImmutableList<OrderLine> lines, ImmutableDictionary<string, string> metadata)
    {
        Id = id;
        Lines = lines;
        Metadata = metadata;
    }
}

// Good: Using builder pattern
var builder = ImmutableList.CreateBuilder<OrderLine>();
foreach (var line in lines)
{
    builder.Add(line);
}
return new Order(id, builder.ToImmutable());

// Good: Using collection initializer
return new Order(
    id,
    lines.ToImmutableList(),
    metadata.ToImmutableDictionary());
```

### Resource Management
Implement proper disposal patterns:

```csharp
// Good: Proper disposal pattern
public sealed class OrderProcessor : IDisposable
{
    readonly HttpClient _httpClient;
    bool _disposed;
    
    public OrderProcessor()
    {
        _httpClient = new HttpClient();
    }
    
    public void Dispose()
    {
        if (_disposed) return;
        
        _httpClient?.Dispose();
        _disposed = true;
    }
}

// Better: Use using declarations
public async Task ProcessOrdersAsync(IEnumerable<OrderId> orderIds)
{
    using var client = new HttpClient();
    foreach (var id in orderIds)
    {
        await ProcessOrderAsync(id, client);
    }
}
```

## 7. Error Handling & Validation

### Result Types vs Exceptions
- Use Result types for expected failures from Six.Tookit.Result library
- Use exceptions for exceptional cases, not for control flow

```csharp
// Good: Explicit error handling with Result types
public sealed record Result<T>
{
    public T? Value { get; }
    public Error? Error { get; }
    
    Result(T value) => Value = value;
    Result(Error error) => Error = error;
    
    public static Result<T> Success(T value) => new(value);
    public static Result<T> Failure(Error error) => new(error);
}

// Good: Exception for truly exceptional case
public static OrderId From(string value)
{
    if (!Guid.TryParse(value, out var guid))
        throw new ArgumentException("Invalid OrderId format", nameof(value));
    
    return new OrderId(guid);
}
```

### Safe Operations
Use Try methods for safer operations:

```csharp
// Good: Using TryGetValue for dictionary access
if (dictionary.TryGetValue(key, out var value))
{
    // Use value safely here
}
else
{
    // Handle missing key case
}

// Good: Using Uri.TryCreate for URL parsing
if (Uri.TryCreate(urlString, UriKind.Absolute, out var uri))
{
    // Use uri safely here
}
else
{
    // Handle invalid URL case
}

// Good: Combining Try methods with null coalescing
var value = dictionary.TryGetValue(key, out var result)
    ? result
    : defaultValue;

// Good: Using Try methods in LINQ with pattern matching
var validNumbers = strings
    .Select(s => (Success: int.TryParse(s, out var num), Value: num))
    .Where(x => x.Success)
    .Select(x => x.Value);

// Avoid: Direct indexing which can throw
var value = dictionary[key];  // Throws if key doesn't exist

// Avoid: Exception handling for expected cases
try
{
    var price = decimal.Parse(priceString);
    // Process price
}
catch (FormatException)
{
    // Handle invalid format
}
```

### Guard Clauses and Validation
Implement guard clauses at method start using modern C# features:

```csharp
// Good: Early validation with modern C# 11+ methods
public decimal CalculateDiscount(decimal price, int quantity, CustomerTier tier)
{
    ArgumentOutOfRangeException.ThrowIfNegativeOrZero(price);
    ArgumentOutOfRangeException.ThrowIfNegativeOrZero(quantity);
    
    return tier switch
    {
        CustomerTier.Premium => price * 0.2m,
        CustomerTier.Gold => price * 0.1m,
        _ => 0m
    };
}

// Good: Use ArgumentNullException.ThrowIfNull (C# 11+)
public void ProcessOrder(Order order)
{
    ArgumentNullException.ThrowIfNull(order);
    ArgumentException.ThrowIfNullOrEmpty(order.Id?.Value.ToString());
    
    // Process order
}
```

### Nullability Handling
Use proper null checking patterns:

```csharp
// Good: Explicit nullability
public class OrderProcessor
{
    readonly ILogger<OrderProcessor>? _logger;
    string? _lastError;
    
    public OrderProcessor(ILogger<OrderProcessor>? logger = null)
    {
        _logger = logger;
    }
}

// Good: Using is null for null checks
public void ProcessOrder(Order? order)
{
    if (order is null)
        throw new ArgumentNullException(nameof(order));
    
    // Process order
}

// Good: Using is not null for non-null checks
public void LogOrder(Order? order)
{
    if (order is not null)
        _logger.LogInformation("Processing order {Id}", order.Id);
}

// Good: Using pattern matching for null checks
public decimal CalculateTotal(Order? order) =>
    order switch
    {
        null => throw new ArgumentNullException(nameof(order)),
        { Lines: null } => throw new ArgumentException("Order lines cannot be null", nameof(order)),
        _ => order.Lines.Sum(l => l.Total)
    };

// Good: Use null-forgiving operator when appropriate
public class OrderValidator
{
    readonly IValidator<Order> _validator;
    
    public OrderValidator(IValidator<Order> validator)
    {
        _validator = validator ?? throw new ArgumentNullException(nameof(validator));
    }
    
    public ValidationResult Validate(Order order)
    {
        // We know _validator can't be null due to constructor check
        return _validator!.Validate(order);
    }
}
```

### Nullability Attributes
Use nullability attributes for better API contracts:

```csharp
public class StringUtilities
{
    // Output is non-null if input is non-null
    [return: NotNullIfNotNull(nameof(input))]
    public static string? ToUpperCase(string? input) =>
        input?.ToUpperInvariant();
    
    // Method never returns null
    [return: NotNull]
    public static string EnsureNotNull(string? input) =>
        input ?? string.Empty;
    
    // Parameter must not be null when method returns true
    public static bool TryParse(string? input, [NotNullWhen(true)] out string? result)
    {
        result = null;
        if (string.IsNullOrEmpty(input))
            return false;
            
        result = input;
        return true;
    }
}

// Good: Document nullability in interfaces
public interface IOrderRepository
{
    // Explicitly shows that null is a valid return value
    Task<Order?> FindByIdAsync(OrderId id, CancellationToken ct = default);
    
    // Method will never return null
    [return: NotNull]
    Task<IReadOnlyList<Order>> GetAllAsync(CancellationToken ct = default);
    
    // Parameter cannot be null
    Task SaveAsync([NotNull] Order order, CancellationToken ct = default);
}
```

## 8. Asynchronous Programming

### Async Best Practices
Follow async/await patterns correctly:

```csharp
// Good: Async method returns Task
public async Task ProcessOrderAsync(Order order)
{
    await _repository.SaveAsync(order);
}

// Good: Return pre-computed value
public Task<int> GetDefaultQuantityAsync() =>
    Task.FromResult(1);

// Better: Use ValueTask for zero allocations
public ValueTask<int> GetDefaultQuantityAsync() =>
    new ValueTask<int>(1);

// Avoid: Async void can crash your application
public async void ProcessOrder(Order order)
{
    await _repository.SaveAsync(order);
}

// Avoid: Unnecessary thread pool usage
public Task<int> GetDefaultQuantityAsync() =>
    Task.Run(() => 1);
```

### CancellationToken Flow
Always flow CancellationToken through async calls:

```csharp
// Good: Propagate cancellation
public async Task<Order> ProcessOrderAsync(
    OrderRequest request,
    CancellationToken cancellationToken)
{
    var order = await _repository.GetAsync(
        request.OrderId, 
        cancellationToken);
        
    await _processor.ProcessAsync(
        order, 
        cancellationToken);
        
    return order;
}

// Good: Proper disposal of CancellationTokenSource
public async Task<Order> GetOrderWithTimeout(OrderId id)
{
    using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(30));
    return await _repository.GetAsync(id, cts.Token);
}
```

### Async Anti-Patterns
Avoid common async pitfalls:

```csharp
// Good: Using await
public async Task<Order> ProcessOrderAsync(OrderId id)
{
    var order = await _repository.GetAsync(id);
    await _validator.ValidateAsync(order);
    return order;
}

// Good: Async all the way
public async Task<Order> GetOrderAsync(OrderId id)
{
    return await _repository.GetAsync(id);
}

// Good: Using async/await
public async Task<Order> ProcessOrderAsync(OrderRequest request)
{
    await _validator.ValidateAsync(request);
    var order = await _factory.CreateAsync(request);
    return order;
}

// Avoid: Using ContinueWith
public Task<Order> ProcessOrderAsync(OrderId id)
{
    return _repository.GetAsync(id)
        .ContinueWith(t => 
        {
            var order = t.Result; // Can deadlock
            return _validator.ValidateAsync(order);
        });
}

// Avoid: Blocking on async code
public Order GetOrder(OrderId id)
{
    return _repository.GetAsync(id).Result; // Can deadlock
}

// Avoid: Manual task composition
public Task<Order> ProcessOrderAsync(OrderRequest request)
{
    return _validator.ValidateAsync(request)
        .ContinueWith(t => _factory.CreateAsync(request))
        .Unwrap();
}
```

### TaskCompletionSource
Use TaskCompletionSource correctly to avoid deadlocks:

```csharp
// Good: Using RunContinuationsAsynchronously
readonly TaskCompletionSource<Order> _tcs = 
    new(TaskCreationOptions.RunContinuationsAsynchronously);

// Avoid: Default TaskCompletionSource can cause deadlocks
readonly TaskCompletionSource<Order> _tcs = new();
```

## 9. Documentation Standards

Write extensive technical XML doc comments for public APIs:

### Documentation Rules
- Be technical, detailed but straight to the point
- **DO NOT** write XML documentation comments for private members, methods, etc.
- **DO NOT** write XML documentation comments if there are XML doc comments already applied but rewrite if they are not valid anymore
- For each top level class, record, struct, enum, and similar write extensive documentation what this type is for and how to use it
- For properties, write brief and concise XML documentation of what this property is for and how to use it. Do not overdocument.
- **DO NOT** by ANY MEANS change APIs, property, method names, etc. unless explicitly requested by the user
- **DO NOT** write XML documentation comments for members implementing interfaces and abstract classes, they are already documented in the interface
- **DO NOT** write XML documentation comments for overriden members, they are already documented in the base class

```csharp
/// <summary>
/// Represents a unique identifier for an order in the system.
/// Provides type safety over using raw Guid values and includes
/// factory methods for common creation scenarios.
/// </summary>
/// <remarks>
/// This value object prevents primitive obsession and provides
/// a clear semantic meaning when used in method signatures.
/// </remarks>
public sealed record OrderId
{
    /// <summary>
    /// Gets the underlying Guid value for this order identifier.
    /// </summary>
    public Guid Value { get; init; }
    
    /// <summary>
    /// Initializes a new instance of OrderId with the specified Guid value.
    /// </summary>
    /// <param name="value">The Guid value to wrap.</param>
    /// <exception cref="ArgumentException">Thrown when value is Guid.Empty.</exception>
    public OrderId(Guid value)
    {
        ArgumentException.ThrowIfEqual(value, Guid.Empty);
        Value = value;
    }
    
    /// <summary>
    /// Creates a new OrderId with a randomly generated Guid.
    /// </summary>
    /// <returns>A new OrderId instance with a unique identifier.</returns>
    public static OrderId New() => new(Guid.NewGuid());
}
```

---

# End of C# Coding Style Guide