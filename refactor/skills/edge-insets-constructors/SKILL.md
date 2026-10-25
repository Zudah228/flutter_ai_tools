---
name: edge-insets-constructors
description: "Builds EdgeInsets with symmetric, only, operators, or copyWith. Use when padding uses fromLTRB, repeats equal vertical or horizontal sides, or rebuilds each side to add, scale, or replace an edge."
---

# edge-insets-constructors


## Examples

### Equal vertical and horizontal sides

**Input (Error State):**
```dart
// Left matches right and top matches bottom, but the four sides are written out
padding: const EdgeInsets.fromLTRB(16, 8, 16, 8),
```

**Output (Resolved State):**
```dart
// The vertical pair and the horizontal pair are each one value, so symmetric names those axes
padding: const EdgeInsets.symmetric(vertical: 8, horizontal: 16),
```

### Adding an inset

**Input (Error State):**
```dart
// Each side is added by hand, repeating padding and the extra inset
padding: EdgeInsets.fromLTRB(
  padding.left + 8,
  padding.top + 4,
  padding.right + 8,
  padding.bottom + 4,
),
```

**Output (Resolved State):**
```dart
// + adds an EdgeInsets. The extra vertical and horizontal pairs match, so that inset is symmetric
padding: padding + const EdgeInsets.symmetric(vertical: 4, horizontal: 8),
```

### Subtracting an inset

**Input (Error State):**
```dart
// Each side is subtracted by hand, repeating padding and the removed inset
padding: EdgeInsets.fromLTRB(
  padding.left - 8,
  padding.top - 4,
  padding.right - 8,
  padding.bottom - 4,
),
```

**Output (Resolved State):**
```dart
// - subtracts an EdgeInsets. The removed vertical and horizontal pairs match, so that inset is symmetric
padding: padding - const EdgeInsets.symmetric(vertical: 4, horizontal: 8),
```

### Multiplying every edge

**Input (Error State):**
```dart
// Each side is scaled by hand
padding: EdgeInsets.fromLTRB(
  padding.left * 2,
  padding.top * 2,
  padding.right * 2,
  padding.bottom * 2,
),
```

**Output (Resolved State):**
```dart
// * multiplies every edge by the same factor
padding: padding * 2,
```

### Dividing every edge

**Input (Error State):**
```dart
// Each side is divided by hand
padding: EdgeInsets.fromLTRB(
  padding.left / 2,
  padding.top / 2,
  padding.right / 2,
  padding.bottom / 2,
),
```

**Output (Resolved State):**
```dart
// / divides every edge by the same factor
padding: padding / 2,
```

### Integer-dividing every edge

**Input (Error State):**
```dart
// Each side is integer-divided by hand, then converted back to double
padding: EdgeInsets.fromLTRB(
  (padding.left ~/ 2).toDouble(),
  (padding.top ~/ 2).toDouble(),
  (padding.right ~/ 2).toDouble(),
  (padding.bottom ~/ 2).toDouble(),
),
```

**Output (Resolved State):**
```dart
// ~/ integer-divides every edge
padding: padding ~/ 2,
```

### Replacing one edge

**Input (Error State):**
```dart
// One edge changes, but the other three are copied out and passed back in
padding: EdgeInsets.fromLTRB(
  padding.left,
  padding.top,
  padding.right,
  24,
),
```

**Output (Resolved State):**
```dart
// copyWith replaces the named edges and keeps the rest
padding: padding.copyWith(bottom: 24),
```

### Positional fromLTRB

**Input (Error State):**
```dart
// fromLTRB takes positional edges, so the sides are unnamed
padding: const EdgeInsets.fromLTRB(16, 8, 4, 24),
```

**Output (Resolved State):**
```dart
// only names left, top, right, and bottom
padding: const EdgeInsets.only(left: 16, top: 8, right: 4, bottom: 24),
```
