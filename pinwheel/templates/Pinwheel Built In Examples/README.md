The templates in this folder are the ones that ship internally with Pinwheel.

Note that for ones with "Base" in their name the "COLOR_FORMAT_TAG" string is replaced at runtime with the appropriate color format tag depending on the build task that is running. We do this to avoid stencil duplication, but you can just replace this with any of our color format tags instead:
```
colorAsFloatRGBA, 
colorAsFloatHSVA,
colorAsCSSHexRGB, 
colorAsCSSRgb, 
colorAsCSSColor, 
colorAsCSSOklch,
colorAsHex, 
colorAsHexRGBA, 
colorAsHexARGB,
colorAsSwiftUIColor, 
colorAsSwiftUIKitColor, 
colorAsSwiftAppKitColor, 
colorAsSwiftUIColorHSBA, 
colorAsSwiftUIKitColorHSBA, 
colorAsSwiftAppKitColorHSBA
```