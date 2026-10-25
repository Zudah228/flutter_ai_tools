---
name: primary-scroll-controller
description: "Sets primary only on the visible scroll view so an iOS status-bar tap scrolls that view to the top. Use when an IndexedStack lays out more than one scroll view, such as a ListView or CustomScrollView, or when \"ScrollController attached to multiple scroll views.\" is thrown."
---

# primary-scroll-controller


## Examples

### Several lists in an IndexedStack

**Input (Error State):**
```dart
// IndexedStack lays out every vertical list, and each one inherits the PrimaryScrollController: "ScrollController attached to multiple scroll views." A status-bar tap cannot scroll the visible list to the top
class _GalleryState extends State<Gallery> {
  var currentIndex = 0;

  @override
  Widget build(BuildContext context) {
    return IndexedStack(
      index: currentIndex,
      children: [
        for (final page in widget.pages)
          ListView(
            children: page,
          ),
      ],
    );
  }
}
```

**Output (Resolved State):**
```dart
// primary is true only for the visible page, so the status-bar tap scrolls that list to the top
class _GalleryState extends State<Gallery> {
  var currentIndex = 0;

  @override
  Widget build(BuildContext context) {
    return IndexedStack(
      index: currentIndex,
      children: [
        for (var index = 0; index < widget.pages.length; index++)
          ListView(
            primary: index == currentIndex,
            children: widget.pages[index],
          ),
      ],
    );
  }
}
```

### Several custom scroll views in an IndexedStack

**Input (Error State):**
```dart
// IndexedStack lays out every vertical CustomScrollView, and each one inherits the PrimaryScrollController: "ScrollController attached to multiple scroll views."
class _GalleryState extends State<Gallery> {
  var currentIndex = 0;

  @override
  Widget build(BuildContext context) {
    return IndexedStack(
      index: currentIndex,
      children: [
        for (final page in widget.pages)
          CustomScrollView(
            slivers: [
              SliverList.list(children: page),
            ],
          ),
      ],
    );
  }
}
```

**Output (Resolved State):**
```dart
// primary is true only for the visible page, so the status-bar tap scrolls that view to the top
class _GalleryState extends State<Gallery> {
  var currentIndex = 0;

  @override
  Widget build(BuildContext context) {
    return IndexedStack(
      index: currentIndex,
      children: [
        for (var index = 0; index < widget.pages.length; index++)
          CustomScrollView(
            primary: index == currentIndex,
            slivers: [
              SliverList.list(children: widget.pages[index]),
            ],
          ),
      ],
    );
  }
}
```
