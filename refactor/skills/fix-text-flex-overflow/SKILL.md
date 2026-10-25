---
name: fix-text-flex-overflow
description: "Gives overflowing text in a Row a bounded width with Flexible or Expanded. Use when a Row throws \"A RenderFlex overflowed\"."
---

# fix-text-flex-overflow


## Examples

### Text overflows a Row

**Input (Error State):**
```dart
// Throws "A RenderFlex overflowed by X pixels on the right"
Row(
  children: [
    const Icon(Icons.info),
    const Text('This is a very long text string that will definitely overflow the available screen width and cause a RenderFlex error.'),
  ],
)
```

**Output (Resolved State):**
```dart
// Prefer MainAxisSize.min and Flexible unless Expanded is intended, so the parent widget's behavior stays unchanged
Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    const Icon(Icons.info),
    const Flexible(
      child: Text('This is a very long text string that will definitely overflow the available screen width and cause a RenderFlex error.'),
    ),
  ],
)
```

```dart
// Wrap the Text widget in Expanded.
Row(
  children: [
    const Icon(Icons.info),
    const Expanded(
      child: Text('This is a very long text string that will definitely overflow the available screen width and cause a RenderFlex error.'),
    ),
  ],
)
```
