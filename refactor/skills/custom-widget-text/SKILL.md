---
name: custom-widget-text
description: "Accepts displayed copy as the caller's widget and shares style and textAlign through DefaultTextStyle. Use when a custom widget takes a label, title, or caption."
---

# custom-widget-text


## Examples

### Displayed copy is the caller's widget

**Input (Error State):**
```dart
// A String title is rebuilt as Text, so the caller cannot pass spans, style, or textAlign
class SectionTitle extends StatelessWidget {
  const SectionTitle({
    super.key,
    required this.title,
  });

  final String title;

  @override
  Widget build(BuildContext context) {
    return Text(
      title,
      style: const TextStyle(fontWeight: FontWeight.bold),
      textAlign: TextAlign.start,
    );
  }
}
```

**Output (Resolved State):**
```dart
// Accept the caller's widget. Merge the shared style and textAlign, and do not read its data to build a new Text
class SectionTitle extends StatelessWidget {
  const SectionTitle({
    super.key,
    required this.title,
  });

  final Widget title;

  @override
  Widget build(BuildContext context) {
    return DefaultTextStyle.merge(
      style: const TextStyle(fontWeight: FontWeight.bold),
      textAlign: TextAlign.start,
      child: title,
    );
  }
}
```
```dart
// Pass a Text for the title
const SectionTitle(
  title: Text('Settings'),
)
```
```dart
// Pass Text.rich when only part of the string changes weight or becomes a link
const SectionTitle(
  title: Text.rich(
    TextSpan(
      children: [
        TextSpan(text: 'Settings'),
        TextSpan(
          text: ' (details)',
          style: TextStyle(fontWeight: FontWeight.normal),
        ),
      ],
    ),
  ),
)
```

### Tooltip and semantics stay strings

**Input (Error State):**
```dart
// One String is used for the displayed copy, the tooltip, and the semantics label
class SectionTitle extends StatelessWidget {
  const SectionTitle({
    super.key,
    required this.title,
  });

  final String title;

  @override
  Widget build(BuildContext context) {
    return Tooltip(
      message: title,
      child: Text(
        title,
        style: const TextStyle(fontWeight: FontWeight.bold),
      ),
    );
  }
}
```

**Output (Resolved State):**
```dart
// Keep the displayed copy as the caller's widget. Pass tooltip and semanticLabel as their own strings. Build Tooltip only when tooltip is non-null
class SectionTitle extends StatelessWidget {
  const SectionTitle({
    super.key,
    required this.title,
    this.tooltip,
    this.semanticLabel,
  });

  final Widget title;
  final String? tooltip;
  final String? semanticLabel;

  @override
  Widget build(BuildContext context) {
    Widget child = DefaultTextStyle.merge(
      style: const TextStyle(fontWeight: FontWeight.bold),
      textAlign: TextAlign.start,
      child: title,
    );

    if (semanticLabel != null) {
      child = Semantics(
        label: semanticLabel,
        child: ,
      );
    }

    if (tooltip != null) {
      child = Tooltip(message: message, child: child);
    }

    return child;
  }
}
```
```dart
// The call site passes the Text and the two strings separately
const SectionTitle(
  title: Text('Settings'),
  tooltip: 'Open app settings',
  semanticLabel: 'Settings',
)
```
