---
name: extract-children
description: "Extracts a multi-line cluster from a bloated children list into a local variable, or a private widget when the widgets belong together, and leaves a one-line widget in the list, so the order is readable and changing it moves one line. Use when children embeds a whole screen or a group of widgets that share one job."
---

# extract-children


## Examples

### A children list embeds the screen

**Input (Error State):**
```dart
// children embeds the screen, so the order is hard to read and a reorder moves many lines
Column(
  children: [
    Text(title),
    Text(body),
    Row(
      children: [
        TextButton(onPressed: onCancel, child: const Text('Cancel')),
        TextButton(onPressed: onSave, child: const Text('Save')),
      ],
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// A one-line widget stays in the list, and the cluster is one name, so a reorder moves one line
final Widget actions = Row(
  children: [
    TextButton(onPressed: onCancel, child: const Text('Cancel')),
    TextButton(onPressed: onSave, child: const Text('Save')),
  ],
);

Column(
  children: [
    Text(title),
    Text(body),
    actions,
  ],
)
```

### A cluster belongs together

**Input (Error State):**
```dart
// The actions are one job and still inline, so they occupy many lines of the order
Column(
  children: [
    Text(title),
    Text(body),
    Row(
      children: [
        TextButton(onPressed: onCancel, child: const Text('Cancel')),
        TextButton(onPressed: onSave, child: const Text('Save')),
      ],
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// The cluster is one name in the order, so reading and reordering stay one line
Column(
  children: [
    Text(title),
    Text(body),
    _Actions(onCancel: onCancel, onSave: onSave),
  ],
)
```
```dart
// The private widget keeps the buttons and their callbacks together
class _Actions extends StatelessWidget {
  const _Actions({required this.onCancel, required this.onSave});

  final VoidCallback onCancel;
  final VoidCallback onSave;

  @override
  Widget build(BuildContext context) {
    return Row(
      children: [
        TextButton(onPressed: onCancel, child: const Text('Cancel')),
        TextButton(onPressed: onSave, child: const Text('Save')),
      ],
    );
  }
}
```
