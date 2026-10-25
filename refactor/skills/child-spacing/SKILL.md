---
name: child-spacing
description: "Spaces Column and Row children with spacing, and uses margin when elevation needs room. Use when vertical padding separates Column children, horizontal padding separates Row children, or a shadow is given room with Padding."
---

# child-spacing


## Examples

### Vertical padding between column children

**Input (Error State):**
```dart
// Vertical padding is the gap between Column children
Column(
  children: const [
    Padding(
      padding: EdgeInsets.symmetric(vertical: 8),
      child: Text('A'),
    ),
    Padding(
      padding: EdgeInsets.symmetric(vertical: 8),
      child: Text('B'),
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// spacing is the gap. A widget split out only to add that padding is the same mistake
Column(
  spacing: 8,
  children: const [
    Text('A'),
    Text('B'),
  ],
)
```

### Horizontal padding between row children

**Input (Error State):**
```dart
// Horizontal padding is the gap between Row children
Row(
  children: const [
    Padding(
      padding: EdgeInsets.symmetric(horizontal: 8),
      child: Text('A'),
    ),
    Padding(
      padding: EdgeInsets.symmetric(horizontal: 8),
      child: Text('B'),
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// spacing is the gap along the row
Row(
  spacing: 8,
  children: const [
    Text('A'),
    Text('B'),
  ],
)
```

### Room for elevation

**Input (Error State):**
```dart
// Padding is used to keep the shadow from being clipped
Padding(
  padding: const EdgeInsets.all(8),
  child: Material(
    elevation: 4,
    child: const Text('Card'),
  ),
)
```

**Output (Resolved State):**
```dart
// The room an elevation requires is margin
Container(
  margin: const EdgeInsets.all(8),
  child: const Material(
    elevation: 4,
    child: Text('Card'),
  ),
)
```
