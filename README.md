# Driver-App-Road-Map

A project for organizing work with a team.  
This repository provides a structured approach for implementing UI constraints, navigation, and utility extensions in a Flutter application.

---

## Table of Contents

1. [UI Constraints](#ui-constraints)  
   1.1 [Extension](#extension)  
       - [Context Util](#context-util)  
       - [Sizebox Util](#sizebox-util)  
   1.2 [Navigation with Go Router](#navigation-with-go-router)  
2. [Explanation of Files](#explanation-of-files)  

---

## UI Constraints

### Extension

#### Context Util

**File location:** `core/utils/context_util.dart`  

**Purpose:**  
The `ContextUtil` extension adds convenient shortcuts for commonly used Flutter `BuildContext` properties. It helps reduce boilerplate code when accessing `MediaQuery`, `Theme`, padding, screen size, and keyboard info.

**Code (as text):**
```

import 'package:flutter/material.dart';

extension ContextUtil on BuildContext {
MediaQueryData get mediaQuery => MediaQuery.of(this);
ThemeData get theme => Theme.of(this);
TextTheme get textTheme => theme.textTheme;
bool get isDarkMode => theme.brightness == Brightness.dark;
Size get screenSize => mediaQuery.size;
double get screenWidth => screenSize.width;
double get screenHeight => screenSize.height;
EdgeInsets get padding => mediaQuery.padding;
EdgeInsets get viewInsets => mediaQuery.viewInsets;
double get statusBarHeight => padding.top;
double get bottomBarHeight => padding.bottom;
double get keyboardHeight => viewInsets.bottom;
}

```

**Minimal Usage Example (as text):**
```

class HomePage extends StatelessWidget {
@override
Widget build(BuildContext context) {
return Scaffold(
body: Padding(
padding: EdgeInsets.only(top: context.statusBarHeight),
child: Center(
child: Text(
'Hello World',
style: context.textTheme.headlineMedium,
),
),
),
);
}
}

```

**Explanation:**  
- `context.screenWidth` / `context.screenHeight` → screen dimensions  
- `context.padding` → safe area padding  
- `context.theme` / `context.textTheme` → access theme  
- `context.isDarkMode` → check dark mode  
- `context.keyboardHeight` → detect keyboard height  

**Notes:**  
- Replaces repetitive calls like `MediaQuery.of(context).size` or `Theme.of(context)`  
- Everything is contained in this single README.

---

#### Sizebox Util

**File location:** `core/utils/sizebox_util.dart`  

**Purpose:**  
The `SizeboxUtil` extension provides a convenient way to create `SizedBox` widgets directly from a `num` (int/double) for spacing in the UI. Reduces boilerplate and improves readability.

**Code (as text):**
```

import 'package:flutter/cupertino.dart';

extension SizeboxUtil on num {
SizedBox get width => SizedBox(width: toDouble());
SizedBox get hight => SizedBox(height: toDouble());
}

```

**Minimal Usage Example (as text):**
```

Column(
children: [
Text('Above'),
20.hight,  // 20 pixels vertical spacing
Text('Below'),
],
);

Row(
children: [
Text('Left'),
15.width,  // 15 pixels horizontal spacing
Text('Right'),
],
);

```

**Explanation:**  
- `20.hight` → `SizedBox(height: 20)`  
- `15.width` → `SizedBox(width: 15)`  
- Replaces repetitive `SizedBox` calls with cleaner syntax  

---

### Navigation with Go Router

**Purpose:**  
Understand what the `go` method does and how to pass extra arguments when navigating. Also, know the organization of Go Router files.

**File Location:** `core/routes`  
- `app_router.dart` → Main router configuration using Go Router  
- `route_center.dart` → Defines named routes and navigation structure

**Notes:**  
- Before implementing UI and navigation, developers should understand **Go Router methods** and **file organization**.

---

## Explanation of Files

| File | Purpose |
|------|---------|
| `context_util.dart` | Provides `ContextUtil` extension for BuildContext shortcuts |
| `sizebox_util.dart` | Provides `SizeboxUtil` extension for spacing |
| `app_router.dart` | Main router configuration using Go Router |
| `route_center.dart` | Defines named routes and navigation structure |




