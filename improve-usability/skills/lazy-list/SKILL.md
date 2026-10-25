---
name: lazy-list
description: "Keeps a long list lazy with CustomScrollView and SliverList.builder, or with a ListView as the top scroll view. Use when ListView.builder sets shrinkWrap so it can sit in a Column: that builds every item, essentially the same as placing them directly in Column.children."
---

# lazy-list


## Examples

### shrinkWrap inside a Column

**Input (Error State):**
```dart
// shrinkWrap builds every item, essentially the same as placing them directly in Column.children
Column(
  children: [
    const Text('Title'),
    ListView.builder(
      shrinkWrap: true,
      itemCount: items.length,
      itemBuilder: (context, index) => Text(items[index]),
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// SliverList.builder builds only the children inside the scroll viewport
CustomScrollView(
  slivers: [
    const SliverToBoxAdapter(
      child: Text('Title'),
    ),
    SliverList.builder(
      itemCount: items.length,
      itemBuilder: (context, index) => Text(items[index]),
    ),
  ],
)
```
