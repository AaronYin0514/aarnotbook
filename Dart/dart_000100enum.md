---
tags: dart enum
---

# Enum

## 定义枚举

```dart
enum Status { none, running, stopped, paused }

Status.values.forEach((it) => print('$it - index: ${it.index}'));
```
