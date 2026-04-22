# Core Mod Update (Beat Saber)

This document outlines the process workflow to update core mods for Beat Saber (assuming the Unity update prerequirements are met)

## Prerequirements
- [QPM.CLI](https://github.com/QuestPackageManager/QPM.CLI)
- A **legal** copy of the game APK
- [cordl](https://github.com/QuestPackageManager/cordl)
- `apktool` (optional)

## Is a Unity update required?
Critera:
- IL2CPP version was changed (TODO: Provide tool to find version)
- IL2CPP runtime metadata was modified (TODO:)
- IL2CPP xrefs no longer work for reasons unrelated
- TODO:

If the criteria is met, follow the steps in [./core_update_unity.md] before continuing here.

## Generating Cordl
Beat Saber's Cordl headers are stored at https://github.com/QuestPackageManager/bs-cordl/. Luckily, this repository provides [a script](https://github.com/QuestPackageManager/bs-cordl/blob/main/generate.ps1) to generate headers from an APK:
```shell
./generate.ps1 <apk>
```
 
However, this process can be also be done manually. Essentially, you'll need to extract two files from the IL2CPP game.
- Android APK:
    - `<apk>/lib/arm64-v8a/libil2cpp.so`
    - `<apk>/assets/bin/Data/Managed/Metadata/global-metadata.dat`

Most archive extractors can read APK files, though `apktool` is what the script uses.

Run the command to generate `cpp` headers
```
cordl --metadata global-metadata.dat --libil2cpp libil2cpp.so cpp
```


TODO:

## Publishing Cordl
TODO:

Cordl versions are defined by the game's modding requirements. For Beat Saber modding, we use `{MAJOR:02}{MINOR:02}-{MAJOR-CORDL}-{MINOR-CORDL}` versioning scheme. 

As an example, we assume we make headers for 1.42.1, the version for cordl would be `4201.0.0`. If cordl headers were updated for the same game version, this would be `4201.1.0` for example.

## Unstripped LibUnity
[./unstripped_libunity](Guide) for retrieving and uploading unstripped libunity.so. You may skip to the PR section if the Unity version is already previously supported.

## Updating the mods

The following mods must be updated, and they require compilation in their dependency order. 

- SongCore
- MetaCore
- ...

Generally, you can update the mods by following this set of commands:
```
qpm restore
qpm qmod zip
qpm package edit --version <version> 
qpm install
```

Then in every project that depends on it, you may use the following commands to use your unreleased library. Remember to run `qpm restore` once you're done updating dependencies.
```
qpm dependency add <id> <version>
```

Now generally, you may make PRs for each mod's GitHub and assume the dependencies will be updated by the time the PR is merged. However, due to some QPM footguns affecting QMod linking, the PRs will have to be updated to use the `qpackage` published dependencies before merging.
