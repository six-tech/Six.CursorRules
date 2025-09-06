---
description: This file provides comprehensive guidelines for creating Avalonia controls using pure C# code without XAML, with separate styling similar to the JS/CSS paradigm.
fileMatchPattern: "*.cs", "*.axaml"
inclusion: fileMatch
---

# Kiro Steering File: Avalonia Pure C# Controls with Separate Styling

**Role Definition:**
- Avalonia Control Developer
- UI Component Architect
- Cross-Platform Desktop Control Specialist
- Performance-Focused Developer

## General

### Description
Create Avalonia controls using pure C# code without XAML markup, following the separation of concerns principle similar to JavaScript/CSS paradigm. Controls maintain their own state and are styled separately through Avalonia's styling system. This approach provides better performance, type safety, and programmatic control over UI construction.

### Requirements
- **NEVER** place sensitive information in control code (passwords, API keys, personal data)
- Use #[[file:steering/dotnet/csharp/csharp-coding-style.md]] for C# coding style and conventions
- Create controls using pure C# code without XAML dependencies
- Separate control logic from styling completely
- Controls maintain their own state without MVVM patterns
- Use Avalonia's styling system for visual appearance
- Ensure cross-platform compatibility
- Focus on performance and memory efficiency
- Use modern C# features and patterns

## Control Architecture Principles

### Separation of Concerns
- **Control Logic**: Pure C# classes that define behavior and state
- **Visual Structure**: Programmatically created visual tree
- **Styling**: Separate style files that define appearance (when needed)
- **State Management**: Internal to the control, no external ViewModels

### Base Class Selection

Choose the appropriate base class based on styling and templating needs:

#### TemplatedControl
- Use when control needs external styling and templating
- Supports control templates and theme integration
- Best for reusable controls that need visual customization
- Requires separate style files for appearance

#### UserControl
- Use for composite controls with fixed visual structure
- Visual tree is defined in code, not externally styleable
- Good for application-specific controls that don't need theming
- Lighter weight than TemplatedControl

#### Control
- Use for completely custom controls with custom rendering
- Override OnRender for custom drawing
- Maximum performance and control over rendering
- No built-in templating or styling support

### Control Creation Patterns

#### TemplatedControl Pattern (Styleable Controls)
```csharp
// Good: TemplatedControl for styleable controls
public sealed class SearchBox : TemplatedControl
{
    // Styled properties for external styling
    public static readonly StyledProperty<string> PlaceholderProperty =
        AvaloniaProperty.Register<SearchBox, string>(nameof(Placeholder), "Search...");
    
    public static readonly StyledProperty<bool> IsLoadingProperty =
        AvaloniaProperty.Register<SearchBox, bool>(nameof(IsLoading));
    
    // Internal state properties
    string _searchText = string.Empty;
    readonly Subject<string> _searchSubject = new();
    
    public string Placeholder
    {
        get => GetValue(PlaceholderProperty);
        set => SetValue(PlaceholderProperty, value);
    }
    
    public bool IsLoading
    {
        get => GetValue(IsLoadingProperty);
        set => SetValue(IsLoadingProperty, value);
    }
    
    public string SearchText
    {
        get => _searchText;
        private set => SetAndRaise(SearchTextProperty, ref _searchText, value);
    }
    
    // Events for external communication
    public event EventHandler<string>? SearchRequested;
    public event EventHandler<string>? TextChanged;
    
    static SearchBox()
    {
        // Register default style key
        DefaultStyleKeyProperty.OverrideMetadata(typeof(SearchBox), 
            new FrameworkPropertyMetadata(typeof(SearchBox)));
    }
    
    public SearchBox()
    {
        // Set up internal behavior
        SetupSearchBehavior();
    }
    
    void SetupSearchBehavior()
    {
        _searchSubject
            .Throttle(TimeSpan.FromMilliseconds(300))
            .DistinctUntilChanged()
            .Subscribe(text => SearchRequested?.Invoke(this, text));
    }
}
```

#### UserControl Pattern (Fixed Structure Controls)
```csharp
// Good: UserControl for composite controls with fixed structure
public sealed class StatusPanel : UserControl
{
    // Public properties for configuration
    public static readonly StyledProperty<string> StatusTextProperty =
        AvaloniaProperty.Register<StatusPanel, string>(nameof(StatusText));
    
    public static readonly StyledProperty<bool> IsOnlineProperty =
        AvaloniaProperty.Register<StatusPanel, bool>(nameof(IsOnline));
    
    // Internal UI elements
    readonly TextBlock _statusTextBlock;
    readonly Ellipse _statusIndicator;
    readonly StackPanel _rootPanel;
    
    public string StatusText
    {
        get => GetValue(StatusTextProperty);
        set => SetValue(StatusTextProperty, value);
    }
    
    public bool IsOnline
    {
        get => GetValue(IsOnlineProperty);
        set => SetValue(IsOnlineProperty, value);
    }
    
    public StatusPanel()
    {
        // Create visual tree in code
        _statusIndicator = new Ellipse
        {
            Width = 12,
            Height = 12,
            Margin = new Thickness(0, 0, 8, 0)
        };
        
        _statusTextBlock = new TextBlock
        {
            VerticalAlignment = VerticalAlignment.Center
        };
        
        _rootPanel = new StackPanel
        {
            Orientation = Orientation.Horizontal,
            Children = { _statusIndicator, _statusTextBlock }
        };
        
        Content = _rootPanel;
        
        // Set up property bindings
        SetupBindings();
    }
    
    void SetupBindings()
    {
        // Bind properties to UI elements
        this.GetObservable(StatusTextProperty)
            .Subscribe(text => _statusTextBlock.Text = text);
            
        this.GetObservable(IsOnlineProperty)
            .Subscribe(isOnline => 
            {
                _statusIndicator.Fill = isOnline ? Brushes.Green : Brushes.Red;
                _statusTextBlock.Foreground = isOnline ? Brushes.Black : Brushes.Gray;
            });
    }
}
```

#### Control Pattern (Custom Rendering)
```csharp
// Good: Control for custom rendering scenarios
public sealed class CircularProgressBar : Control
{
    public static readonly StyledProperty<double> ValueProperty =
        AvaloniaProperty.Register<CircularProgressBar, double>(nameof(Value), 0.0,
            coerce: CoerceValue);
    
    public static readonly StyledProperty<double> StrokeThicknessProperty =
        AvaloniaProperty.Register<CircularProgressBar, double>(nameof(StrokeThickness), 4.0);
    
    public static readonly StyledProperty<IBrush> ProgressBrushProperty =
        AvaloniaProperty.Register<CircularProgressBar, IBrush>(nameof(ProgressBrush), Brushes.Blue);
    
    public double Value
    {
        get => GetValue(ValueProperty);
        set => SetValue(ValueProperty, value);
    }
    
    public double StrokeThickness
    {
        get => GetValue(StrokeThicknessProperty);
        set => SetValue(StrokeThicknessProperty, value);
    }
    
    public IBrush ProgressBrush
    {
        get => GetValue(ProgressBrushProperty);
        set => SetValue(ProgressBrushProperty, value);
    }
    
    static CircularProgressBar()
    {
        AffectsRender<CircularProgressBar>(ValueProperty, StrokeThicknessProperty, ProgressBrushProperty);
    }
    
    static double CoerceValue(AvaloniaObject sender, double value)
    {
        return Math.Clamp(value, 0.0, 1.0);
    }
    
    public override void Render(DrawingContext context)
    {
        var bounds = Bounds;
        var center = new Point(bounds.Width / 2, bounds.Height / 2);
        var radius = Math.Min(bounds.Width, bounds.Height) / 2 - StrokeThickness / 2;
        
        if (radius <= 0) return;
        
        // Draw background circle
        var backgroundPen = new Pen(Brushes.LightGray, StrokeThickness);
        context.DrawEllipse(null, backgroundPen, center, radius, radius);
        
        // Draw progress arc
        if (Value > 0)
        {
            var progressPen = new Pen(ProgressBrush, StrokeThickness);
            var startAngle = -90; // Start at top
            var sweepAngle = 360 * Value;
            
            var rect = new Rect(center.X - radius, center.Y - radius, radius * 2, radius * 2);
            var geometry = new ArcGeometry
            {
                Center = center,
                RadiusX = radius,
                RadiusY = radius,
                StartAngle = startAngle,
                SweepAngle = sweepAngle
            };
            
            context.DrawGeometry(null, progressPen, geometry);
        }
    }
}
```

## Advanced Control Patterns

### TemplatedControl with Complex State
```csharp
// Good: TemplatedControl with complex state management
public sealed class ProgressCard : TemplatedControl
{
    // Public styled properties for styling system
    public static readonly StyledProperty<string> TitleProperty =
        AvaloniaProperty.Register<ProgressCard, string>(nameof(Title));
    
    public static readonly StyledProperty<double> ProgressProperty =
        AvaloniaProperty.Register<ProgressCard, double>(nameof(Progress), 0.0, 
            coerce: CoerceProgress);
    
    public static readonly StyledProperty<string> StatusTextProperty =
        AvaloniaProperty.Register<ProgressCard, string>(nameof(StatusText));
    
    // Internal state
    readonly Timer _animationTimer;
    bool _isAnimating;
    
    public string Title
    {
        get => GetValue(TitleProperty);
        set => SetValue(TitleProperty, value);
    }
    
    public double Progress
    {
        get => GetValue(ProgressProperty);
        set => SetValue(ProgressProperty, value);
    }
    
    public string StatusText
    {
        get => GetValue(StatusTextProperty);
        set => SetValue(StatusTextProperty, value);
    }
    
    static ProgressCard()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(ProgressCard), 
            new FrameworkPropertyMetadata(typeof(ProgressCard)));
            
        // Property change callbacks
        ProgressProperty.Changed.AddClassHandler<ProgressCard>(OnProgressChanged);
    }
    
    public ProgressCard()
    {
        _animationTimer = new Timer(OnAnimationTick, null, Timeout.Infinite, Timeout.Infinite);
    }
    
    static double CoerceProgress(AvaloniaObject sender, double value)
    {
        return Math.Clamp(value, 0.0, 1.0);
    }
    
    static void OnProgressChanged(ProgressCard control, AvaloniaPropertyChangedEventArgs e)
    {
        control.UpdateProgressAnimation();
    }
    
    void UpdateProgressAnimation()
    {
        if (!_isAnimating)
        {
            _isAnimating = true;
            _animationTimer.Change(0, 16); // 60 FPS
        }
    }
    
    void OnAnimationTick(object? state)
    {
        Dispatcher.UIThread.Post(() =>
        {
            // Update animation state
            UpdateVisualState();
        });
    }
    
    void UpdateVisualState()
    {
        PseudoClasses.Set(":loading", Progress < 1.0);
        PseudoClasses.Set(":complete", Progress >= 1.0);
    }
    
    protected override void OnApplyTemplate(TemplateAppliedEventArgs e)
    {
        base.OnApplyTemplate(e);
        UpdateVisualState();
    }
}
```

### UserControl for Complex Composites
```csharp
// Good: UserControl for complex composite with fixed structure
public sealed class DataTable : UserControl
{
    public static readonly StyledProperty<IEnumerable> ItemsSourceProperty =
        AvaloniaProperty.Register<DataTable, IEnumerable>(nameof(ItemsSource));
    
    public static readonly StyledProperty<IList<DataColumn>> ColumnsProperty =
        AvaloniaProperty.Register<DataTable, IList<DataColumn>>(nameof(Columns));
    
    public static readonly StyledProperty<object?> SelectedItemProperty =
        AvaloniaProperty.Register<DataTable, object?>(nameof(SelectedItem));
    
    // Internal components
    ScrollViewer? _scrollViewer;
    Grid? _headerGrid;
    ItemsControl? _rowsControl;
    readonly ObservableCollection<DataColumn> _columns = new();
    
    public IEnumerable ItemsSource
    {
        get => GetValue(ItemsSourceProperty);
        set => SetValue(ItemsSourceProperty, value);
    }
    
    public IList<DataColumn> Columns
    {
        get => GetValue(ColumnsProperty);
        set => SetValue(ColumnsProperty, value);
    }
    
    public object? SelectedItem
    {
        get => GetValue(SelectedItemProperty);
        set => SetValue(SelectedItemProperty, value);
    }
    
    // Events
    public event EventHandler<SelectionChangedEventArgs>? SelectionChanged;
    public event EventHandler<DataColumnEventArgs>? ColumnClicked;
    
    static DataTable()
    {
        ItemsSourceProperty.Changed.AddClassHandler<DataTable>(OnItemsSourceChanged);
        ColumnsProperty.Changed.AddClassHandler<DataTable>(OnColumnsChanged);
    }
    
    public DataTable()
    {
        Columns = _columns;
        CreateVisualTree();
        SetupDefaultColumns();
    }
    
    void CreateVisualTree()
    {
        // Create the visual structure in code
        _headerGrid = new Grid { Classes = { "data-table-header" } };
        
        _rowsControl = new ItemsControl { Classes = { "data-table-rows" } };
        
        _scrollViewer = new ScrollViewer
        {
            Content = _rowsControl,
            HorizontalScrollBarVisibility = ScrollBarVisibility.Auto,
            VerticalScrollBarVisibility = ScrollBarVisibility.Auto
        };
        
        var mainGrid = new Grid
        {
            RowDefinitions = new RowDefinitions("Auto,*"),
            Children =
            {
                _headerGrid,
                _scrollViewer
            }
        };
        
        Grid.SetRow(_scrollViewer, 1);
        Content = mainGrid;
        
        BuildTable();
    }
    
    void BuildTable()
    {
        if (_headerGrid is null || _rowsControl is null) return;
        
        BuildHeaders();
        BuildRows();
    }
    
    void BuildHeaders()
    {
        _headerGrid!.Children.Clear();
        _headerGrid.ColumnDefinitions.Clear();
        
        foreach (var column in _columns)
        {
            _headerGrid.ColumnDefinitions.Add(new ColumnDefinition(column.Width));
            
            var headerButton = new Button
            {
                Content = column.Header,
                Classes = { "table-header" },
                [Grid.ColumnProperty] = _headerGrid.ColumnDefinitions.Count - 1
            };
            
            headerButton.Click += (s, e) => ColumnClicked?.Invoke(this, new DataColumnEventArgs(column));
            _headerGrid.Children.Add(headerButton);
        }
    }
    
    void BuildRows()
    {
        // Implementation for building data rows
        if (ItemsSource is null) return;
        
        var rowsPanel = new StackPanel();
        foreach (var item in ItemsSource)
        {
            var row = CreateRow(item);
            rowsPanel.Children.Add(row);
        }
        
        _rowsControl!.Items = new[] { rowsPanel };
    }
    
    Control CreateRow(object dataItem)
    {
        var rowGrid = new Grid();
        
        for (int i = 0; i < _columns.Count; i++)
        {
            rowGrid.ColumnDefinitions.Add(new ColumnDefinition(_columns[i].Width));
            
            var cellContent = CreateCellContent(dataItem, _columns[i]);
            cellContent.SetValue(Grid.ColumnProperty, i);
            rowGrid.Children.Add(cellContent);
        }
        
        rowGrid.Classes.Add("table-row");
        return rowGrid;
    }
    
    Control CreateCellContent(object dataItem, DataColumn column)
    {
        var value = GetPropertyValue(dataItem, column.PropertyName);
        return new TextBlock 
        { 
            Text = value?.ToString() ?? string.Empty,
            Classes = { "table-cell" }
        };
    }
    
    static void OnItemsSourceChanged(DataTable table, AvaloniaPropertyChangedEventArgs e)
    {
        table.BuildRows();
    }
    
    static void OnColumnsChanged(DataTable table, AvaloniaPropertyChangedEventArgs e)
    {
        table.BuildTable();
    }
    
    void SetupDefaultColumns()
    {
        // Setup default columns if none provided
    }
    
    static object? GetPropertyValue(object obj, string propertyName)
    {
        return obj.GetType().GetProperty(propertyName)?.GetValue(obj);
    }
}

public sealed class DataColumn
{
    public string Header { get; set; } = string.Empty;
    public string PropertyName { get; set; } = string.Empty;
    public GridLength Width { get; set; } = GridLength.Auto;
}

public sealed class DataColumnEventArgs : EventArgs
{
    public DataColumn Column { get; }
    public DataColumnEventArgs(DataColumn column) => Column = column;
}
```

## Choosing the Right Base Class

### Decision Matrix

| Scenario | Base Class | Reasoning |
|----------|------------|-----------|
| Needs external styling/theming | `TemplatedControl` | Supports control templates and style customization |
| Fixed visual structure, no theming | `UserControl` | Lighter weight, visual tree defined in code |
| Custom drawing/rendering | `Control` | Override OnRender for maximum performance |
| Simple data display | `UserControl` | Easy to compose from existing controls |
| Reusable library control | `TemplatedControl` | Allows consumers to customize appearance |
| Application-specific control | `UserControl` | No need for external styling overhead |

### Examples by Use Case

#### When to Use TemplatedControl
```csharp
// Good: Reusable control that needs theming
public sealed class CustomButton : TemplatedControl
{
    // Styled properties for external customization
    public static readonly StyledProperty<string> TextProperty = 
        AvaloniaProperty.Register<CustomButton, string>(nameof(Text));
    
    // Control logic only - appearance defined in styles
    static CustomButton()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(CustomButton), 
            new FrameworkPropertyMetadata(typeof(CustomButton)));
    }
}
```

#### When to Use UserControl
```csharp
// Good: Fixed structure composite control
public sealed class LoginForm : UserControl
{
    readonly TextBox _usernameBox;
    readonly TextBox _passwordBox;
    readonly Button _loginButton;
    
    public LoginForm()
    {
        // Create fixed visual structure
        _usernameBox = new TextBox { Watermark = "Username" };
        _passwordBox = new TextBox { PasswordChar = '*', Watermark = "Password" };
        _loginButton = new Button { Content = "Login" };
        
        Content = new StackPanel
        {
            Spacing = 10,
            Children = { _usernameBox, _passwordBox, _loginButton }
        };
    }
}
```

#### When to Use Control
```csharp
// Good: Custom rendering control
public sealed class Sparkline : Control
{
    public static readonly StyledProperty<IEnumerable<double>> DataProperty =
        AvaloniaProperty.Register<Sparkline, IEnumerable<double>>(nameof(Data));
    
    static Sparkline()
    {
        AffectsRender<Sparkline>(DataProperty);
    }
    
    public override void Render(DrawingContext context)
    {
        // Custom drawing logic
        var data = Data?.ToArray();
        if (data == null || data.Length < 2) return;
        
        // Draw sparkline using custom rendering
        DrawSparkline(context, data);
    }
}
```

## State Management Patterns

### Internal State Management
```csharp
// Good: Control manages its own state
public sealed class ToggleSwitch : TemplatedControl
{
    public static readonly StyledProperty<bool> IsCheckedProperty =
        AvaloniaProperty.Register<ToggleSwitch, bool>(nameof(IsChecked));
    
    public static readonly StyledProperty<string> OnTextProperty =
        AvaloniaProperty.Register<ToggleSwitch, string>(nameof(OnText), "ON");
    
    public static readonly StyledProperty<string> OffTextProperty =
        AvaloniaProperty.Register<ToggleSwitch, string>(nameof(OffText), "OFF");
    
    // Internal animation state
    bool _isAnimating;
    double _animationProgress;
    readonly Stopwatch _animationTimer = new();
    
    public bool IsChecked
    {
        get => GetValue(IsCheckedProperty);
        set => SetValue(IsCheckedProperty, value);
    }
    
    public string OnText
    {
        get => GetValue(OnTextProperty);
        set => SetValue(OnTextProperty, value);
    }
    
    public string OffText
    {
        get => GetValue(OffTextProperty);
        set => SetValue(OffTextProperty, value);
    }
    
    // Events
    public event EventHandler<bool>? Toggled;
    
    static ToggleSwitch()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(ToggleSwitch), 
            new FrameworkPropertyMetadata(typeof(ToggleSwitch)));
            
        IsCheckedProperty.Changed.AddClassHandler<ToggleSwitch>(OnIsCheckedChanged);
    }
    
    public ToggleSwitch()
    {
        SetupInteraction();
    }
    
    void SetupInteraction()
    {
        PointerPressed += OnPointerPressed;
        KeyDown += OnKeyDown;
    }
    
    void OnPointerPressed(object? sender, PointerPressedEventArgs e)
    {
        if (e.GetCurrentPoint(this).Properties.IsLeftButtonPressed)
        {
            Toggle();
            e.Handled = true;
        }
    }
    
    void OnKeyDown(object? sender, KeyEventArgs e)
    {
        if (e.Key == Key.Space || e.Key == Key.Enter)
        {
            Toggle();
            e.Handled = true;
        }
    }
    
    public void Toggle()
    {
        IsChecked = !IsChecked;
    }
    
    static void OnIsCheckedChanged(ToggleSwitch control, AvaloniaPropertyChangedEventArgs e)
    {
        control.StartToggleAnimation();
        control.UpdatePseudoClasses();
        control.Toggled?.Invoke(control, control.IsChecked);
    }
    
    void StartToggleAnimation()
    {
        if (_isAnimating) return;
        
        _isAnimating = true;
        _animationTimer.Restart();
        
        // Start animation loop
        Dispatcher.UIThread.Post(AnimationLoop, DispatcherPriority.Render);
    }
    
    void AnimationLoop()
    {
        if (!_isAnimating) return;
        
        var elapsed = _animationTimer.ElapsedMilliseconds;
        var duration = 200; // 200ms animation
        
        if (elapsed >= duration)
        {
            _animationProgress = IsChecked ? 1.0 : 0.0;
            _isAnimating = false;
        }
        else
        {
            var progress = (double)elapsed / duration;
            _animationProgress = IsChecked ? progress : 1.0 - progress;
            
            // Continue animation
            Dispatcher.UIThread.Post(AnimationLoop, DispatcherPriority.Render);
        }
        
        InvalidateVisual();
    }
    
    void UpdatePseudoClasses()
    {
        PseudoClasses.Set(":checked", IsChecked);
        PseudoClasses.Set(":unchecked", !IsChecked);
    }
    
    protected override void OnApplyTemplate(TemplateAppliedEventArgs e)
    {
        base.OnApplyTemplate(e);
        UpdatePseudoClasses();
    }
}
```

### Complex State Management
```csharp
// Good: Control with complex internal state
public sealed class FileExplorer : TemplatedControl
{
    public static readonly StyledProperty<string> CurrentPathProperty =
        AvaloniaProperty.Register<FileExplorer, string>(nameof(CurrentPath), Environment.GetFolderPath(Environment.SpecialFolder.UserProfile));
    
    public static readonly StyledProperty<FileExplorerViewMode> ViewModeProperty =
        AvaloniaProperty.Register<FileExplorer, FileExplorerViewMode>(nameof(ViewMode), FileExplorerViewMode.List);
    
    // Internal state
    readonly ObservableCollection<FileSystemItem> _items = new();
    readonly Stack<string> _navigationHistory = new();
    readonly HashSet<string> _selectedItems = new();
    FileSystemWatcher? _fileWatcher;
    CancellationTokenSource? _loadCancellation;
    
    public string CurrentPath
    {
        get => GetValue(CurrentPathProperty);
        set => SetValue(CurrentPathProperty, value);
    }
    
    public FileExplorerViewMode ViewMode
    {
        get => GetValue(ViewModeProperty);
        set => SetValue(ViewModeProperty, value);
    }
    
    public IReadOnlyCollection<FileSystemItem> Items => _items;
    public IReadOnlyCollection<string> SelectedItems => _selectedItems;
    
    // Events
    public event EventHandler<string>? PathChanged;
    public event EventHandler<FileSystemItem>? ItemDoubleClicked;
    public event EventHandler<IEnumerable<string>>? SelectionChanged;
    
    static FileExplorer()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(FileExplorer), 
            new FrameworkPropertyMetadata(typeof(FileExplorer)));
            
        CurrentPathProperty.Changed.AddClassHandler<FileExplorer>(OnCurrentPathChanged);
        ViewModeProperty.Changed.AddClassHandler<FileExplorer>(OnViewModeChanged);
    }
    
    public FileExplorer()
    {
        SetupFileWatcher();
        LoadCurrentPath();
    }
    
    void SetupFileWatcher()
    {
        _fileWatcher = new FileSystemWatcher
        {
            NotifyFilter = NotifyFilters.FileName | NotifyFilters.DirectoryName | NotifyFilters.Size | NotifyFilters.LastWrite
        };
        
        _fileWatcher.Created += OnFileSystemChanged;
        _fileWatcher.Deleted += OnFileSystemChanged;
        _fileWatcher.Renamed += OnFileSystemChanged;
        _fileWatcher.Changed += OnFileSystemChanged;
    }
    
    void OnFileSystemChanged(object sender, FileSystemEventArgs e)
    {
        Dispatcher.UIThread.Post(() => RefreshCurrentPath());
    }
    
    async void LoadCurrentPath()
    {
        _loadCancellation?.Cancel();
        _loadCancellation = new CancellationTokenSource();
        
        try
        {
            var items = await LoadDirectoryAsync(CurrentPath, _loadCancellation.Token);
            
            _items.Clear();
            foreach (var item in items)
            {
                _items.Add(item);
            }
            
            UpdateFileWatcher();
        }
        catch (OperationCanceledException)
        {
            // Loading was cancelled
        }
        catch (Exception ex)
        {
            // Handle loading error
            ShowError($"Failed to load directory: {ex.Message}");
        }
    }
    
    async Task<IEnumerable<FileSystemItem>> LoadDirectoryAsync(string path, CancellationToken cancellationToken)
    {
        return await Task.Run(() =>
        {
            var items = new List<FileSystemItem>();
            
            try
            {
                var directory = new DirectoryInfo(path);
                
                // Add directories
                foreach (var dir in directory.GetDirectories())
                {
                    cancellationToken.ThrowIfCancellationRequested();
                    items.Add(new FileSystemItem
                    {
                        Name = dir.Name,
                        FullPath = dir.FullName,
                        IsDirectory = true,
                        Size = 0,
                        LastModified = dir.LastWriteTime
                    });
                }
                
                // Add files
                foreach (var file in directory.GetFiles())
                {
                    cancellationToken.ThrowIfCancellationRequested();
                    items.Add(new FileSystemItem
                    {
                        Name = file.Name,
                        FullPath = file.FullName,
                        IsDirectory = false,
                        Size = file.Length,
                        LastModified = file.LastWriteTime
                    });
                }
            }
            catch (UnauthorizedAccessException)
            {
                // Handle access denied
            }
            
            return items.OrderBy(i => !i.IsDirectory).ThenBy(i => i.Name);
        }, cancellationToken);
    }
    
    void UpdateFileWatcher()
    {
        if (_fileWatcher is not null && Directory.Exists(CurrentPath))
        {
            _fileWatcher.Path = CurrentPath;
            _fileWatcher.EnableRaisingEvents = true;
        }
    }
    
    public void NavigateUp()
    {
        var parent = Directory.GetParent(CurrentPath);
        if (parent is not null)
        {
            NavigateTo(parent.FullName);
        }
    }
    
    public void NavigateTo(string path)
    {
        if (Directory.Exists(path))
        {
            _navigationHistory.Push(CurrentPath);
            CurrentPath = path;
        }
    }
    
    public void NavigateBack()
    {
        if (_navigationHistory.Count > 0)
        {
            CurrentPath = _navigationHistory.Pop();
        }
    }
    
    public void RefreshCurrentPath()
    {
        LoadCurrentPath();
    }
    
    public void SelectItem(string fullPath)
    {
        _selectedItems.Add(fullPath);
        SelectionChanged?.Invoke(this, _selectedItems);
    }
    
    public void UnselectItem(string fullPath)
    {
        _selectedItems.Remove(fullPath);
        SelectionChanged?.Invoke(this, _selectedItems);
    }
    
    public void ClearSelection()
    {
        _selectedItems.Clear();
        SelectionChanged?.Invoke(this, _selectedItems);
    }
    
    static void OnCurrentPathChanged(FileExplorer control, AvaloniaPropertyChangedEventArgs e)
    {
        control.LoadCurrentPath();
        control.PathChanged?.Invoke(control, control.CurrentPath);
    }
    
    static void OnViewModeChanged(FileExplorer control, AvaloniaPropertyChangedEventArgs e)
    {
        control.UpdateViewMode();
    }
    
    void UpdateViewMode()
    {
        PseudoClasses.Set(":list-view", ViewMode == FileExplorerViewMode.List);
        PseudoClasses.Set(":grid-view", ViewMode == FileExplorerViewMode.Grid);
        PseudoClasses.Set(":details-view", ViewMode == FileExplorerViewMode.Details);
    }
    
    void ShowError(string message)
    {
        // Implementation for showing error messages
    }
    
    protected override void OnApplyTemplate(TemplateAppliedEventArgs e)
    {
        base.OnApplyTemplate(e);
        UpdateViewMode();
    }
}

public enum FileExplorerViewMode
{
    List,
    Grid,
    Details
}

public sealed class FileSystemItem
{
    public string Name { get; set; } = string.Empty;
    public string FullPath { get; set; } = string.Empty;
    public bool IsDirectory { get; set; }
    public long Size { get; set; }
    public DateTime LastModified { get; set; }
}
```

## Styling Integration

### TemplatedControl Styling

Only `TemplatedControl` supports external styling through control templates:

#### Control Template Structure
```csharp
// Good: Control designed for external styling
public sealed class NotificationCard : TemplatedControl
{
    public static readonly StyledProperty<string> TitleProperty =
        AvaloniaProperty.Register<NotificationCard, string>(nameof(Title));
    
    public static readonly StyledProperty<string> MessageProperty =
        AvaloniaProperty.Register<NotificationCard, string>(nameof(Message));
    
    public static readonly StyledProperty<NotificationSeverity> SeverityProperty =
        AvaloniaProperty.Register<NotificationCard, NotificationSeverity>(nameof(Severity));
    
    public static readonly StyledProperty<bool> IsClosableProperty =
        AvaloniaProperty.Register<NotificationCard, bool>(nameof(IsClosable), true);
    
    // Template part names for styling
    public const string PART_CloseButton = "PART_CloseButton";
    public const string PART_IconPresenter = "PART_IconPresenter";
    public const string PART_ContentPanel = "PART_ContentPanel";
    
    Button? _closeButton;
    ContentPresenter? _iconPresenter;
    Panel? _contentPanel;
    
    public string Title
    {
        get => GetValue(TitleProperty);
        set => SetValue(TitleProperty, value);
    }
    
    public string Message
    {
        get => GetValue(MessageProperty);
        set => SetValue(MessageProperty, value);
    }
    
    public NotificationSeverity Severity
    {
        get => GetValue(SeverityProperty);
        set => SetValue(SeverityProperty, value);
    }
    
    public bool IsClosable
    {
        get => GetValue(IsClosableProperty);
        set => SetValue(IsClosableProperty, value);
    }
    
    // Events
    public event EventHandler? CloseRequested;
    
    static NotificationCard()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(NotificationCard), 
            new FrameworkPropertyMetadata(typeof(NotificationCard)));
            
        SeverityProperty.Changed.AddClassHandler<NotificationCard>(OnSeverityChanged);
        IsClosableProperty.Changed.AddClassHandler<NotificationCard>(OnIsClosableChanged);
    }
    
    protected override void OnApplyTemplate(TemplateAppliedEventArgs e)
    {
        // Disconnect old template parts
        if (_closeButton is not null)
        {
            _closeButton.Click -= OnCloseButtonClick;
        }
        
        base.OnApplyTemplate(e);
        
        // Connect new template parts
        _closeButton = e.NameScope.Find<Button>(PART_CloseButton);
        _iconPresenter = e.NameScope.Find<ContentPresenter>(PART_IconPresenter);
        _contentPanel = e.NameScope.Find<Panel>(PART_ContentPanel);
        
        if (_closeButton is not null)
        {
            _closeButton.Click += OnCloseButtonClick;
        }
        
        UpdatePseudoClasses();
    }
    
    void OnCloseButtonClick(object? sender, RoutedEventArgs e)
    {
        CloseRequested?.Invoke(this, EventArgs.Empty);
    }
    
    static void OnSeverityChanged(NotificationCard control, AvaloniaPropertyChangedEventArgs e)
    {
        control.UpdatePseudoClasses();
    }
    
    static void OnIsClosableChanged(NotificationCard control, AvaloniaPropertyChangedEventArgs e)
    {
        control.UpdatePseudoClasses();
    }
    
    void UpdatePseudoClasses()
    {
        PseudoClasses.Set(":info", Severity == NotificationSeverity.Info);
        PseudoClasses.Set(":warning", Severity == NotificationSeverity.Warning);
        PseudoClasses.Set(":error", Severity == NotificationSeverity.Error);
        PseudoClasses.Set(":success", Severity == NotificationSeverity.Success);
        PseudoClasses.Set(":closable", IsClosable);
    }
}

public enum NotificationSeverity
{
    Info,
    Warning,
    Error,
    Success
}
```

#### Corresponding Style File Example
```xml
<!-- NotificationCard.axaml - Separate styling file -->
<Styles xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:controls="using:MyApp.Controls">
  
  <!-- Default NotificationCard style -->
  <Style Selector="controls|NotificationCard">
    <Setter Property="Template">
      <ControlTemplate>
        <Border Classes="notification-card">
          <Grid ColumnDefinitions="Auto,*,Auto">
            
            <!-- Icon -->
            <ContentPresenter Name="PART_IconPresenter"
                              Grid.Column="0"
                              Classes="notification-icon" />
            
            <!-- Content -->
            <StackPanel Name="PART_ContentPanel"
                        Grid.Column="1"
                        Classes="notification-content">
              <TextBlock Text="{TemplateBinding Title}"
                         Classes="notification-title" />
              <TextBlock Text="{TemplateBinding Message}"
                         Classes="notification-message" />
            </StackPanel>
            
            <!-- Close button -->
            <Button Name="PART_CloseButton"
                    Grid.Column="2"
                    Classes="notification-close"
                    Content="×" />
          </Grid>
        </Border>
      </ControlTemplate>
    </Setter>
  </Style>
  
  <!-- Severity-specific styles -->
  <Style Selector="controls|NotificationCard:info">
    <Setter Property="Classes" Value="info" />
  </Style>
  
  <Style Selector="controls|NotificationCard:warning">
    <Setter Property="Classes" Value="warning" />
  </Style>
  
  <Style Selector="controls|NotificationCard:error">
    <Setter Property="Classes" Value="error" />
  </Style>
  
  <Style Selector="controls|NotificationCard:success">
    <Setter Property="Classes" Value="success" />
  </Style>
  
  <!-- Hide close button when not closable -->
  <Style Selector="controls|NotificationCard:not(:closable) /template/ Button#PART_CloseButton">
    <Setter Property="IsVisible" Value="False" />
  </Style>
  
</Styles>
```

### UserControl and Control Styling

`UserControl` and `Control` don't use control templates but can still be styled:

#### UserControl Styling
```csharp
// UserControl with styleable properties
public sealed class InfoCard : UserControl
{
    public static readonly StyledProperty<string> TitleProperty =
        AvaloniaProperty.Register<InfoCard, string>(nameof(Title));
    
    readonly TextBlock _titleBlock;
    readonly Border _rootBorder;
    
    public InfoCard()
    {
        _titleBlock = new TextBlock { Classes = { "info-card-title" } };
        _rootBorder = new Border 
        { 
            Classes = { "info-card" },
            Child = _titleBlock
        };
        
        Content = _rootBorder;
        
        // Bind properties to UI elements
        this.GetObservable(TitleProperty)
            .Subscribe(title => _titleBlock.Text = title);
    }
}
```

#### UserControl Style File
```xml
<!-- InfoCard can be styled through classes -->
<Styles xmlns="https://github.com/avaloniaui">
  <Style Selector="Border.info-card">
    <Setter Property="Background" Value="White" />
    <Setter Property="BorderBrush" Value="Gray" />
    <Setter Property="BorderThickness" Value="1" />
    <Setter Property="CornerRadius" Value="4" />
    <Setter Property="Padding" Value="16" />
  </Style>
  
  <Style Selector="TextBlock.info-card-title">
    <Setter Property="FontWeight" Value="Bold" />
    <Setter Property="FontSize" Value="16" />
  </Style>
</Styles>
```

#### Control Styling
```csharp
// Control with styleable properties
public sealed class CustomShape : Control
{
    public static readonly StyledProperty<IBrush> FillProperty =
        AvaloniaProperty.Register<CustomShape, IBrush>(nameof(Fill), Brushes.Blue);
    
    public IBrush Fill
    {
        get => GetValue(FillProperty);
        set => SetValue(FillProperty, value);
    }
    
    static CustomShape()
    {
        AffectsRender<CustomShape>(FillProperty);
    }
    
    public override void Render(DrawingContext context)
    {
        // Use styleable properties in rendering
        context.FillRectangle(Fill, Bounds);
    }
}
```

#### Control Style File
```xml
<!-- Control styled through direct properties -->
<Styles xmlns="https://github.com/avaloniaui"
        xmlns:controls="using:MyApp.Controls">
  <Style Selector="controls|CustomShape">
    <Setter Property="Fill" Value="Red" />
  </Style>
  
  <Style Selector="controls|CustomShape:pointerover">
    <Setter Property="Fill" Value="DarkRed" />
  </Style>
</Styles>
```

## Performance Optimization

### Efficient Rendering
```csharp
// Good: Performance-optimized control
public sealed class VirtualizedList : TemplatedControl
{
    public static readonly StyledProperty<IEnumerable> ItemsSourceProperty =
        AvaloniaProperty.Register<VirtualizedList, IEnumerable>(nameof(ItemsSource));
    
    public static readonly StyledProperty<double> ItemHeightProperty =
        AvaloniaProperty.Register<VirtualizedList, double>(nameof(ItemHeight), 25.0);
    
    // Virtualization state
    readonly List<object> _items = new();
    readonly Dictionary<int, Control> _realizedItems = new();
    ScrollViewer? _scrollViewer;
    Canvas? _itemsCanvas;
    int _firstVisibleIndex;
    int _lastVisibleIndex;
    
    public IEnumerable ItemsSource
    {
        get => GetValue(ItemsSourceProperty);
        set => SetValue(ItemsSourceProperty, value);
    }
    
    public double ItemHeight
    {
        get => GetValue(ItemHeightProperty);
        set => SetValue(ItemHeightProperty, value);
    }
    
    static VirtualizedList()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(VirtualizedList), 
            new FrameworkPropertyMetadata(typeof(VirtualizedList)));
            
        ItemsSourceProperty.Changed.AddClassHandler<VirtualizedList>(OnItemsSourceChanged);
    }
    
    protected override void OnApplyTemplate(TemplateAppliedEventArgs e)
    {
        base.OnApplyTemplate(e);
        
        _scrollViewer = e.NameScope.Find<ScrollViewer>("PART_ScrollViewer");
        _itemsCanvas = e.NameScope.Find<Canvas>("PART_ItemsCanvas");
        
        if (_scrollViewer is not null)
        {
            _scrollViewer.ScrollChanged += OnScrollChanged;
        }
        
        UpdateVirtualization();
    }
    
    void OnScrollChanged(object? sender, ScrollChangedEventArgs e)
    {
        UpdateVirtualization();
    }
    
    void UpdateVirtualization()
    {
        if (_scrollViewer is null || _itemsCanvas is null) return;
        
        var viewportHeight = _scrollViewer.Viewport.Height;
        var scrollOffset = _scrollViewer.Offset.Y;
        
        _firstVisibleIndex = Math.Max(0, (int)(scrollOffset / ItemHeight));
        _lastVisibleIndex = Math.Min(_items.Count - 1, 
            (int)((scrollOffset + viewportHeight) / ItemHeight) + 1);
        
        RealizeItems();
        VirtualizeItems();
        UpdateCanvasHeight();
    }
    
    void RealizeItems()
    {
        for (int i = _firstVisibleIndex; i <= _lastVisibleIndex; i++)
        {
            if (!_realizedItems.ContainsKey(i))
            {
                var item = CreateItemContainer(_items[i]);
                Canvas.SetTop(item, i * ItemHeight);
                _realizedItems[i] = item;
                _itemsCanvas!.Children.Add(item);
            }
        }
    }
    
    void VirtualizeItems()
    {
        var itemsToRemove = _realizedItems
            .Where(kvp => kvp.Key < _firstVisibleIndex || kvp.Key > _lastVisibleIndex)
            .ToList();
        
        foreach (var (index, control) in itemsToRemove)
        {
            _itemsCanvas!.Children.Remove(control);
            _realizedItems.Remove(index);
        }
    }
    
    Control CreateItemContainer(object dataItem)
    {
        return new TextBlock
        {
            Text = dataItem?.ToString() ?? string.Empty,
            Height = ItemHeight,
            VerticalAlignment = VerticalAlignment.Center
        };
    }
    
    void UpdateCanvasHeight()
    {
        if (_itemsCanvas is not null)
        {
            _itemsCanvas.Height = _items.Count * ItemHeight;
        }
    }
    
    static void OnItemsSourceChanged(VirtualizedList control, AvaloniaPropertyChangedEventArgs e)
    {
        control.RefreshItems();
    }
    
    void RefreshItems()
    {
        _items.Clear();
        _realizedItems.Clear();
        _itemsCanvas?.Children.Clear();
        
        if (ItemsSource is not null)
        {
            foreach (var item in ItemsSource)
            {
                _items.Add(item);
            }
        }
        
        UpdateVirtualization();
    }
}
```

## Event Handling and Communication

### Event-Driven Architecture
```csharp
// Good: Control with proper event handling
public sealed class MediaPlayer : TemplatedControl
{
    public static readonly StyledProperty<string> SourceProperty =
        AvaloniaProperty.Register<MediaPlayer, string>(nameof(Source));
    
    public static readonly StyledProperty<bool> IsPlayingProperty =
        AvaloniaProperty.Register<MediaPlayer, bool>(nameof(IsPlaying));
    
    public static readonly StyledProperty<TimeSpan> PositionProperty =
        AvaloniaProperty.Register<MediaPlayer, TimeSpan>(nameof(Position));
    
    public static readonly StyledProperty<TimeSpan> DurationProperty =
        AvaloniaProperty.Register<MediaPlayer, TimeSpan>(nameof(Duration));
    
    // Internal media state
    readonly Timer _positionTimer;
    IMediaEngine? _mediaEngine;
    
    public string Source
    {
        get => GetValue(SourceProperty);
        set => SetValue(SourceProperty, value);
    }
    
    public bool IsPlaying
    {
        get => GetValue(IsPlayingProperty);
        private set => SetValue(IsPlayingProperty, value);
    }
    
    public TimeSpan Position
    {
        get => GetValue(PositionProperty);
        set => SetValue(PositionProperty, value);
    }
    
    public TimeSpan Duration
    {
        get => GetValue(DurationProperty);
        private set => SetValue(DurationProperty, value);
    }
    
    // Events
    public event EventHandler? PlaybackStarted;
    public event EventHandler? PlaybackPaused;
    public event EventHandler? PlaybackStopped;
    public event EventHandler<TimeSpan>? PositionChanged;
    public event EventHandler<string>? MediaError;
    
    static MediaPlayer()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(MediaPlayer), 
            new FrameworkPropertyMetadata(typeof(MediaPlayer)));
            
        SourceProperty.Changed.AddClassHandler<MediaPlayer>(OnSourceChanged);
        PositionProperty.Changed.AddClassHandler<MediaPlayer>(OnPositionChanged);
    }
    
    public MediaPlayer()
    {
        _positionTimer = new Timer(UpdatePosition, null, Timeout.Infinite, Timeout.Infinite);
        InitializeMediaEngine();
    }
    
    void InitializeMediaEngine()
    {
        _mediaEngine = CreateMediaEngine();
        _mediaEngine.PlaybackStarted += OnPlaybackStarted;
        _mediaEngine.PlaybackPaused += OnPlaybackPaused;
        _mediaEngine.PlaybackStopped += OnPlaybackStopped;
        _mediaEngine.MediaError += OnMediaError;
    }
    
    public async Task PlayAsync()
    {
        if (_mediaEngine is not null && !string.IsNullOrEmpty(Source))
        {
            await _mediaEngine.PlayAsync();
        }
    }
    
    public async Task PauseAsync()
    {
        if (_mediaEngine is not null)
        {
            await _mediaEngine.PauseAsync();
        }
    }
    
    public async Task StopAsync()
    {
        if (_mediaEngine is not null)
        {
            await _mediaEngine.StopAsync();
        }
    }
    
    public async Task SeekAsync(TimeSpan position)
    {
        if (_mediaEngine is not null)
        {
            await _mediaEngine.SeekAsync(position);
        }
    }
    
    void OnPlaybackStarted(object? sender, EventArgs e)
    {
        IsPlaying = true;
        _positionTimer.Change(0, 100); // Update every 100ms
        UpdatePseudoClasses();
        PlaybackStarted?.Invoke(this, EventArgs.Empty);
    }
    
    void OnPlaybackPaused(object? sender, EventArgs e)
    {
        IsPlaying = false;
        _positionTimer.Change(Timeout.Infinite, Timeout.Infinite);
        UpdatePseudoClasses();
        PlaybackPaused?.Invoke(this, EventArgs.Empty);
    }
    
    void OnPlaybackStopped(object? sender, EventArgs e)
    {
        IsPlaying = false;
        Position = TimeSpan.Zero;
        _positionTimer.Change(Timeout.Infinite, Timeout.Infinite);
        UpdatePseudoClasses();
        PlaybackStopped?.Invoke(this, EventArgs.Empty);
    }
    
    void OnMediaError(object? sender, string error)
    {
        MediaError?.Invoke(this, error);
    }
    
    void UpdatePosition(object? state)
    {
        if (_mediaEngine is not null && IsPlaying)
        {
            Dispatcher.UIThread.Post(() =>
            {
                Position = _mediaEngine.CurrentPosition;
                Duration = _mediaEngine.Duration;
                PositionChanged?.Invoke(this, Position);
            });
        }
    }
    
    void UpdatePseudoClasses()
    {
        PseudoClasses.Set(":playing", IsPlaying);
        PseudoClasses.Set(":paused", !IsPlaying);
    }
    
    static void OnSourceChanged(MediaPlayer control, AvaloniaPropertyChangedEventArgs e)
    {
        control.LoadMedia();
    }
    
    static void OnPositionChanged(MediaPlayer control, AvaloniaPropertyChangedEventArgs e)
    {
        // Handle external position changes (seeking)
        if (!control.IsPlaying)
        {
            control.SeekAsync(control.Position);
        }
    }
    
    async void LoadMedia()
    {
        if (_mediaEngine is not null && !string.IsNullOrEmpty(Source))
        {
            try
            {
                await _mediaEngine.LoadAsync(Source);
                Duration = _mediaEngine.Duration;
            }
            catch (Exception ex)
            {
                MediaError?.Invoke(this, ex.Message);
            }
        }
    }
    
    IMediaEngine CreateMediaEngine()
    {
        // Platform-specific media engine creation
        return new PlatformMediaEngine();
    }
    
    protected override void OnApplyTemplate(TemplateAppliedEventArgs e)
    {
        base.OnApplyTemplate(e);
        UpdatePseudoClasses();
    }
}

// Interface for media engine abstraction
public interface IMediaEngine
{
    TimeSpan CurrentPosition { get; }
    TimeSpan Duration { get; }
    
    event EventHandler? PlaybackStarted;
    event EventHandler? PlaybackPaused;
    event EventHandler? PlaybackStopped;
    event EventHandler<string>? MediaError;
    
    Task LoadAsync(string source);
    Task PlayAsync();
    Task PauseAsync();
    Task StopAsync();
    Task SeekAsync(TimeSpan position);
}
```

## Cross-Platform Considerations

### Platform-Specific Implementations
```csharp
// Good: Cross-platform control with platform-specific behavior
public sealed class SystemTrayIcon : TemplatedControl
{
    public static readonly StyledProperty<string> TooltipTextProperty =
        AvaloniaProperty.Register<SystemTrayIcon, string>(nameof(TooltipText));
    
    public static readonly StyledProperty<object?> IconProperty =
        AvaloniaProperty.Register<SystemTrayIcon, object?>(nameof(Icon));
    
    public static readonly StyledProperty<bool> IsVisibleProperty =
        AvaloniaProperty.Register<SystemTrayIcon, bool>(nameof(IsVisible), true);
    
    // Platform-specific implementations
    readonly IPlatformTrayIcon _platformTrayIcon;
    
    public string TooltipText
    {
        get => GetValue(TooltipTextProperty);
        set => SetValue(TooltipTextProperty, value);
    }
    
    public object? Icon
    {
        get => GetValue(IconProperty);
        set => SetValue(IconProperty, value);
    }
    
    public bool IsVisible
    {
        get => GetValue(IsVisibleProperty);
        set => SetValue(IsVisibleProperty, value);
    }
    
    // Events
    public event EventHandler? LeftClicked;
    public event EventHandler? RightClicked;
    public event EventHandler? DoubleClicked;
    
    static SystemTrayIcon()
    {
        DefaultStyleKeyProperty.OverrideMetadata(typeof(SystemTrayIcon), 
            new FrameworkPropertyMetadata(typeof(SystemTrayIcon)));
            
        TooltipTextProperty.Changed.AddClassHandler<SystemTrayIcon>(OnTooltipTextChanged);
        IconProperty.Changed.AddClassHandler<SystemTrayIcon>(OnIconChanged);
        IsVisibleProperty.Changed.AddClassHandler<SystemTrayIcon>(OnIsVisibleChanged);
    }
    
    public SystemTrayIcon()
    {
        _platformTrayIcon = CreatePlatformTrayIcon();
        _platformTrayIcon.LeftClicked += (s, e) => LeftClicked?.Invoke(this, EventArgs.Empty);
        _platformTrayIcon.RightClicked += (s, e) => RightClicked?.Invoke(this, EventArgs.Empty);
        _platformTrayIcon.DoubleClicked += (s, e) => DoubleClicked?.Invoke(this, EventArgs.Empty);
    }
    
    IPlatformTrayIcon CreatePlatformTrayIcon()
    {
        if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
        {
            return new WindowsTrayIcon();
        }
        else if (RuntimeInformation.IsOSPlatform(OSPlatform.OSX))
        {
            return new MacOSTrayIcon();
        }
        else if (RuntimeInformation.IsOSPlatform(OSPlatform.Linux))
        {
            return new LinuxTrayIcon();
        }
        else
        {
            return new NullTrayIcon();
        }
    }
    
    static void OnTooltipTextChanged(SystemTrayIcon control, AvaloniaPropertyChangedEventArgs e)
    {
        control._platformTrayIcon.SetTooltip(control.TooltipText);
    }
    
    static void OnIconChanged(SystemTrayIcon control, AvaloniaPropertyChangedEventArgs e)
    {
        control._platformTrayIcon.SetIcon(control.Icon);
    }
    
    static void OnIsVisibleChanged(SystemTrayIcon control, AvaloniaPropertyChangedEventArgs e)
    {
        control._platformTrayIcon.SetVisible(control.IsVisible);
    }
}

// Platform abstraction interface
public interface IPlatformTrayIcon
{
    event EventHandler? LeftClicked;
    event EventHandler? RightClicked;
    event EventHandler? DoubleClicked;
    
    void SetTooltip(string tooltip);
    void SetIcon(object? icon);
    void SetVisible(bool visible);
}
```

## Testing and Debugging

### Testable Control Design
```csharp
// Good: Control designed for testing
public sealed class Calculator : TemplatedControl
{
    public static readonly StyledProperty<string> DisplayValueProperty =
        AvaloniaProperty.Register<Calculator, string>(nameof(DisplayValue), "0");
    
    // Internal calculation state
    decimal _currentValue;
    decimal _previousValue;
    CalculatorOperation _pendingOperation = CalculatorOperation.None;
    bool _isNewEntry = true;
    
    public string DisplayValue
    {
        get => GetValue(DisplayValueProperty);
        private set => SetValue(DisplayValueProperty, value);
    }
    
    // Public methods for testing
    public void InputDigit(int digit)
    {
        if (digit < 0 || digit > 9) return;
        
        if (_isNewEntry)
        {
            DisplayValue = digit.ToString();
            _isNewEntry = false;
        }
        else
        {
            DisplayValue = DisplayValue == "0" ? digit.ToString() : DisplayValue + digit;
        }
        
        _currentValue = decimal.Parse(DisplayValue);
    }
    
    public void InputOperation(CalculatorOperation operation)
    {
        if (_pendingOperation != CalculatorOperation.None && !_isNewEntry)
        {
            Calculate();
        }
        
        _previousValue = _currentValue;
        _pendingOperation = operation;
        _isNewEntry = true;
    }
    
    public void Calculate()
    {
        if (_pendingOperation == CalculatorOperation.None) return;
        
        var result = _pendingOperation switch
        {
            CalculatorOperation.Add => _previousValue + _currentValue,
            CalculatorOperation.Subtract => _previousValue - _currentValue,
            CalculatorOperation.Multiply => _previousValue * _currentValue,
            CalculatorOperation.Divide => _currentValue != 0 ? _previousValue / _currentValue : 0,
            _ => _currentValue
        };
        
        _currentValue = result;
        DisplayValue = result.ToString();
        _pendingOperation = CalculatorOperation.None;
        _isNewEntry = true;
    }
    
    public void Clear()
    {
        _currentValue = 0;
        _previousValue = 0;
        _pendingOperation = CalculatorOperation.None;
        _isNewEntry = true;
        DisplayValue = "0";
    }
    
    // Internal state access for testing
    internal decimal CurrentValue => _currentValue;
    internal decimal PreviousValue => _previousValue;
    internal CalculatorOperation PendingOperation => _pendingOperation;
    internal bool IsNewEntry => _isNewEntry;
}

public enum CalculatorOperation
{
    None,
    Add,
    Subtract,
    Multiply,
    Divide
}
```

# End of Kiro Steering File