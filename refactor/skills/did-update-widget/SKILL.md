---
name: did-update-widget
description: "Seeds a TextEditingController from a widget field in initState and writes that field into the controller again from didUpdateWidget when it changes. Use when initState creates a TextEditingController from a widget field."
---

# did-update-widget


## Examples

### A TextEditingController seeded from a widget field

**Input (Error State):**
```dart
// initState seeds the controller from widget.title once, so a new title never updates the text
late final TextEditingController _controller;

@override
void initState() {
  super.initState();
  _controller = TextEditingController(text: widget.title.trim());
}

@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```

**Output (Resolved State):**
```dart
// didUpdateWidget writes the processed title into the same controller when that field changes
late final TextEditingController _controller;

@override
void initState() {
  super.initState();
  _controller = TextEditingController(text: widget.title.trim());
}

@override
void didUpdateWidget(covariant ArticleView oldWidget) {
  super.didUpdateWidget(oldWidget);
  if (oldWidget.title != widget.title) {
    _controller.text = widget.title.trim();
  }
}

@override
void dispose() {
  _controller.dispose();
  super.dispose();
}
```
