---
name: scroll-cache-extent
description: "Lays out image items before and after the viewport by setting scrollCacheExtent to half the screen height. Use when a ListView of slow images builds each item only as it scrolls into view. A larger cache builds more off-screen children, so weigh the image size against that extra work."
---

# scroll-cache-extent


## Examples

### Images start loading only as they scroll in

**Input (Error State):**
```dart
// The default cache is 250px, so each image starts loading only as it reaches the viewport
ListView.builder(
  itemCount: urls.length,
  itemBuilder: (context, index) {
    return Image.network(urls[index]);
  },
)
```

**Output (Resolved State):**
```dart
// Half a screen is laid out before and after the viewport, so those images start loading before they appear
ListView.builder(
  scrollCacheExtent: ScrollCacheExtent.pixels(
    MediaQuery.sizeOf(context).height / 2,
  ),
  itemCount: urls.length,
  itemBuilder: (context, index) {
    return Image.network(urls[index]);
  },
)
```
