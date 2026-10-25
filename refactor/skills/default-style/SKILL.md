---
name: default-style
description: "Leaves style and color optional so the built-in default applies, and copies a non-null default from that widget with a source comment. Use when a widget requires a color, style, or similar value the caller must pass every time."
---

# default-style


## Examples

### A required style the built-in accepts as null

**Input (Error State):**
```dart
// style is required, so the text cannot follow DefaultTextStyle
class TitleText extends StatelessWidget {
  const TitleText(this.text, {required this.style});

  final String text;
  final TextStyle style;

  @override
  Widget build(BuildContext context) {
    return Text(text, style: style);
  }
}
```

**Output (Resolved State):**
```dart
// A null style lets Text use DefaultTextStyle.of(context)
class TitleText extends StatelessWidget {
  const TitleText(this.text, {this.style});

  final String text;
  final TextStyle? style;

  @override
  Widget build(BuildContext context) {
    return Text(text, style: style);
  }
}
```

### A value the built-in does not accept as null

**Input (Error State):**
```dart
// elevation is required even though Material already has a default
class Surface extends StatelessWidget {
  const Surface({required this.elevation, required this.child});

  final double elevation;
  final Widget child;

  @override
  Widget build(BuildContext context) {
    return Material(elevation: elevation, child: child);
  }
}
```

**Output (Resolved State):**
```dart
// Copy the non-null default and name the widget it came from
class Surface extends StatelessWidget {
  const Surface({
    this.elevation = 0.0, // Material.elevation
    required this.child,
  });

  final double elevation;
  final Widget child;

  @override
  Widget build(BuildContext context) {
    return Material(elevation: elevation, child: child);
  }
}
```
