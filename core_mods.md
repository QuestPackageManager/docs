# Core mod list

## Unity/IL2CPP core mods

These core mods are generic purpose libraries to facilitate modding Unity IL2CPP games. They are currently designed for Android IL2CPP (arm64v8-a) targets, though that can be changed. 

### Flamingo
Flamingo is the in-house ARM64 hooking library for Quest modding. It offers priorities, uninstalls and recompiles, allowing mods to improve compatibility and reduce inconsistency between runs.


### BS-Hooks 
BS-Hooks is the core library which facilitates interfacing with the IL2CPP runtime and hooking it using Flamingo as a backend. This shared library attempts to support multiple IL2CPP versions though does so as a low priority goal.

It provides functionality for finding types and for those fields, methods, properties. Additionally, it provides mechanisms for finding methods based on overloading and inheritance, then invoking them and doing the according invoke logic for proper calling e.g boxing/unboxing params/returns, virtual table lookups and method slot resolving for interfaces. 

This makes it incredibly important for the foundation C++ IL2CPP functionality in Quest modding. However, this is also one of the most error prone and most complicated libraries due to the unsafe low level nature. Additionally, it depends on using xrefs and pattern matching to provide certain functionality that libil2cpp does not export, such as GC allocs and various internal il2cpp runtime functions. This needs to be adjusted every libil2cpp runtime update.

### Quest-Hook-RS 
The rust port of BS-Hooks, providing various utility functions to interface with IL2CPP. Additionally, it provides hook abstractions for defining hook functions for IL2CPP types+methods.

In contrast to BS-Hooks, this is statically linked and uses Cargo's `features` attributes to allow targetting various versions and targets for IL2CPP e.g Windows x64 out of the box.

More information can be found at the github here:
[QuestPackageManager/quest-hook-rs](https://github.com/QuestPackageManager/quest-hook-rs)

### Custom types
Custom types is an optional library that allows defining IL2CPP types during runtime using unsafe magic. This allows creating MonoBehaviours and implementation for interfaces among other things.


This requires updating when a IL2CPP runtime update occurs.

### Cordl
Cordl is a tool which generates bindings from the IL2CPP metadata, creating type safe interfaces for C# types. It currently supports JSON, C++ and Rust outputs.

This requires updating when a IL2CPP runtime update occurs.

## Beat Saber Core mods

### SongCore
### SongDownloader
### Playlist Manager
### MetaCore