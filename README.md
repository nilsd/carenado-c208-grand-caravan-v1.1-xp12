# Fixing Carenado Aircraft with SASL 2.6.x for X-Plane 12

## Problem

Older Carenado aircraft (designed for X-Plane 11) use the SASL 2.6.x plugin that fails to load in X-Plane 12. You may see errors like:

- `Error Code = 126: The specified module cannot be found`
- `Error Code = 193: Not a valid Win32 application`
- Switches and systems not responding

This is caused by missing or incompatible DLL dependencies.

## Solution

Replace the 32-bit `OpenAL32.dll` with a 64-bit version. X-Plane 12 is 64-bit only and cannot load 32-bit DLLs.

## Installation Steps

### 1. Download 64-bit OpenAL

- Go to https://openal-soft.org/openal-binaries/
- Download the latest binary package (e.g., `openal-soft-1.24.3-bin.zip`)
- Extract the ZIP file

### 2. Locate the 64-bit DLL

- Navigate to the extracted folder
- Go to `bin\Win64\`
- Find `soft_oal.dll`

### 3. Install the DLL

- Copy `soft_oal.dll` to your aircraft's plugin folder:
  ```
  [Aircraft Folder]\plugins\sasl\64\
  ```
- Rename it to `OpenAL32.dll`
- Replace/overwrite if a file already exists

### 4. Test

- Load the aircraft in X-Plane 12
- Verify all switches and systems work properly
- Check the X-Plane Log.txt for any SASL errors

## Verification

To verify the DLL is 64-bit, you can use the Windows `file` command or check the file properties:

```bash
file plugins/sasl/64/OpenAL32.dll
```

Should show: `PE32+ executable ... x86-64` (PE32+ indicates 64-bit)

## Troubleshooting

| Error | Cause | Solution |
|-------|-------|----------|
| **Error 126** | DLL not found | Ensure `OpenAL32.dll` is in the `plugins\sasl\64\` subfolder (not the parent `sasl` folder) |
| **Error 193** | Wrong architecture | Verify you copied the 64-bit version from the `Win64` folder, not `Win32` |
| **Module not detected** | Wrong SASL version | Use the original SASL 2.6.x plugin, not SASL 3.x (which cannot read `.sec` files) |
| **Systems don't work** | SASL not loading | Check X-Plane Log.txt for specific error messages |

## Technical Details

### Why This Fix Works

1. **SASL 2.6.x** uses encrypted `.sec` files for aircraft logic
2. **SASL 3.x** cannot read `.sec` files, only plain `.lua` files
3. The original SASL 2.6.x shipped with a 32-bit `OpenAL32.dll`
4. X-Plane 12 is 64-bit only and requires 64-bit DLLs
5. Replacing with a 64-bit OpenAL allows SASL 2.6.x to load in X-Plane 12

### File Locations

```
Aircraft/
└── Carenado/
    └── [Aircraft Name]/
        └── plugins/
            └── sasl/
                ├── OpenAL32.dll (32-bit - original, not used in XP12)
                └── 64/
                    ├── win.xpl (64-bit SASL plugin)
                    └── OpenAL32.dll (64-bit - THIS is what XP12 needs)
```

## Applies To

- Carenado aircraft using SASL 2.6.x plugin
- X-Plane 11 aircraft being migrated to X-Plane 12
- Any aircraft with similar OpenAL dependency issues

## Credits

- OpenAL Soft: https://github.com/kcat/openal-soft
- SASL Plugin: https://1-sim.com/

## License

This guide is provided as-is for educational purposes. OpenAL Soft is LGPL-licensed. Respect the original aircraft and plugin licenses.
