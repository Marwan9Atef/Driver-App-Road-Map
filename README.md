
# Driver-App-Road-Map

A project for organizing work with a team.  
This repository provides a structured approach for implementing UI constraints, navigation, utility extensions, and responsive design in a Flutter application.

---

## Table of Contents

## [UI Constraints](#ui-constraints)  

 
   1 [Extension](#extension)  
      1.1 - [Context Util](#context-util)  
      1.2 - [Sizebox Util](#sizebox-util)  
2. [Navigation with Go Router](#navigation-with-go-router)  
3. [Assets](#assets)  
4. [AppColor](#appcolor)  
5. [AppStyles](#appstyles)  
6. [SizeConfig](#sizeconfig)  
7. [Explanation of Files](#explanation-of-files)  

---

## UI Constraints

### Extension

#### Context Util

**File location:** `core/utils/context_util.dart`  

**Purpose:**  
The `ContextUtil` extension adds convenient shortcuts for commonly used Flutter `BuildContext` properties. It helps reduce boilerplate code when accessing `MediaQuery`, `Theme`, padding, screen size, and keyboard info.

---

### Old Way vs New Way

**Old Way** (without `ContextUtil`):
```

import 'package:flutter/material.dart';

class HomePage extends StatelessWidget {
@override
Widget build(BuildContext context) {
final mediaQuery = MediaQuery.of(context);
final theme = Theme.of(context);

```

```
return Scaffold(
  body: Padding(
    padding: EdgeInsets.only(top: mediaQuery.padding.top),
    child: Center(
      child: Text(
        'Hello World',
        style: theme.textTheme.headlineMedium,
      ),
    ),
  ),
);
}
}

```

**New Way** (with `ContextUtil`):
```

import 'package:flutter/material.dart';
import 'core/utils/context_util.dart';

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
- **Old Way:** Must repeatedly call `MediaQuery.of(context)` and `Theme.of(context)` → verbose  
- **New Way:** Shortcuts like `context.statusBarHeight` and `context.textTheme` → cleaner, readable, less boilerplate  

---

#### Sizebox Util

**File location:** `core/utils/sizebox_util.dart`  

**Purpose:**  
Provides a convenient way to create `SizedBox` widgets from a `num` for spacing in the UI.  

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
20.hight, not SizedBox(hight:20)
Text('Below'),
],
);

Row(
children: [
Text('Left'),
15.width,
Text('Right'),
],
);

```

---

### Navigation with Go Router

**Purpose:**  
Understand what the `go` method does and how to pass extra arguments. Know the organization of Go Router files.

**File Location:** `core/routes`  
- `app_router.dart` → Main router configuration  
- `route_center.dart` → Named routes and navigation structure  

---

## Assets

**Purpose:**  
Manage images, icons, and fonts efficiently in the app.

**File Location:**  
- `core/generated/assets.dart`  
- **Automatically generated — do not edit manually.**

**Example Generated File:**
```

class Assets {
Assets._();

static const String fontsMontserratMedium = 'assets/fonts/Montserrat-Medium.ttf';
static const String fontsMontserratRegular = 'assets/fonts/Montserrat-Regular.ttf';
static const String imagesAvatar1 = 'assets/images/avatar_1.svg';
static const String imagesAvatar2 = 'assets/images/avatar_2.svg';
static const String imagesAvatar3 = 'assets/images/avatar_3.svg';
static const String imagesBalance = 'assets/images/balance.svg';
static const String imagesCardBackground = 'assets/images/card_background.png';
static const String imagesDashboard = 'assets/images/dashboard.svg';
static const String imagesExpenses = 'assets/images/expenses.svg';
static const String imagesGallery = 'assets/images/gallery.svg';
static const String imagesIncome = 'assets/images/income.svg';
static const String imagesLogout = 'assets/images/logout.svg';
static const String imagesMyInvestments = 'assets/images/my_investments.svg';
static const String imagesMyTransctions = 'assets/images/my_transctions.svg';
static const String imagesSettings = 'assets/images/settings.svg';
static const String imagesStatistics = 'assets/images/statistics.svg';
static const String imagesWalletAccount = 'assets/images/wallet_account.svg';
}

```

**Usage in Widgets:**
```

SvgPicture.asset(Assets.imagesAvatar1, width: 50, height: 50)
Container(
decoration: BoxDecoration(
image: DecorationImage(
image: AssetImage(Assets.imagesCardBackground),
fit: BoxFit.cover,
),
),
)

```

**Notes:**  
- Prefer **SVG files** for all icons/images.  
- Do not manually edit `assets.dart`.  
- Using generated references reduces errors and improves maintainability.  

---

## AppColor

**File Location:** `core/utils/app_color.dart`  

**Purpose:**  
Centralizes all app colors for consistency and maintainability.

**Code (as text):**
```

import 'dart:ui';

class AppColor {
static const Color primaryColor = Color(0xFF4EB7F2);
static const Color darkBlue = Color(0xFF064060);
static const Color lightBlue = Color(0xFF064061);
static const Color white = Color(0xFFFFFFFF);
static const Color offWhite = Color(0xFFFAFAFA);
static const Color gray = Color(0xFFAAAAAA);
}

```

**Usage:**
```

Text('Hello', style: TextStyle(color: AppColor.primaryColor))
Container(color: AppColor.darkBlue)
ElevatedButton(style: ElevatedButton.styleFrom(backgroundColor: AppColor.primaryColor))

```

---

## AppStyles

**File Location:** `core/utils/app_styles.dart`  

**Purpose:**  
Centralizes responsive text styles using `AppColor` and `SizeConfig`.

**Code (as text):**
```

abstract class AppStyles {
static TextStyle styleRegular16(context) {
return TextStyle(
color: AppColor.darkBlue,
fontSize: getResponsiveFontSize(context, fontSize: 16),
fontFamily: 'Montserrat',
fontWeight: FontWeight.w400,
);
}

static TextStyle styleBold16(BuildContext context) { ... }
static TextStyle styleMedium16(BuildContext context) { ... }
// Other styles...
}

double getResponsiveFontSize(context, {required double fontSize}) { ... }
double getScaleFactor(context) { ... }

```

**Usage:**
```

Text('Hello', style: AppStyles.styleBold16(context))
Text('Subtitle', style: AppStyles.styleRegular16(context))

```

---

## SizeConfig

**File Location:** `core/utils/size_config.dart`  

**Purpose:**  
Provides screen width/height and breakpoints for responsive layouts.

**Code (as text):**
```

class SizeConfig {
static const double desktop = 1200;
static const double tablet = 800;

static late double width, height;

static init(BuildContext context) {
height = MediaQuery.sizeOf(context).height;
width = MediaQuery.sizeOf(context).width;
}
}

```

**Usage:**
```

SizeConfig.init(context);
double screenWidth = SizeConfig.width;
double screenHeight = SizeConfig.height;
if (SizeConfig.width > SizeConfig.desktop) { ... }

```

---

## Explanation of Files

| File | Purpose |
|------|---------|
| `context_util.dart` | BuildContext shortcuts |
| `sizebox_util.dart` | SizedBox spacing shortcuts |
| `app_router.dart` | Main router configuration |
| `route_center.dart` | Named routes and navigation |
| `assets.dart` | Generated asset references |
| `app_color.dart` | Centralized colors |
| `app_styles.dart` | Centralized responsive text styles |
| `size_config.dart` | Screen size and breakpoints |




