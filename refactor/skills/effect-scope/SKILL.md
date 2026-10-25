---
name: effect-scope
description: "Moves a page-open side effect out of the page State into a mixin or a Scope widget that returns its child. Use when a page State checks for a force update or shows a dialog when the page opens."
---

# effect-scope


## Examples

### A side effect written in the page State

**Input (Error State):**
```dart
// The force-update check is written directly in the page State
class _HomePageState extends State<HomePage> {
  @override
  void initState() {
    super.initState();
    _checkForceUpdate();
  }

  Future<void> _checkForceUpdate() async {}

  @override
  Widget build(BuildContext context) {
    return const Text('Settings');
  }
}
```

**Output (Resolved State):**
```dart
// The check lives in a mixin, and the page State applies it with with
class _HomePageState extends State<HomePage> with ForceUpdateMixin {
  @override
  Widget build(BuildContext context) {
    return const Text('Settings');
  }
}
```
```dart
mixin ForceUpdateMixin<T extends StatefulWidget> on State<T> {
  @override
  void initState() {
    super.initState();
    _checkForceUpdate();
  }

  Future<void> _checkForceUpdate() async {}
}
```
```dart
// ForceUpdateScope takes a child, returns that child, and runs the check. The name follows PopScope
ForceUpdateScope(
  child: const HomePage(),
)
```
```dart
class ForceUpdateScope extends StatefulWidget {
  const ForceUpdateScope({super.key, required this.child});

  final Widget child;

  @override
  State<ForceUpdateScope> createState() => _ForceUpdateScopeState();
}

class _ForceUpdateScopeState extends State<ForceUpdateScope> {
  @override
  void initState() {
    super.initState();
    _checkForceUpdate();
  }

  Future<void> _checkForceUpdate() async {}

  @override
  Widget build(BuildContext context) => widget.child;
}
```
