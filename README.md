# Android Performance and App Size Challenge
Use the new Xamarin.Android features and open up an issue with your results

# Before you start

1. Enable [Diagnostic MSBuild Output](https://docs.microsoft.com/en-us/xamarin/android/troubleshooting/troubleshooting#diagnostic-msbuild-output) or use [MSBuild BinLog](http://msbuildlog.com/).
2. Open up the Android Device Log by going to `Tools > Android > Device Log`

## Build Times
- [Enable aapt2](https://devblogs.microsoft.com/xamarin/optimize-xamarin-android-builds/).
- [Enable D8](https://devblogs.microsoft.com/xamarin/androids-d8-dexer-and-r8-shrinker/).
- [Use Portable PDB](https://devblogs.microsoft.com/xamarin/optimize-xamarin-android-builds/).
- [Enable Reference Assemblies](https://devblogs.microsoft.com/xamarin/optimize-xamarin-android-builds/).

Within a .csproj, add the following properties to your current configuration:

```
<PropertyGroup>
    <AndroidUseAapt2>true</AndroidUseAapt2>
    <AndroidDexTool>d8</AndroidDexTool>
    <DebugType>portable</DebugType>
    <ProduceReferenceAssembly>True</ProduceReferenceAssembly>
</PropertyGroup>
```

Build Times should only be measured with a `Debug` configuration. To measure the performance, enable [Diagnostic MSBuild Output](https://docs.microsoft.com/en-us/xamarin/android/troubleshooting/troubleshooting#diagnostic-msbuild-output) or use [MSBuild BinLog](http://msbuildlog.com/) to provide the time elapsed. 

**Bonus:** If you can include a full MSBuild Output or MSBuild BinLog, this will help us understand where time is allocated to each target and task while building your app! This will help us understand bottlenecks and where to improve in the future.

**Note:** To do a **cold build**, `Clean` your project prior to building. To do an **incremental build**, change a single piece of code (XAML / C# / etc) and bil

**Before Cold Build Times Changes:**
```
Time Elapsed 00:00:15.77
```

**After Cold Build Times Changes:**
```
Time Elapsed 00:00:14.85
```

**Before Incremental Build Times Changes:**
```
Time Elapsed 00:00:02.67
```

**After Incremental Build Times Changes:**
```
Time Elapsed 00:00:02.43
```

## Startup Times

- [Enable Startup Tracing](https://devblogs.microsoft.com/xamarin/faster-startup-times-with-startup-tracing-on-android/).

Within a .csproj, add the following property to your release configuration:

```
<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|AnyCPU' "> 
    <AndroidEnableProfiledAot>true</AndroidEnableProfiledAot> 
</PropertyGroup>
```

Startup Times should only be measured with a `Release` configuration. To measure the performance, with the Android Device Log, add a filter to search for `ActivityManager: Displayed` to which you will be provided a timing of your application such as:

**Before Startup Tracing:**
```
2019-08-19 09:35:43.374 1870-1892/? I/ActivityManager: Displayed com.companyname.startuptiming/md5984e4549386b302a2fd47914e0849f36.MainActivity: +2s995ms
```

**After Startup Tracing:**
```
2019-08-19 09:37:40.504 1870-1892/? I/ActivityManager: Displayed com.companyname.startuptiming/md5984e4549386b302a2fd47914e0849f36.MainActivity: +1s760ms
```

## Application Size
- [Enable d8](https://devblogs.microsoft.com/xamarin/androids-d8-dexer-and-r8-shrinker/).
- [Enable ProGuard](https://docs.microsoft.com/en-us/xamarin/android/deploy-test/release-prep/proguard) or [R8](https://devblogs.microsoft.com/xamarin/androids-d8-dexer-and-r8-shrinker/).
- [Create an Android App Bundle](https://github.com/xamarin/xamarin-android/blob/master/Documentation/guides/app-bundles.md).

Application size should be measured with a `Release` configuration and signed package. To measure the perceived app download size, you can use [Android Studio's APK Analyzer](https://developer.android.com/studio/build/apk-analyzer) or with the [following command](https://developer.android.com/studio/command-line/apkanalyzer) in your `Android SDK/tools/bin` location:

```
apkanalyzer -h apk download-size myapk.apk
```

or

```
apkanalyzer -h apk download-size myaab.aab
```

**Note:** [apkanalyzer may not work on Windows](https://issuetracker.google.com/issues/75981301).

**Before Application Size Changes:**
```
App download size: 23.6MB
```

**After Application Size Changes:**
```
App download size: 4.82MB
```
