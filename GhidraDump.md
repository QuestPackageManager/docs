
### Running Il2CppInspector

For Unity 6000.0.40f1, build Il2CppInspector from [this branch](https://github.com/StackDoubleFlow/Il2CppInspector/tree/dev/stack/il2cpp_31_0_fix).

WIP

### Building a debug dummy libil2cpp.so

Template: 3D (Built-In Render Pipeline)
- This is the smallest template available

Build Profiles (Platforms -> Android):
- Switch Platform (Top Right)
- Debug Symbols -> Debugging (Full)
- Development Build -> False

Project Settings -> Player -> Identification
- Package Name -> `com.example.test` (can be anything)
- Minimum API Level -> Android 12L (API level 32)
- Target API Level -> Android 12L (API level 32)

Project Settings -> Player -> Configuration
- Scripting Backend -> IL2CPP
- Use incremental GC -> False
- Target Architectures -> ARM64 (only)
- IL2CPP Code Generation -> Faster runtime
- C++ Compiler Configuration -> Master

Create a new script `Assets/Editor/CustomPlayerSettings.cs`:
```c#
using UnityEditor;

public class CustomPlayerSettings
{
    [MenuItem("Tools/Set Custom IL2CPP Settings")]
    public static void SetCustomPlayerSettings()
    {
        PlayerSettings.SetAdditionalIl2CppArgs("--compiler-flags=\"-flto=thin\" --linker-flags=\"-flto=thin\"");
        UnityEngine.Debug.Log(PlayerSettings.GetAdditionalIl2CppArgs());
    }
}
```
Then, run it from the Editor from the menu bar dropdown: Tools -> Set Custom IL2CPP Settings

Build Profiles (Platforms -> Android):
- Build (Top Right)
- Debug `libil2cpp.so` should be in the generated `*.symbols.zip`.
- Metadata file should be in the output apk file at `assets/bin/Data/Managed/Metadata/global-metadata.dat`

### Obtaining `xref_gen` and `xref_apply`

```sh
cargo install --locked --git https://github.com/StackDoubleFlow/xref_gen
cargo install --locked --git https://github.com/StackDoubleFlow/xref_apply
```

### Running `xref_gen` and `xref_apply`

WIP