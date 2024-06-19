---
tags: dart
---

# Symbol

Dart语言的标识符，在反射中用的很普及，特别是很多发布包都是混淆后的。

```dart
import 'dart:mirrors';  
  
Symbol libraryName = new Symbol('dart.core');  
MirrorSystem mirrorSystem = currentMirrorSystem();  
LibraryMirror libMirror = mirrorSystem.findLibrary(libraryName);  
libMirror.declarations.forEach((s, d) => print('$s - $d'));
```



