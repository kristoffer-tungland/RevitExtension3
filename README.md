# Error in VS Code's LanguageServerProjectSystem with Conditional Target Frameworks

## Issue Type: Bug

### Does this issue occur when all extensions are disabled?: No

- VS Code Version: Latest stable
- OS Version: Windows

## Steps to Reproduce:

1. Create a multi-configuration project structure with conditional target frameworks:
   - Main project file that imports a common targets file
   - Common targets file that conditionally imports version-specific targets based on configuration
   - Version-specific targets files with different target frameworks (net48 vs net8.0-windows)

2. Open the project in VS Code with C# Dev Kit or C# extension enabled

3. Observe the error in Output panel: "The 'ResolvePackageAssets' task was not given a value for the required parameter 'TargetFramework'"

## Expected Behavior
The language server should load the project successfully and provide IntelliSense based on the selected build configuration.

## Actual Behavior
The language server fails to load the project with the error:
```
Error while loading c:\Users\...\RevitExtension3.csproj: The "ResolvePackageAssets" task was not given a value for the required parameter "TargetFramework".
```

## Workarounds Attempted
1. Added Directory.Build.props with default TargetFramework value - only works for .NET Framework but not .NET Core targets
2. Modified targets files to be more explicit about target framework selection
3. Command-line builds work correctly (`dotnet build -c "Debug 2025"`) but the language server still fails

## Relevant Files
- Project structure:
  ```
  RevitExtension3.csproj
  RevitExtension3Command.cs
  Directory.Build.props
  targets/
    CommonRevit.targets
    Revit2024.targets (net48)  
    Revit2025.targets (net8.0-windows)
  ```

## Extension Bisect Results
Issue is specific to the C# language server/C# Dev Kit when dealing with conditionally defined target frameworks.

## Additional Information
- The issue only affects IDE tooling (IntelliSense, code navigation)
- Command-line builds work correctly
- Appears to be a limitation in how the language server resolves conditional imports and target frameworks

### Repo with test files
https://github.com/kristoffer-tungland/RevitExtension3