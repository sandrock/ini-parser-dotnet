
INI File Parser
========================

A .NET, Mono and Unity3d compatible(*) library for reading/writing INI data from IO streams, file streams, and strings written in C#.

Also implements merging operations, both for complete ini files, sections, or even just a subset of the keys contained by the files.

(*) This library is 100% .NET code and does not have any dependencies on Windows API calls in order to be portable.

Install it with NuGet: https://www.nuget.org/packages/ini-parser-new/

Maintainer note
--------------------------

[I will maintain](https://github.com/sandrock/ini-parser-dotnet) the current fork of [rickyah's ini-parser library](https://github.com/rickyah/ini-parser).

Feel free to open issues and PR. 

Actions:

- [x] publish version 2.6 with support for both net4.0 and netstandard2.0
- [ ] integrate urgent PRs
- [ ] review sources, tests, conventions, names; and publish v3.0
- [ ] open repo to more contributors

Documentation: [is in the upstream repo](https://github.com/rickyah/ini-parser/wiki)


Changelog
--------------------------

### vNext

See [WIP issues](https://github.com/sandrock/ini-parser-dotnet/labels/WIP)


### v2.6

Support for `net40` and `netstandard2.0`.


### Version 2

Since the INI format isn't really a "standard", version 2 introduces a simpler way to customize INI parsing:

- Pass a configuration object to an `IniParser`, specifying the behaviour of the parser. A default implementation is used if none is provided. 
- Derive from `IniDataParser` and override the fine-grained parsing methods.


Use
--------------------------

### Installation

The library is published to NuGet and can be installed on the command-line from the directory containing your solution.


### Getting Started

All code examples expect the following using clauses:

```csharp
using IniParser;
using IniParser.Configuration;
```

INI data is stored in nested dictionaries, so accessing the value associated to a key in a section is straightforward. Load the data using one of the provided methods.

```csharp
var parser = new FileIniDataParser();
IniData data = parser.ReadFile("Configuration.ini");
```

Retrieve the value for a key inside of a named section. Values are always retrieved as `string`s.

```csharp
string useFullScreenStr = data["UI"]["fullscreen"];
// useFullScreenStr contains "true"
bool useFullScreen = bool.Parse(useFullScreenStr);
```

Modify the value in the dictionary, not the value retrieved, and save to a new file or overwrite.

```csharp
data["UI"]["fullscreen"] = "true";
parser.WriteFile("Configuration.ini", data);
```

Head to the [wiki](https://github.com/rickyah/ini-parser/wiki) for more usage examples, or [check out the code of the example project](https://github.com/rickyah/ini-parser/blob/development/src/IniFileParser.Example/Program.cs)


### Merging ini files

Merging ini files is a one-method operation:

```csharp
var parser = new IniParser.Parser.IniDataParser();

IniData config = parser.Parse(File.ReadAllText("global_config.ini"));
IniData user_config = parser.Parse(File.ReadAllText("user_config.ini"));
config.Merge(user_config);

// config now contains that data from both ini files, and the values of
// the keys and sections are overwritten with the values of the keys and
// sections that also existed in the user config file
```

Keep in mind that you can merge individual sections if you like:

```csharp
config["user_settings"].Merge(user_config["user_settings"]);
```

### Comments

The library allows modifying the comments from an ini file. 
However note than writing the file back to disk, the comments will be rearranged so 
comments are written before the element they refer to.

To query, add or remove comments, access the property `Comments` available both in `SectionData` and `KeyData` models.

```csharp
var listOfCommentsForSection = config.["user_settings"].Comments;
var listOfCommentsForKey = config["user_settings"].GetKeyData("resolution").Comments;
```

### Unity3D

You can easily use this library in your Unity3D projects. Just drop either the code or the DLL inside your project's Assets folder and you're ready to go!

ini-parser is actually being used in [ProjectPrefs](http://u3d.as/content/garrafote/project-prefs/5so) a free add-on available in the Unity Assets Store that allows you to set custom preferences for your project. I'm not affiliated with this project: Kudos to Garrafote for making this add-on.


## Contributing

Do you have an idea to improve this library, or did you happen to run into a bug? Please share your idea or the bug you found in the issues page, or even better: feel free to fork and contribute to this project with a Pull Request.
