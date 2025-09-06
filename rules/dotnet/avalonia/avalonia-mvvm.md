---
description: This file provides comprehensive guidelines for writing clean, maintainable Avalonia C# code with MVVM architecture, ReactiveUI patterns, and performance best practices.
fileMatchPattern: "*.cs", "*.csproj", "Program.cs", "appsettings.json"
inclusion: manual
---

# Kiro Steering File: Avalonia MVVM and C# Best Practices

**Role Definition:**
- Avalonia UI Expert
- MVVM Architecture Specialist
- ReactiveUI Expert
- Cross-Platform Desktop Application Developer

## General

### Description
Avalonia C# code should be written to maximize readability, maintainability, and performance while following MVVM patterns, proper data binding, and cross-platform compatibility. Focus on reactive programming patterns, proper resource management, and modern Avalonia UI features.

### Requirements
- **NEVER** place sensitive information in generated code (passwords, API keys, personal data)
- Use steering rules from #[[file:.kiro/steering/dotnet/csharp/csharp-coding-style.md]] for C# coding style and conventions
- Use steering rules from #[[file:.kiro/steering/dotnet/general/dotnet-testing.md]] for testing guidelines and best practices
- Follow MVVM architecture patterns
- Use reactive programming with ReactiveUI when appropriate
- Use ReactiveUI Source Generators to simplify and enhance ReactiveUI objects
- All views must implement IViewFor<TViewModel> for proper view-viewmodel binding
- Use DynamicData library for reactive collections
- Write performant and memory-efficient UI code
- Ensure cross-platform compatibility
- Use modern Avalonia UI features and controls

## MVVM Architecture

### ViewModel Structure
- Create clean and testable ViewModels:

```csharp
// Good: Clean ViewModel with ReactiveUI Source Generators
using ReactiveUI.SourceGenerators;

public partial class CustomerViewModel : ViewModelBase
{
    readonly ICustomerService _customerService;
    readonly ObservableAsPropertyHelper<bool> _isLoading;
    
    [Reactive] public partial string SearchText { get; set; } = string.Empty;
    [Reactive] public partial Customer? SelectedCustomer { get; set; }
    
    public bool IsLoading => _isLoading.Value;
    
    public CustomerViewModel(ICustomerService customerService)
    {
        _customerService = customerService;
        
        _isLoading = LoadCustomersCommand.IsExecuting
            .ToProperty(this, x => x.IsLoading);
            
        // Search functionality
        this.WhenAnyValue(x => x.SearchText)
            .Throttle(TimeSpan.FromMilliseconds(300))
            .DistinctUntilChanged()
            .Subscribe(FilterCustomers);
    }
    
    [ReactiveCommand]
    async Task LoadCustomersAsync()
    {
        var customers = await _customerService.GetAllAsync();
        Customers.Clear();
        Customers.AddRange(customers);
    }
    
    [ReactiveCommand]
    async Task DeleteCustomerAsync(Customer customer)
    {
        await _customerService.DeleteAsync(customer.Id);
        Customers.Remove(customer);
    }
    
    void FilterCustomers(string searchText)
    {
        // Implement filtering logic
    }
}
```

```csharp
// Avoid: ViewModels with business logic mixed in
public class CustomerViewModel : INotifyPropertyChanged
{
    public void SaveCustomer()
    {
        // Database logic directly in ViewModel
        using var connection = new SqlConnection(connectionString);
        // ... database operations
    }
}
```

### View-ViewModel Connection
- Properly connect Views and ViewModels:

```csharp
// Good: Clean View code-behind with IViewFor
using ReactiveUI.SourceGenerators;

[IViewFor<CustomerViewModel>]
public partial class CustomerView : UserControl
{
    public CustomerView()
    {
        InitializeComponent();
        this.WhenActivated(d => 
        {
            // Bind view to viewmodel properties
            this.Bind(ViewModel, vm => vm.SearchText, v => v.SearchTextBox.Text)
                .DisposeWith(d);
            this.OneWayBind(ViewModel, vm => vm.Customers, v => v.CustomersListBox.Items)
                .DisposeWith(d);
            this.BindCommand(ViewModel, vm => vm.LoadCustomersCommand, v => v.LoadButton)
                .DisposeWith(d);
        });
    }
    
    public CustomerView(CustomerViewModel viewModel) : this()
    {
        ViewModel = viewModel;
    }
}
```

## ReactiveUI Source Generators

### Overview
- Use ReactiveUI Source Generators to eliminate boilerplate code and automatically generate ReactiveUI objects
- Minimum Requirements: C# 12.0, Visual Studio 17.8.0, ReactiveUI 19.5.31+
- Source: https://github.com/reactiveui/ReactiveUI.SourceGenerators

### Reactive Properties
- Use `[Reactive]` attribute on partial properties (C# 13+ with Visual Studio 17.12.0+):

```csharp
// Good: Partial property with reactive attribute
using ReactiveUI.SourceGenerators;

public partial class MyReactiveClass : ReactiveObject
{
    [Reactive]
    public partial string MyProperty { get; set; }
}

// Good: Partial property with default value (C# preview)
public partial class MyReactiveClass : ReactiveObject
{
    [Reactive]
    public partial string MyProperty { get; set; } = "Default Value";
}
```

### ReactiveCommand Generation
- Use `[ReactiveCommand]` attribute to generate commands automatically:

```csharp
// Good: Various ReactiveCommand patterns
using ReactiveUI.SourceGenerators;

public partial class MyReactiveClass
{
    // Simple command without parameter
    [ReactiveCommand]
    void Execute() { }
    
    // Command with parameter
    [ReactiveCommand]
    void Execute(string parameter) { }
    
    // Command with return value
    [ReactiveCommand]
    string Execute(string parameter) => parameter;
    
    // Async command (Async suffix is removed from generated command)
    [ReactiveCommand]
    async Task<string> ExecuteAsync(string parameter) => 
        await Task.FromResult(parameter);
    
    // Observable return value
    [ReactiveCommand]
    IObservable<string> Execute(string parameter) => 
        Observable.Return(parameter);
    
    // Command with CancellationToken
    [ReactiveCommand]
    async Task Execute(CancellationToken token) => 
        await Task.Delay(1000, token);
    
    // Command with parameter and CancellationToken
    [ReactiveCommand]
    async Task<string> Execute(string parameter, CancellationToken token)
    {
        await Task.Delay(1000, token);
        return parameter;
    }
}
```

### ReactiveCommand with CanExecute
- Use CanExecute parameter for conditional command execution:

```csharp
using ReactiveUI.SourceGenerators;

public partial class MyReactiveClass
{
    IObservable<bool> _canExecute;

    [Reactive] partial string MyProperty1 { get; set; }
    [Reactive] partial string MyProperty2 { get; set; }

    public MyReactiveClass()
    {
        _canExecute = this.WhenAnyValue(x => x.MyProperty1, x => x.MyProperty2, 
            (x, y) => !string.IsNullOrEmpty(x) && !string.IsNullOrEmpty(y));
    }

    [ReactiveCommand(CanExecute = nameof(_canExecute))]
    void Search() { }
    
    // Command with attribute pass-through
    [ReactiveCommand(CanExecute = nameof(_canExecute))]
    [property: JsonIgnore]
    void SearchWithAttributes() { }
}
```

### ReactiveCommand with Schedulers
- Specify output schedulers for command execution:

```csharp
using ReactiveUI.SourceGenerators;

public partial class MyReactiveClass
{
    // Using ReactiveUI scheduler
    [ReactiveCommand(OutputScheduler = "RxApp.MainThreadScheduler")]
    void Execute() { }
    
    // Using custom scheduler
    IScheduler _customScheduler = new TestScheduler();
    
    [ReactiveCommand(OutputScheduler = nameof(_customScheduler))]
    void ExecuteWithCustomScheduler() { }
}
```

## IViewFor Implementation

### Automatic IViewFor Generation
- Use `[IViewFor<TViewModel>]` attribute for automatic view-viewmodel binding:

```csharp
// Good: Automatic IViewFor implementation
using ReactiveUI.SourceGenerators;

[IViewFor<CustomerViewModel>]
public partial class CustomerView : UserControl
{
    public CustomerView()
    {
        InitializeComponent();
        this.WhenActivated(d => 
        {
            // Bind view to viewmodel properties
            this.Bind(ViewModel, vm => vm.SearchText, v => v.SearchTextBox.Text)
                .DisposeWith(d);
            this.OneWayBind(ViewModel, vm => vm.Customers, v => v.CustomersListBox.Items)
                .DisposeWith(d);
            this.BindCommand(ViewModel, vm => vm.LoadCustomersCommand, v => v.LoadButton)
                .DisposeWith(d);
        });
    }
}

// For generic ViewModels, use string name
[IViewFor("MyReactiveGenericClass<int>")]
public partial class MyGenericView : UserControl
{
    // Implementation
}
```

## DynamicData for Reactive Collections

### DynamicData Integration
- Use DynamicData library for advanced reactive collection operations:

```csharp
// Good: DynamicData usage for reactive collections
using DynamicData;
using DynamicData.Binding;

public partial class CustomerViewModel : ViewModelBase
{
    readonly SourceList<Customer> _customersSource = new();
    readonly ReadOnlyObservableCollection<CustomerViewModel> _customers;
    
    [Reactive] public partial string SearchFilter { get; set; } = string.Empty;
    
    public ReadOnlyObservableCollection<CustomerViewModel> Customers => _customers;
    
    public CustomerViewModel()
    {
        // Create filtered and transformed observable collection
        _customersSource
            .Connect()
            .Filter(this.WhenAnyValue(x => x.SearchFilter)
                .Select<string, Func<Customer, bool>>(filter => customer => 
                    string.IsNullOrEmpty(filter) || 
                    customer.Name.Contains(filter, StringComparison.OrdinalIgnoreCase)))
            .Transform(customer => new CustomerViewModel(customer))
            .Sort(SortExpressionComparer<CustomerViewModel>.Ascending(vm => vm.Name))
            .ObserveOn(RxApp.MainThreadScheduler)
            .Bind(out _customers)
            .Subscribe();
    }
    
    public void AddCustomer(Customer customer)
    {
        _customersSource.Add(customer);
    }
    
    public void RemoveCustomer(Customer customer)
    {
        _customersSource.Remove(customer);
    }
}
```

## Custom Controls and UserControls

### Control Development
- Create reusable and properly structured controls:

```csharp
// Good: Custom control with proper dependency properties
public sealed class LoadingButton : Button
{
    public static readonly StyledProperty<bool> IsLoadingProperty =
        AvaloniaProperty.Register<LoadingButton, bool>(nameof(IsLoading));
        
    public static readonly StyledProperty<string> LoadingTextProperty =
        AvaloniaProperty.Register<LoadingButton, string>(
            nameof(LoadingText), 
            "Loading...");
    
    public bool IsLoading
    {
        get => GetValue(IsLoadingProperty);
        set => SetValue(IsLoadingProperty, value);
    }
    
    public string LoadingText
    {
        get => GetValue(LoadingTextProperty);
        set => SetValue(LoadingTextProperty, value);
    }
    
    protected override void OnApplyTemplate(TemplateAppliedEventArgs e)
    {
        base.OnApplyTemplate(e);
        UpdateVisualState();
    }
    
    protected override void OnPropertyChanged(AvaloniaPropertyChangedEventArgs change)
    {
        base.OnPropertyChanged(change);
        
        if (change.Property == IsLoadingProperty)
        {
            UpdateVisualState();
        }
    }
    
    void UpdateVisualState()
    {
        PseudoClasses.Set(":loading", IsLoading);
        IsEnabled = !IsLoading;
    }
}
```

## Performance and Memory Management

### Resource Disposal
- Properly manage resources and subscriptions:

```csharp
// Good: Proper disposal in ViewModels
public sealed class CustomerViewModel : ViewModelBase, IDisposable
{
    readonly CompositeDisposable _disposables = new();
    
    public CustomerViewModel()
    {
        // Add subscriptions to disposables
        this.WhenAnyValue(x => x.SearchText)
            .Subscribe(OnSearchTextChanged)
            .DisposeWith(_disposables);
    }
    
    public void Dispose()
    {
        _disposables?.Dispose();
    }
}
```

## Converters and Behaviors

### Value Converters
- Create reusable and efficient converters:

```csharp
// Good: Efficient and reusable converter
public sealed class BooleanToVisibilityConverter : IValueConverter
{
    public static readonly BooleanToVisibilityConverter Instance = new();
    
    public object? Convert(object? value, Type targetType, object? parameter, CultureInfo culture)
    {
        if (value is bool boolValue)
        {
            var invert = parameter?.ToString()?.Equals("Invert", StringComparison.OrdinalIgnoreCase) == true;
            var result = invert ? !boolValue : boolValue;
            return result ? Visibility.Visible : Visibility.Collapsed;
        }
        
        return Visibility.Collapsed;
    }
    
    public object? ConvertBack(object? value, Type targetType, object? parameter, CultureInfo culture)
    {
        if (value is Visibility visibility)
        {
            var invert = parameter?.ToString()?.Equals("Invert", StringComparison.OrdinalIgnoreCase) == true;
            var result = visibility == Visibility.Visible;
            return invert ? !result : result;
        }
        
        return false;
    }
}
```

### Behaviors
- Use behaviors for UI interactions:

```csharp
// Good: Reusable behavior
public sealed class DoubleClickBehavior : Behavior<Control>
{
    public static readonly StyledProperty<ICommand?> CommandProperty =
        AvaloniaProperty.Register<DoubleClickBehavior, ICommand?>(nameof(Command));
        
    public static readonly StyledProperty<object?> CommandParameterProperty =
        AvaloniaProperty.Register<DoubleClickBehavior, object?>(nameof(CommandParameter));
    
    public ICommand? Command
    {
        get => GetValue(CommandProperty);
        set => SetValue(CommandProperty, value);
    }
    
    public object? CommandParameter
    {
        get => GetValue(CommandParameterProperty);
        set => SetValue(CommandParameterProperty, value);
    }
    
    protected override void OnAttached()
    {
        if (AssociatedObject is not null)
        {
            AssociatedObject.DoubleTapped += OnDoubleTapped;
        }
    }
    
    protected override void OnDetaching()
    {
        if (AssociatedObject is not null)
        {
            AssociatedObject.DoubleTapped -= OnDoubleTapped;
        }
    }
    
    void OnDoubleTapped(object? sender, TappedEventArgs e)
    {
        if (Command?.CanExecute(CommandParameter) == true)
        {
            Command.Execute(CommandParameter);
        }
    }
}
```

## Cross-Platform Considerations

### Platform-Specific Code
- Handle platform differences properly:

```csharp
// Good: Platform-specific implementations
public static class PlatformHelper
{
    public static bool IsWindows => RuntimeInformation.IsOSPlatform(OSPlatform.Windows);
    public static bool IsMacOS => RuntimeInformation.IsOSPlatform(OSPlatform.OSX);
    public static bool IsLinux => RuntimeInformation.IsOSPlatform(OSPlatform.Linux);
    
    public static void OpenUrl(string url)
    {
        try
        {
            if (IsWindows)
            {
                Process.Start(new ProcessStartInfo("cmd", $"/c start {url}") { CreateNoWindow = true });
            }
            else if (IsMacOS)
            {
                Process.Start("open", url);
            }
            else if (IsLinux)
            {
                Process.Start("xdg-open", url);
            }
        }
        catch (Exception ex)
        {
            // Handle or log exception
        }
    }
}
```

## Testing and Design-Time Support

### Design-Time Data
- Provide proper design-time support:

```csharp
// Good: Design-time ViewModel
public sealed class DesignCustomerViewModel : CustomerViewModel
{
    public DesignCustomerViewModel() : base(new DesignCustomerService())
    {
        SearchText = "Sample search";
        SelectedCustomer = new Customer 
        { 
            Name = "John Doe", 
            Email = "john@example.com" 
        };
    }
}

public sealed class DesignCustomerService : ICustomerService
{
    public Task<IEnumerable<Customer>> GetAllAsync() =>
        Task.FromResult(GenerateSampleCustomers());
        
    IEnumerable<Customer> GenerateSampleCustomers() =>
        Enumerable.Range(1, 10)
            .Select(i => new Customer 
            { 
                Id = i, 
                Name = $"Customer {i}", 
                Email = $"customer{i}@example.com" 
            });
}
```

## Error Handling and Validation

### Input Validation
- Implement proper validation:

```csharp
// Good: ViewModel with validation
public sealed class CustomerEditViewModel : ViewModelBase, INotifyDataErrorInfo
{
    readonly Dictionary<string, List<string>> _errors = new();
    
    string _name = string.Empty;
    public string Name
    {
        get => _name;
        set
        {
            this.RaiseAndSetIfChanged(ref _name, value);
            ValidateName();
        }
    }
    
    public bool HasErrors => _errors.Any();
    
    public event EventHandler<DataErrorsChangedEventArgs>? ErrorsChanged;
    
    public IEnumerable GetErrors(string? propertyName)
    {
        if (propertyName is not null && _errors.TryGetValue(propertyName, out var errors))
        {
            return errors;
        }
        return Enumerable.Empty<string>();
    }
    
    void ValidateName()
    {
        var errors = new List<string>();
        
        if (string.IsNullOrWhiteSpace(Name))
        {
            errors.Add("Name is required");
        }
        else if (Name.Length < 2)
        {
            errors.Add("Name must be at least 2 characters");
        }
        
        SetErrors(nameof(Name), errors);
    }
    
    void SetErrors(string propertyName, List<string> errors)
    {
        if (errors.Any())
        {
            _errors[propertyName] = errors;
        }
        else
        {
            _errors.Remove(propertyName);
        }
        
        ErrorsChanged?.Invoke(this, new DataErrorsChangedEventArgs(propertyName));
    }
}
```

## Dependency Injection and Services

### Service Registration
- Properly configure dependency injection:

```csharp
// Good: Service registration in Program.cs
public static class Program
{
    public static void Main(string[] args)
    {
        BuildAvaloniaApp()
            .StartWithClassicDesktopLifetime(args);
    }

    public static AppBuilder BuildAvaloniaApp()
    {
        var services = new ServiceCollection();
        ConfigureServices(services);
        
        var serviceProvider = services.BuildServiceProvider();
        
        return AppBuilder.Configure(() => new App(serviceProvider))
            .UsePlatformDetect()
            .LogToTrace();
    }
    
    static void ConfigureServices(IServiceCollection services)
    {
        // Register services
        services.AddSingleton<ICustomerService, CustomerService>();
        services.AddSingleton<IDialogService, DialogService>();
        
        // Register ViewModels
        services.AddTransient<MainWindowViewModel>();
        services.AddTransient<CustomerViewModel>();
    }
}
```

### Service Usage in ViewModels
- Use dependency injection properly:

```csharp
// Good: ViewModel with injected services
public partial class CustomerViewModel : ViewModelBase
{
    readonly ICustomerService _customerService;
    readonly IDialogService _dialogService;
    readonly ILogger<CustomerViewModel> _logger;
    
    public CustomerViewModel(
        ICustomerService customerService,
        IDialogService dialogService,
        ILogger<CustomerViewModel> logger)
    {
        _customerService = customerService;
        _dialogService = dialogService;
        _logger = logger;
    }
    
    [ReactiveCommand]
    async Task SaveCustomerAsync()
    {
        try
        {
            await _customerService.SaveAsync(SelectedCustomer);
            await _dialogService.ShowMessageAsync("Success", "Customer saved successfully");
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to save customer");
            await _dialogService.ShowErrorAsync("Error", "Failed to save customer");
        }
    }
}
```

# End of Kiro Steering File