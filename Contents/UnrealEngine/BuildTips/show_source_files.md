In my current project, I need to add a third party library with source files. Before this, I only have expericence that adding built library such as .lib files. This time, I create a new folder and module files, i.e. IXXLib.h， XXLib.cpp and XXLib.build.cs, under the path *'UnrealEngine\Engine\Plugins\MyPlugin\Source\ThirdParty\XXLib'* because I don't want to expose it to other modules explicitly. But After I regenerating the project file, I didn't find any files under the folder except those three module files.

In default, source files will be discovered recursively. There should be no reason to be empty. So I debuged into the UnrealBuildTool([see details here]) and found a wordless things that UBT will check if the module path contains *'ThirdParty'* in order to determine to add source files or not. The related codes are in ProjectFileGenerator.cs, Line 1961:

```C++
// We'll keep track of whether this is an "engine" or "external" module.  This is determined below while loading module rules.
bool IsThirdPartyModule = CurModuleFile.ContainsNam("ThirdParty", UnrealBuildTool.RootDirectory);
```
and Line 1995:
```C++
bool SearchSubdirectories = !IsThirdPartyModule || bGatherThirdPartySource;

if( bGatherThirdPartySource )
{
    Log.TraceInformation( "Searching for third-party source files..." );
}

// Find all of the source files (and other files) and add them to the project
List<FileReference> FoundFiles = SourceFileSearch.FindModuleSourceFiles( CurModuleFile, SearchSubdirectories:SearchSubdirectories );
```

Hence, we have two ways to solve this problem. The intuitive way is modifying the path of the third party library such as replacing *'ThirdParty'* to *'External'* and promising that there are no more *'ThirdParty'* words in the path. The other ways is adding an argument `-THIRDPARTY` when executing GenerateProjectFiles.bat. You can find the source at Line 1127:
```C++
case "-THIRDPARTY":
    bGatherThirdPartySource = true;
    break;
```