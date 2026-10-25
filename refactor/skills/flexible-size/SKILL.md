---
name: flexible-size
description: "Sizes a child from the parent constraint, Expanded, AspectRatio, or token spacing, and uses Padding or BoxConstraints for a minimum. Use when a widget sets a fixed width or height that text length or screen width can overflow."
---

# flexible-size


## Examples

### A fixed width and height

**Input (Error State):**
```dart
// A fixed size overflows when the text or the screen is wider
SizedBox(
  width: 120,
  height: 40,
  child: Text(label),
)
```

**Output (Resolved State):**
```dart
```dart
// AspectRatio sets the shape from the parent's bounded side, and the text fills that box
AspectRatio(
  aspectRatio: 16 / 9,
  child: Text(label),
)
```
```dart
// A spacing token is the gap. The text keeps the parent's width
Padding(
  padding: const EdgeInsets.all(16),
  child: Text(label),
)
```

### A minimum gap or size

**Input (Error State):**
```dart
// A fixed size is used to keep a minimum
SizedBox(
  width: 48,
  height: 48,
  child: child,
)
```

**Output (Resolved State):**
```dart
// The floor is a constraint, so the child can still grow
ConstrainedBox(
  constraints: const BoxConstraints(minWidth: 48, minHeight: 48),
  child: child,
)
```
```dart
// Padding keeps the minimum gap, and the child keeps the parent's width
Padding(
  padding: const EdgeInsets.all(8),
  child: child,
)
```
