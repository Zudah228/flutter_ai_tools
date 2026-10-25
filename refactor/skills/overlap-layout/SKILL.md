---
name: overlap-layout
description: "Lays out an overlap with CustomMultiChildLayout when the child fills constraints.biggest, and with a RenderStack subclass when the size follows the children. Use when a Stack places a child from a GlobalKey size or a fixed width and height."
---

# overlap-layout


## Examples

### The overlap is measured with a GlobalKey or a fixed size

**Input (Error State):**
```dart
// A GlobalKey size is missing on the first frame, and a fixed size ignores text scale and wrapping
final GlobalKey backKey = GlobalKey();

Stack(
  children: [
    Back(key: backKey),
    Positioned(
      left: backKey.currentContext?.size?.width ?? 120,
      child: const Badge(),
    ),
  ],
)
```

**Output (Resolved State):**
```dart
// The child fills constraints.biggest, so CustomMultiChildLayout decides size and position together
CustomMultiChildLayout(
  delegate: const _FillDelegate(),
  children: const [
    LayoutId(id: 'back', child: Back()),
    LayoutId(id: 'badge', child: Badge()),
  ],
)
```
```dart
// layoutChild uses the incoming size, which is constraints.biggest
class _FillDelegate extends MultiChildLayoutDelegate {
  const _FillDelegate();

  @override
  void performLayout(Size size) {
    layoutChild('back', BoxConstraints.tight(size));
    positionChild('back', Offset.zero);
    final Size badgeSize = layoutChild('badge', BoxConstraints.loose(size));
    positionChild('badge', Offset(size.width - badgeSize.width, 0));
  }

  @override
  bool shouldRelayout(covariant _FillDelegate oldDelegate) => false;
}
```
```dart
// The stack size follows the children, so the widget creates a RenderStack
class _ChildSizedStack extends MultiChildRenderObjectWidget {
  const _ChildSizedStack({super.children});

  @override
  RenderObject createRenderObject(BuildContext context) {
    return _ChildSizedRenderStack(
      alignment: alignment,
      textDirection: Directionality.maybeOf(context),
      fit: fit,
      clipBehavior: clipBehavior,
    );
  }
}

class _ChildSizedRenderStackParentData extends ContainerBoxParentData<RenderBox> {
  _ChildSizedRenderStackParentData();
}

// performLayout reads each child's size and positions the badge from it
class _ChildSizedRenderStack extends RenderStack
   with
        ContainerRenderObjectMixin<RenderBox, _ChildSizedRenderStackParentData>,
        RenderBoxContainerDefaultsMixin<
          RenderBox,
          _ChildSizedRenderStackParentData
        >  {
  _ChildSizedRenderStack({
    required super.alignment,
    required super.textDirection,
    required super.fit,
    required super.clipBehavior,
  });

  @override
  void performLayout() {
    final RenderBox back = firstChild!;
    final RenderBox badge = childAfter(back)!;
    final BoxConstraints childConstraints = BoxConstraints.loose(
      constraints.biggest,
    );
    back.layout(childConstraints, parentUsesSize: true);
    badge.layout(childConstraints, parentUsesSize: true);
    size = constraints.constrain(back.size);
    (back.parentData! as StackParentData).offset = Offset.zero;
    (badge.parentData! as StackParentData).offset = Offset(
      back.size.width - badge.size.width,
      0,
    );
  }

  @override
  void setupParentData(RenderBox child) {
    if (child.parentData is! _ChildSizedRenderStackParentData) {
      child.parentData = _ChildSizedRenderStackParentData();
    }
  }
}
```
