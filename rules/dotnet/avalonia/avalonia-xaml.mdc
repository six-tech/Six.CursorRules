---
description: This file provides comprehensive guidelines for writing clean, maintainable Avalonia XAML with modern UI patterns, data binding, styling, and performance best practices.
fileMatchPattern: "*.axaml", "*.xaml"
inclusion: fileMatch
---

# Kiro Steering File: Avalonia XAML Best Practices

**Role Definition:**
- Avalonia UI Expert
- XAML Specialist
- UI/UX Designer
- Cross-Platform Desktop Application Developer

## General

### Description
Avalonia XAML code should be written to maximize readability, maintainability, and performance while following modern UI patterns, proper data binding, and cross-platform compatibility. Focus on clean markup, efficient styling, and proper resource management.

### Requirements
- **NEVER** place sensitive information in XAML markup (passwords, API keys, personal data)
- When writing XAML, clearly separate main logical UI sections with a space and a comment, for example: `<!-- MAIN HEADER -->`
- Use proper data binding patterns with explicit binding modes
- Create reusable and maintainable styles and templates
- Ensure cross-platform compatibility
- Use modern Avalonia UI features and controls

## XAML Structure and Organization

### View Structure
- Organize XAML with clear hierarchy and proper indentation:

```xml
<!-- Good: Clear structure with proper organization -->
<UserControl x:Class="MyApp.Views.CustomerView"
             xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             xmlns:controls="using:MyApp.Controls">
  
  <Design.DataContext>
    <vm:CustomerViewModel />
  </Design.DataContext>
  
  <Grid RowDefinitions="Auto,*,Auto">
    <!-- HEADER -->
    <StackPanel Grid.Row="0" Classes="header">
      <TextBlock Text="{Binding Title}" Classes="title" />
    </StackPanel>
    
    <!-- MAIN CONTENT -->
    <ScrollViewer Grid.Row="1">
      <ContentPresenter Content="{Binding CurrentView}" />
    </ScrollViewer>
    
    <!-- FOOTER -->
    <StackPanel Grid.Row="2" Classes="footer">
      <Button Command="{Binding SaveCommand}" Content="Save" />
    </StackPanel>
  </Grid>
</UserControl>
```

```xml
<!-- Avoid: Flat structure without organization -->
<UserControl>
  <TextBlock Text="{Binding Title}" />
  <Button Command="{Binding SaveCommand}" />
  <ListBox Items="{Binding Items}" />
</UserControl>
```

### Namespace Organization
- Use consistent and meaningful namespace aliases:

```xml
<!-- Good: Clear namespace organization -->
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             xmlns:controls="using:MyApp.Controls"
             xmlns:converters="using:MyApp.Converters"
             xmlns:behaviors="using:MyApp.Behaviors">

<!-- Avoid: Generic or unclear aliases -->
<UserControl xmlns:local="using:MyApp"
             xmlns:stuff="using:MyApp.Something">
```

## Data Binding Best Practices

### Binding Syntax
- Use proper binding modes and null handling:

```xml
<!-- Good: Explicit binding modes and null handling -->
<TextBox Text="{Binding CustomerName, Mode=TwoWay, FallbackValue=''}" />
<TextBlock Text="{Binding LastUpdated, StringFormat='Last updated: {0:yyyy-MM-dd}'}" />
<CheckBox IsChecked="{Binding IsActive, Mode=TwoWay}" />

<!-- Good: Command binding with parameter -->
<Button Command="{Binding DeleteCommand}" 
        CommandParameter="{Binding SelectedItem}"
        Content="Delete" />

<!-- Avoid: Implicit binding modes -->
<TextBox Text="{Binding CustomerName}" />
```

### Collection Binding
- Use proper collection binding with item templates:

```xml
<!-- Good: Proper collection binding with templates -->
<ListBox Items="{Binding Customers}"
         SelectedItem="{Binding SelectedCustomer, Mode=TwoWay}">
  <ListBox.ItemTemplate>
    <DataTemplate>
      <Border Classes="customer-item">
        <StackPanel>
          <TextBlock Text="{Binding Name}" Classes="customer-name" />
          <TextBlock Text="{Binding Email}" Classes="customer-email" />
        </StackPanel>
      </Border>
    </DataTemplate>
  </ListBox.ItemTemplate>
</ListBox>

<!-- Good: DataGrid with proper column definitions -->
<DataGrid Items="{Binding Orders}" 
          SelectedItem="{Binding SelectedOrder, Mode=TwoWay}"
          IsReadOnly="True">
  <DataGrid.Columns>
    <DataGridTextColumn Header="Order ID" Binding="{Binding Id}" />
    <DataGridTextColumn Header="Customer" Binding="{Binding CustomerName}" />
    <DataGridTextColumn Header="Total" Binding="{Binding Total, StringFormat=C}" />
  </DataGrid.Columns>
</DataGrid>
```

## Styling and Theming

### Style Definitions
- Create reusable and maintainable styles:

```xml
<!-- Good: Organized styles with clear naming -->
<Styles xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
  
  <!-- Base button style -->
  <Style Selector="Button.primary">
    <Setter Property="Background" Value="{DynamicResource PrimaryBrush}" />
    <Setter Property="Foreground" Value="{DynamicResource PrimaryForegroundBrush}" />
    <Setter Property="Padding" Value="12,6" />
    <Setter Property="CornerRadius" Value="4" />
  </Style>
  
  <!-- Hover state -->
  <Style Selector="Button.primary:pointerover">
    <Setter Property="Background" Value="{DynamicResource PrimaryHoverBrush}" />
  </Style>
  
  <!-- Disabled state -->
  <Style Selector="Button.primary:disabled">
    <Setter Property="Opacity" Value="0.6" />
  </Style>
</Styles>
```

```xml
<!-- Avoid: Inline styles without reusability -->
<Button Background="Blue" Foreground="White" Padding="10" />
```

### Resource Management
- Use proper resource organization:

```xml
<!-- Good: Organized resources -->
<Application.Resources>
  <ResourceDictionary>
    <!-- Colors -->
    <Color x:Key="PrimaryColor">#2196F3</Color>
    <Color x:Key="SecondaryColor">#FFC107</Color>
    
    <!-- Brushes -->
    <SolidColorBrush x:Key="PrimaryBrush" Color="{StaticResource PrimaryColor}" />
    <SolidColorBrush x:Key="SecondaryBrush" Color="{StaticResource SecondaryColor}" />
    
    <!-- Converters -->
    <converters:BooleanToVisibilityConverter x:Key="BoolToVisibility" />
    <converters:InverseBooleanConverter x:Key="InverseBool" />
  </ResourceDictionary>
</Application.Resources>
```

## Control Templates and Custom Controls

### Control Templates
- Define proper control templates:

```xml
<!-- Good: Comprehensive control template -->
<Style Selector="controls|LoadingButton">
  <Setter Property="Template">
    <ControlTemplate>
      <Border Background="{TemplateBinding Background}"
              BorderBrush="{TemplateBinding BorderBrush}"
              BorderThickness="{TemplateBinding BorderThickness}"
              CornerRadius="{TemplateBinding CornerRadius}">
        <Grid>
          <ContentPresenter Name="PART_ContentPresenter"
                            Content="{TemplateBinding Content}"
                            Margin="{TemplateBinding Padding}"
                            HorizontalAlignment="{TemplateBinding HorizontalContentAlignment}"
                            VerticalAlignment="{TemplateBinding VerticalContentAlignment}" />
                            
          <StackPanel Name="PART_LoadingPanel"
                      Orientation="Horizontal"
                      HorizontalAlignment="Center"
                      VerticalAlignment="Center"
                      IsVisible="False">
            <controls:LoadingSpinner Width="16" Height="16" Margin="0,0,8,0" />
            <TextBlock Text="{TemplateBinding LoadingText}" />
          </StackPanel>
        </Grid>
      </Border>
    </ControlTemplate>
  </Setter>
</Style>

<!-- Loading state -->
<Style Selector="controls|LoadingButton:loading /template/ Panel#PART_ContentPresenter">
  <Setter Property="IsVisible" Value="False" />
</Style>

<Style Selector="controls|LoadingButton:loading /template/ Panel#PART_LoadingPanel">
  <Setter Property="IsVisible" Value="True" />
</Style>
```

## Performance and Memory Management

### Virtualization
- Use virtualization for large collections:

```xml
<!-- Good: Virtualized list for performance -->
<ListBox Items="{Binding LargeCollection}"
         VirtualizationMode="Simple">
  <ListBox.ItemsPanel>
    <ItemsPanelTemplate>
      <VirtualizingStackPanel />
    </ItemsPanelTemplate>
  </ListBox.ItemsPanel>
</ListBox>

<!-- Good: TreeView with virtualization -->
<TreeView Items="{Binding TreeItems}">
  <TreeView.ItemsPanel>
    <ItemsPanelTemplate>
      <VirtualizingStackPanel />
    </ItemsPanelTemplate>
  </TreeView.ItemsPanel>
</TreeView>
```

## Cross-Platform Considerations

### Responsive Design
- Create responsive layouts:

```xml
<!-- Good: Responsive design with adaptive layouts -->
<Grid>
  <Grid.ColumnDefinitions>
    <ColumnDefinition Width="*" MinWidth="200" MaxWidth="300" />
    <ColumnDefinition Width="3*" />
  </Grid.ColumnDefinitions>
  
  <!-- SIDEBAR -->
  <Border Grid.Column="0" Classes="sidebar">
    <ScrollViewer>
      <StackPanel>
        <!-- Navigation items -->
      </StackPanel>
    </ScrollViewer>
  </Border>
  
  <!-- MAIN CONTENT -->
  <ContentPresenter Grid.Column="1" 
                    Content="{Binding CurrentView}" 
                    Classes="main-content" />
</Grid>

<!-- Style for small screens -->
<Style Selector="Grid:maxwidth(800) Border.sidebar">
  <Setter Property="IsVisible" Value="False" />
</Style>
```

## Design-Time Support

### XAML Design-Time Context
- Set up proper design-time context:

```xml
<!-- Good: Design-time context -->
<UserControl x:Class="MyApp.Views.CustomerView"
             xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             xmlns:design="using:MyApp.ViewModels.Design">
  
  <Design.DataContext>
    <design:DesignCustomerViewModel />
  </Design.DataContext>
  
  <!-- Rest of XAML -->
</UserControl>
```

## Layout Best Practices

### Grid Layouts
- Use proper Grid definitions:

```xml
<!-- Good: Clear grid structure -->
<Grid RowDefinitions="Auto,*,Auto" ColumnDefinitions="200,*,Auto">
  <!-- Header spanning all columns -->
  <Border Grid.Row="0" Grid.ColumnSpan="3" Classes="header">
    <TextBlock Text="Application Title" />
  </Border>
  
  <!-- Sidebar -->
  <Border Grid.Row="1" Grid.Column="0" Classes="sidebar">
    <!-- Navigation content -->
  </Border>
  
  <!-- Main content -->
  <ContentPresenter Grid.Row="1" Grid.Column="1" 
                    Content="{Binding MainContent}" />
  
  <!-- Tools panel -->
  <Border Grid.Row="1" Grid.Column="2" Classes="tools">
    <!-- Tool buttons -->
  </Border>
  
  <!-- Status bar -->
  <Border Grid.Row="2" Grid.ColumnSpan="3" Classes="status-bar">
    <TextBlock Text="{Binding StatusMessage}" />
  </Border>
</Grid>
```

### StackPanel and DockPanel Usage
- Use appropriate layout panels:

```xml
<!-- Good: StackPanel for simple vertical/horizontal layouts -->
<StackPanel Orientation="Vertical" Spacing="8">
  <TextBlock Text="Form Title" Classes="form-title" />
  <TextBox Watermark="Enter name..." />
  <TextBox Watermark="Enter email..." />
  <Button Content="Submit" Classes="primary" />
</StackPanel>

<!-- Good: DockPanel for complex layouts -->
<DockPanel>
  <Border DockPanel.Dock="Top" Classes="toolbar">
    <!-- Toolbar content -->
  </Border>
  <Border DockPanel.Dock="Bottom" Classes="status-bar">
    <!-- Status bar content -->
  </Border>
  <Border DockPanel.Dock="Left" Classes="sidebar">
    <!-- Sidebar content -->
  </Border>
  <!-- Main content fills remaining space -->
  <ContentPresenter Content="{Binding MainContent}" />
</DockPanel>
```

## Accessibility and Usability

### Accessibility Support
- Implement proper accessibility features:

```xml
<!-- Good: Accessibility attributes -->
<Button Content="Save Document"
        AutomationProperties.Name="Save Document"
        AutomationProperties.HelpText="Saves the current document to disk"
        ToolTip.Tip="Save (Ctrl+S)" />

<TextBox Text="{Binding CustomerName}"
         AutomationProperties.Name="Customer Name"
         AutomationProperties.LabeledBy="{Binding #CustomerNameLabel}" />

<TextBlock x:Name="CustomerNameLabel" 
           Text="Customer Name:" />
```

### Keyboard Navigation
- Ensure proper keyboard navigation:

```xml
<!-- Good: Tab order and keyboard support -->
<StackPanel>
  <TextBox TabIndex="1" Text="{Binding FirstName}" />
  <TextBox TabIndex="2" Text="{Binding LastName}" />
  <TextBox TabIndex="3" Text="{Binding Email}" />
  <Button TabIndex="4" Content="Save" Command="{Binding SaveCommand}" />
  <Button TabIndex="5" Content="Cancel" Command="{Binding CancelCommand}" />
</StackPanel>
```

# End of Kiro Steering File