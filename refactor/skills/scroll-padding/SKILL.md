---
name: scroll-padding
description: "Passes system insets and design padding to ListView, GridView, SingleChildScrollView, and CustomScrollView. Use when setting scroll padding."
---

# scroll-padding


## Examples

### Design padding on a ListView

**Input (Error State):**
```dart
// padding replaces the automatic system insets, so EdgeInsets.all(16) drops them. GridView does the same
ListView(
  padding: const EdgeInsets.all(16),
  children: const [
    Text('Item'),
  ],
)
```

**Output (Resolved State):**
```dart
// Add the design padding on top of every system edge the automatic padding would have supplied
return ListView(
  padding: const EdgeInsets.all(16) + MediaQuery.paddingOf(context),
  children: const [
    Text('Item'),
  ],
);
```

### System insets only, or flush to the edge

**Input (Error State):**
```dart
// A vertical list already applies the top and bottom of MediaQueryData.padding. Passing MediaQuery.paddingOf also adds the left and right
ListView(
  padding: MediaQuery.paddingOf(context),
  children: const [
    Text('Item'),
  ],
)
```

**Output (Resolved State):**
```dart
// Leave padding unset when only those scroll-axis insets are wanted. ListView and GridView apply them on their own
return ListView(
  children: const [
    Text('Item'),
  ],
);
```
```dart
// EdgeInsets.zero drops the automatic insets and keeps the list flush with the edge
return ListView(
  padding: const EdgeInsets.zero,
  children: const [
    Text('Item'),
  ],
);
```

### Horizontal insets on a vertical list

**Input (Error State):**
```dart
// A vertical list's automatic padding is only the top and bottom, so a landscape notch covers the items
ListView(
  children: const [
    Text('Item'),
  ],
)
```

**Output (Resolved State):**
```dart
// Any padding replaces the automatic top and bottom, so keep those edges and add the left and right notch
final insets = MediaQuery.paddingOf(context);
return ListView(
  padding: EdgeInsets.only(top: insets.top, bottom: insets.bottom) +
      EdgeInsets.only(left: insets.left, right: insets.right),
  children: const [
    Text('Item'),
  ],
);
```

### SingleChildScrollView

**Input (Error State):**
```dart
// SingleChildScrollView does not apply system insets on its own
SingleChildScrollView(
  child: const Text('Body'),
)
```

**Output (Resolved State):**
```dart
// Include the system insets in padding
return SingleChildScrollView(
  padding: MediaQuery.paddingOf(context),
  child: const Text('Body'),
);
```

### CustomScrollView

**Input (Error State):**
```dart
// A SliverToBoxAdapter sized to the top and bottom padding leaves the left and right uncovered
CustomScrollView(
  slivers: [
    SliverToBoxAdapter(
      child: SizedBox(height: MediaQuery.paddingOf(context).top),
    ),
    SliverList(
      delegate: SliverChildListDelegate(const [
        Text('Item'),
      ]),
    ),
    SliverToBoxAdapter(
      child: SizedBox(height: MediaQuery.paddingOf(context).bottom),
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// SliverSafeArea insets every edge
CustomScrollView(
  slivers: [
    SliverSafeArea(
      sliver: SliverList(
        delegate: SliverChildListDelegate(const [
          Text('Item'),
        ]),
      ),
    ),
  ],
)
```
```dart
// SliverPadding with the system insets is the other way to inset every edge
CustomScrollView(
  slivers: [
    SliverPadding(
      padding: MediaQuery.paddingOf(context),
      sliver: SliverList(
        delegate: SliverChildListDelegate(const [
          Text('Item'),
        ]),
      ),
    ),
  ],
)
```
