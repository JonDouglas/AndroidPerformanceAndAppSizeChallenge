# Android Performance and App Size Challenge
Use the new Xamarin.Android features and open up an issue with your results

# Before you start

1. Enable [Diagnostic MSBuild Output](https://docs.microsoft.com/en-us/xamarin/android/troubleshooting/troubleshooting#diagnostic-msbuild-output) or use [MSBuild BinLog](http://msbuildlog.com/).
2. Open up the Android Device Log by going to `Tools > Android > Device Log`

## Build Times
- Enable aapt2.
- Enable D8/R8.
- Use Portable PDB.
- Enable Reference Assemblies.

Within a .csproj, add the following properties to your current configuration:

```
<PropertyGroup>
    <AndroidUseAapt2>true</AndroidUseAapt2>
    <AndroidDexTool>d8</AndroidDexTool>
    <AndroidLinkTool>r8</AndroidLinkTool>
    <DebugType>portable</DebugType>
    <ProduceReferenceAssembly>True</ProduceReferenceAssembly>
</PropertyGroup>
```
## Startup Times

Startup Times should only be measured with a `Release` configuration. To measure the performance, with the Android Device Log, add a filter to search for `
- Enable Startup Tracing.
## Application Size
- Change the Linker settings to SDK and User Assemblies.
- Enable ProGuard or R8.
- Create an Android App Bundle.
