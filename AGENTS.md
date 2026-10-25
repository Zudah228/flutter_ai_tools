# Skillifying

Turn one decided review note into one skill at `skills/<name>/SKILL.md`. Write the file in English. The body is the examples. A comment on the first line of each code block carries the instruction.

## File

````markdown
---
name: <kebab-case-name>
description: "<What the skill fixes>. Use when <trigger: error text, widget, or situation>."
---

# <kebab-case-name>


## Examples

### <Case name>

**Input (Error State):**
```dart
// <The error this code throws, or the constraint it violates>
<widget that produces the error>
```

**Output (Resolved State):**
```dart
// <What this change does>
<widget that resolves it>
```
```dart
// <What the other valid change does>
<widget that also resolves it>
```
````

The directory name, the `name` field, and the `#` heading are the same string: lowercase letters, numbers, and hyphens. Name the skill after the fix.

## description

One quoted line, in the third person. State what the skill fixes, then `Use when` and the concrete trigger the agent should match.

## Examples

`## Examples` is the body. Each case is a `###` heading that names the situation.

**Input (Error State):** one Dart fence. The widget tree is the code that produces the error.

**Output (Resolved State):** one or more Dart fences. Each fence is a complete resolution, and each opens with a `//` comment that states that fix. When more than one fix is valid, place the fences one after another under the same Output.

Show the widget the agent should write, including the parent that supplies the constraint.
