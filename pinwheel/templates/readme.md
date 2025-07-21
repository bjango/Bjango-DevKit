Pinwheel’s [exporting](https://bjango.com/help/pinwheel/exporting/) supports a wide range of common formats. If you need a format we don’t support, a custom template can be used.

Custom templates use the [Stencil template language](https://stencil.fuller.li/en/latest/templates.html). Most of Pinwheel’s built-in exporting uses Stencil templates, and are available in our [Bjango DevKit GitHub repository](https://github.com/bjango/Bjango-DevKit/tree/main/pinwheel/templates/Pinwheel%20Built%20In%20Examples). Feel free to use them as the basis of your own templates, or to learn how templates work.


### Main structure

The data structure available in templates is a list of `items` at the top level. There is also a `exportFileName` item that corresponds to the file name in the build task. You can iterate through all the items like this:

```
{% for item in items %}
```

Each item has a `name` and can be one of 7 types a `colorItem`, `colorSet`, `number`, `string`, `boolean`, `shadowItem` or `shadowSet`. If you wanted to see if an item was a `colorItem` you can use this syntax:

```
{% if item.colorItem %}
```

And, if you wanted to see if it is a number, you'd use:

```
{% if item.number %}
```


### Appearance, contrast, and color formatters

A `colorItem` has a `color` which can have `light`, `dark`, `highContrast` and `darkHighContrast` appearances. If a color has no appearances, then only the `light` value will be set. You can access it like this:

```
{% colorAsCSSColor item.colorItem.color.light %}
```

The formatter above is a custom tag. There are lots of format options available for colors. They correspond to all the formats you can use in the Pinwheel color picker:

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

Additionally there is a formatter which can generate `light-dark()` function calls to use in CSS, called `lightDark`. You can use it like this:
```
{%+ lightDark color.light color.dark cssColor +%}
```
There are 3 parameters to this formatter, the first two are required, the last is optional
1. The light color you want to use
2. The dark color you want to use
3. The CSS format you want these colors to be output in. This can be `cssHex` (eg: `#FFFFFF`), `cssRgb` (eg: `rgb(255, 255, 255)`), `cssColor` (eg: `color(srgb 1 1 1)`) or `cssOKLCH` (eg: `oklch(100.0% 0.0 89.9 / 1.0)`). If none is specified `cssColor` will be used.


### Code safe names

There is also a formatter for creating safe variable names in most programming languages (things like making sure it doesn't start with a number, removing spaces, etc):
```
codeSafeName
```

We also have formatters for shadows:
```
shadowAsCSSBoxShadow
shadowAsSwiftUIPinwheelShadow
shadowAsSwiftAppKitPinwheelShadow
shadowAsSwiftUIKitPinwheelShadow
shadowAsKotlinPinwheelShadow
```

You can combine multiple formatters, for example say you wanted to create a SwiftUI Color variable in a template, from a color:
```
let {%+ codeSafeName color.name +%} = {%+ colorAsSwiftUIColor color.light +%}
```


### Numbers, strings, and booleans

For `number`, `string` and `boolean` types you can access the `value` of each in a similar way:

```
{{ number.value.light }}
{{ boolean.value.light }}
{{ string.value.light }}
```

Anything you type into the template that’s outside of `{{}}` and `{%%}` tags will come out as text. For example:

```
{% for item in items %}
This item is called {{ item.name }}.
{% endfor %}
```

In a document with 3 items: Yellow, Green and Important would print:
```
This item is called Yellow.
This item is called Green.
This item is called Important.
```


### Full example

The full data structure is below for the technically minded:

```swift
struct TemplateItem: Encodable {
    let name: String
    var colorItem: TemplateColorItem?
    var colorSet: TemplateColorSetItem?
    var number: TemplateNumberItem?
    var boolean: TemplateBooleanItem?
    var string: TemplateStringItem?
}

struct TemplateColorSetItem: Encodable {
    let colors: [TemplateThemedColor]
}

struct TemplateStringItem: Encodable {
    let value: TemplateThemedString
}

struct TemplateNumberItem: Encodable {
    let value: TemplateThemedNumber
}

struct TemplateBooleanItem: Encodable {
    let value: TemplateThemedBoolean
}

struct TemplateColorItem: Encodable {
    var color: TemplateThemedColor
}

struct TemplateThemedColor: Encodable {
    var light: TemplateRGBAColor
    var highContrast: TemplateRGBAColor?
    var dark: TemplateRGBAColor?
    var darkHighContrast: TemplateRGBAColor?
    var name: String?
}

struct TemplateThemedString: Encodable {
    var light: String
    var highContrast: String?
    var dark: String?
    var darkHighContrast: String?
}

struct TemplateThemedNumber: Encodable {
    var light: Double
    var highContrast: Double?
    var dark: Double?
    var darkHighContrast: Double?
}

struct TemplateThemedBoolean: Encodable {
    var light: Bool
    var highContrast: Bool?
    var dark: Bool?
    var darkHighContrast: Bool?
}

struct TemplateRGBAColor: Encodable {
    let red: Double
    let green: Double
    let blue: Double
    let alpha: Double
    let colorSpace: TemplateColorSpace
}

enum TemplateColorSpace: String, Encodable {
    case sRGB, displayP3, extendedSRGB
}
```
