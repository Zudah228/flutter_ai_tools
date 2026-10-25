---
name: non-scroll-grid
description: "Lays out a grid that does not scroll with Wrap and FractionallySizedBox. When that grid is shrink-wrapped inside another scroll view, replaces the outer scroll view with CustomScrollView and SliverGrid. Use when GridView is given NeverScrollableScrollPhysics or shrinkWrap."
---

# non-scroll-grid


## Examples

### GridView that does not scroll

**Input (Error State):**
```dart
// NeverScrollableScrollPhysics forces a scroll widget into a non-scrolling layout
Column(
  children: [
    GridView.count(
      shrinkWrap: true,
      physics: const NeverScrollableScrollPhysics(),
      crossAxisCount: 2,
      children: const [Text('A'), Text('B')],
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// map wraps every child in FractionallySizedBox. widthFactor is 1 / crossCount. The Column keeps this layout from scrolling
const crossCount = 2;
Column(
  children: [
    Wrap(
      children: [
        const Text('A'),
        const Text('B'),
      ].map((child) {
        return FractionallySizedBox(
          widthFactor: 1 / crossCount,
          child: child,
        );
      }).toList(),
    ),
  ],
)
```

### A shrink-wrapped grid inside another scroll view

**Input (Error State):**
```dart
// shrinkWrap and NeverScrollableScrollPhysics nest a grid inside another scroll view
SingleChildScrollView(
  child: Column(
    children: [
      const Text('Title'),
      GridView.count(
        shrinkWrap: true,
        physics: const NeverScrollableScrollPhysics(),
        crossAxisCount: 2,
        children: const [Text('A'), Text('B')],
      ),
    ],
  ),
)
```

**Output (Resolved State):**
```dart
// SliverGrid belongs in one CustomScrollView, which replaces the surrounding scrollable
CustomScrollView(
  slivers: const [
    SliverToBoxAdapter(
      child: Text('Title'),
    ),
    SliverGrid.count(
      crossAxisCount: 2,
      children: [Text('A'), Text('B')],
    ),
  ],
)
```
