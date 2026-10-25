---
name: static-image-provider
description: "Exposes a static ImageProvider that every constructor of a custom image widget uses, so decoration can reuse the same source. Use when a widget such as UserImage takes a URL, asset, or bytes and is also needed as a DecorationImage."
---

# static-image-provider


## Examples

### A custom image widget loads the source itself

**Input (Error State):**
```dart
// Each constructor paints on its own, so DecorationImage cannot reuse the image
class UserImage extends StatelessWidget {
  const UserImage.url(this.url) : asset = null, bytes = null;

  const UserImage.asset(this.asset) : url = null, bytes = null;

  const UserImage.memory(this.bytes) : url = null, asset = null;

  final String? url;
  final String? asset;
  final Uint8List? bytes;

  @override
  Widget build(BuildContext context) {
    if (bytes != null) {
      return Image.memory(bytes!);
    }
    if (asset != null) {
      return Image.asset(asset!);
    }
    return Image.network(url!);
  }
}
```

**Output (Resolved State):**
```dart
// url, asset, and memory constructors all paint UserImage.provider
class UserImage extends StatelessWidget {
  const UserImage.url(this.url) : asset = null, bytes = null;

  const UserImage.asset(this.asset) : url = null, bytes = null;

  const UserImage.memory(this.bytes) : url = null, asset = null;

  final String? url;
  final String? asset;
  final Uint8List? bytes;

  static ImageProvider provider({
    String? url,
    String? asset,
    Uint8List? bytes,
  }) {
    // Network, asset, memory, and a custom cache stay in this provider
    if (bytes != null) {
      return MemoryImage(bytes);
    }
    if (asset != null) {
      return AssetImage(asset);
    }
    return NetworkImage(url!);
  }

  @override
  Widget build(BuildContext context) {
    return Image(
      image: UserImage.provider(url: url, asset: asset, bytes: bytes),
    );
  }
}
```
```dart
// DecorationImage takes the same static provider the widget paints
DecorationImage(
  image: UserImage.provider(url: url),
)
```
