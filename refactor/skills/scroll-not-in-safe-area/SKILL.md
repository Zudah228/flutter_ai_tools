---
name: scroll-not-in-safe-area
description: "Paints the background and the scroll view to the screen edge, and puts the inset that remains when scrolling stops into the scroll padding. Use when wrapping a scroll view in SafeArea or Padding, placing SafeArea in a list tile, or setting clipBehavior to Clip.none."
---

# scroll-not-in-safe-area


## Examples

### The scroll view paints to the screen edge

**Input (Error State):**
```dart
// SafeArea around the scroll view clips the edge and the shadow
SafeArea(
  child: ListView(
    padding: const EdgeInsets.all(16),
    children: const [
      Text('Item'),
    ],
  ),
)
```

**Output (Resolved State):**
```dart
// Paint the background and the scroll view to the screen edge. Put MediaQuery.paddingOf in the scroll padding
return ListView(
  padding: const EdgeInsets.all(16) + MediaQuery.paddingOf(context),
  children: const [
    Text('Item'),
  ],
);
```

### A clipped shadow

**Input (Error State):**
```dart
// Outer Padding clips the card shadow. clipBehavior: Clip.none does not put the inset back on the scroll view
Padding(
  padding: const EdgeInsets.all(16),
  child: ListView(
    clipBehavior: Clip.none,
    children: const [
      Card(
        elevation: 4,
        child: ListTile(title: Text('Item')),
      ),
    ],
  ),
)
```

**Output (Resolved State):**
```dart
// Move the outer padding onto the scroll view and add the system insets on every edge
return ListView(
  padding: const EdgeInsets.all(16) + MediaQuery.paddingOf(context),
  children: const [
    Card(
      elevation: 4,
      child: ListTile(title: Text('Item')),
    ),
  ],
);
```
