---
name: minimum-tap-target
description: "Gives a button a 48px minimum tap target and keeps its label and padding inside the pressable area. Use when a button or GestureDetector is hard to press, the hit target is only the icon or the text, or padding sits outside the tap."
---

# minimum-tap-target


## Examples

### Tap target smaller than 48px

**Input (Error State):**
```dart
// The icon is the whole tap target, so the control is smaller than 48px
GestureDetector(
  onTap: onPressed,
  child: const Icon(Icons.close),
)
```

**Output (Resolved State):**
```dart
// minimumSize adds a 48px minimum tap target
IconButton(
  onPressed: onPressed,
  style: IconButton.styleFrom(
    minimumSize: const Size(48, 48),
  ),
  icon: const Icon(Icons.close),
)
```
```dart
// The parent ConstrainedBox supplies the 48px minimum, and the opaque gesture makes that box the tap target
ConstrainedBox(
  constraints: const BoxConstraints(minWidth: 48, minHeight: 48),
  child: GestureDetector(
    onTap: onPressed,
    behavior: HitTestBehavior.opaque,
    child: const Icon(Icons.close),
  ),
)
```

### Label outside the tap

**Input (Error State):**
```dart
// The text sits outside the gesture, so only the icon receives the press
Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    const Text('Add'),
    GestureDetector(
      onTap: onPressed,
      child: const Icon(Icons.add),
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// The label is inside the button, so the text is part of the tap
TextButton.icon(
  onPressed: onPressed,
  style: TextButton.styleFrom(
    minimumSize: const Size(48, 48),
  ),
  icon: const Icon(Icons.add),
  label: const Text('Add'),
)
```

### Padding outside the tap

**Input (Error State):**
```dart
// Padding is outside the gesture, so the gap is not part of the tap
Padding(
  padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
  child: GestureDetector(
    onTap: onPressed,
    child: const Text('Save'),
  ),
)
```

**Output (Resolved State):**
```dart
// The padding is inside the button, so the gap is part of the tap
TextButton(
  onPressed: onPressed,
  style: TextButton.styleFrom(
    padding: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
    minimumSize: const Size(48, 48),
  ),
  child: const Text('Save'),
)
```
```dart
// Padding is the child of the tap, and the box keeps that target at least 48px
Material(
  type: MaterialType.transparency,
  child: InkWell(
    onTap: onPressed,
    child: const ConstrainedBox(
      constraints: BoxConstraints(minWidth: 48, minHeight: 48),
      child: Padding(
        padding: EdgeInsets.symmetric(horizontal: 16, vertical: 12),
        child: Text('Save'),
      ),
    ),
  ),
)
```
