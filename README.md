<div align="center">

# 🎮 VALO App

### A feature-rich VALORANT companion app built with Flutter

[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?style=for-the-badge&logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.7.2-0175C2?style=for-the-badge&logo=dart&logoColor=white)](https://dart.dev)
[![BLoC](https://img.shields.io/badge/BLoC-9.1.1-purple?style=for-the-badge)](https://bloclibrary.dev)
[![License](https://img.shields.io/badge/License-Private-red?style=for-the-badge)]()

> Browse agents, weapons, maps, ranks, sprays, player cards, and gun buddies — all in one sleek, VALORANT-themed mobile experience.

</div>

---

## 📖 Project Overview

**VALO App** is a Flutter-based mobile application that serves as a comprehensive encyclopedia and companion for the popular tactical shooter *VALORANT*. It pulls live data from the [VALORANT API](https://valorant-api.com) to present beautifully rendered information about every in-game element — from agent abilities to weapon stats and map splash arts.

The app is designed with the iconic VALORANT color palette (deep navy `#0F1923` and vivid red `#FF4657`), fully supports both **dark and light themes**, features **push notifications**, **image downloading**, **wallpaper setting**, and is optimized for both phones and tablets.

---

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| **Framework** | Flutter 3.x (Dart SDK ^3.7.2) |
| **State Management** | flutter_bloc ^9.1.1 (Cubit pattern) |
| **Dependency Injection** | get_it ^8.0.3 |
| **Networking** | dio ^5.8.0+1 |
| **Functional Programming** | dartz ^0.10.1 (Either type for error handling) |
| **Local Storage** | shared_preferences ^2.5.3 |
| **Notifications** | flutter_local_notifications ^19.2.1 |
| **Image Handling** | photo_manager 3.0.0, async_wallpaper ^2.1.0 |
| **Permissions** | permission_handler ^12.0.0+1 |
| **Responsive UI** | flutter_screenutil ^5.9.3 |
| **Theme Management** | Provider (ChangeNotifier) |
| **Fonts** | Bowlby One SC, Venite Adoremus |

---

## 🏛️ Architecture

The project follows **Clean Architecture** principles, organized in a **Feature-First** folder structure. Each feature is self-contained and communicates through well-defined layers.

```
Presentation Layer  →  BLoC (Cubit) + UI Screens + Widgets
        ↓
Domain Layer        →  Repository Interfaces (Abstract Classes)
        ↓
Data Layer          →  Repository Implementations + API Models + ApiService (Dio)
```

**Key architectural decisions:**

- **Cubit (BLoC)** is used for all async state management. Every feature has its own Cubit and State classes (e.g., `AgentsCubit`, `AgentsState`).
- **dartz `Either<Error, Data>`** is used in every repository call, making error handling explicit and functional rather than relying on exceptions.
- **GetIt** acts as the service locator and dependency injection container, registered once at app startup via `setUp()` in `di.dart`.
- **Provider** is used exclusively for the `ThemeProvider` (light/dark toggle), which is simpler and more appropriate for app-wide reactive state.
- **Named Routes** (`AppRoutes`) centralize navigation, keeping route definitions clean and separate from UI.

---

## ✨ Features

### 🧑‍🦸 Agents
- Browse all VALORANT agents in a responsive grid layout.
- Tap any agent to view a detailed profile including full portrait, role, and all four abilities with icons and descriptions.

### 🔫 Weapons
- List all weapons with images.
- Tap to view detailed stats: fire rate, fire mode, wall penetration, damage ranges per distance, and all available skins.

### 🗺️ Maps
- View all maps displayed in a 2-column grid.
- Tap any map to view the full splash art in a dialog, with a **one-tap download** button to save it to the device gallery.

### 🏆 Ranks
- Displays the complete rank ladder from Iron to Radiant, organized in clearly labeled tiers (Iron, Bronze, Silver, Gold, Platinum, Diamond, Ascendant, Immortal, Radiant).

### 🎨 Sprays & Player Cards
- Browse all in-game sprays and player card cosmetics in a scrollable grid.

### 🐶 Gun Buddies
- Browse all gun buddy cosmetics with their display images.

### ⚙️ Settings
- **Theme Toggle**: Switch between Dark Mode and Light Mode, persisted across sessions via SharedPreferences.
- **Notifications Toggle**: Enable or disable daily VALORANT digest notifications. State is persisted locally.

### 🔔 Push Notifications
- On-demand notification when toggling the notification setting.
- **Daily repeating notification** ("VALORANT Daily Digest") delivered via `RepeatInterval.daily`.

### ℹ️ About Screen
- A beautifully styled screen with a parallax-like overlay showing the app name, version, and developer credit.

---

## 🧪 Testing

The project includes the standard Flutter test setup:

```bash
# Run all tests
flutter test

# Run with coverage
flutter test --coverage
```

> **Note:** The project uses `flutter_lints ^5.0.0` for static analysis. Run `flutter analyze` to check for linting issues before submitting code.

Currently, unit tests can be added for each Cubit using the `bloc_test` package. The clean architecture makes it straightforward to mock repositories and test state transitions in isolation.

---

## 📁 Folder Structure

```
valo/
├── lib/
│   ├── main.dart                    # App entry point
│   ├── app.dart                     # Root widget, theme & routing setup
│   ├── core/
│   │   ├── network/
│   │   │   ├── api_service.dart     # Dio HTTP client
│   │   │   └── di.dart              # GetIt dependency injection setup
│   │   ├── resources/
│   │   │   ├── app_assets.dart      # Asset path constants
│   │   │   ├── app_color.dart       # Color constants
│   │   │   ├── app_provider.dart    # BlocProvider list for MultiBlocProvider
│   │   │   └── app_routes.dart      # Named route map
│   │   ├── services/
│   │   │   ├── is_tablet.dart       # Tablet detection utility
│   │   │   ├── local_notifications_services.dart
│   │   │   └── theme_provider.dart  # ChangeNotifier for dark/light mode
│   │   └── widgets/                 # Shared/reusable widgets
│   │       ├── connection_error.dart
│   │       ├── custom_app_bar.dart
│   │       ├── custom_options_data_page.dart
│   │       └── custom_row_text.dart
│   └── features/
│       ├── agents/
│       │   ├── data/
│       │   │   ├── model/           # AgentsModel, AgentDetailsModel, AbilitiesModel
│       │   │   └── repo/            # AgentsHomeRepo (abstract) & AgentsHomeRepoImpl
│       │   └── UI/
│       │       ├── manager/         # AgentsCubit, AgentDetailsCubit, States
│       │       └── screens/         # AgentsScreen, AgentDetails, Widgets
│       ├── weapons/                 # Same structure as agents
│       ├── maps/                    # Same structure + download functionality
│       ├── ranks/                   # Static ranks display
│       ├── sprays/                  # Same structure as weapons
│       ├── player cards/            # Same structure as weapons
│       ├── gun buddies/             # Same structure as weapons
│       ├── home/                    # HomeScreen with navigation options
│       ├── splash/                  # Splash screen
│       └── settings/                # SettingsScreen + AboutScreen
├── assets/
│   ├── img/                         # App images and icons
│   └── fonts/                       # BowlbyOneSC-Regular.ttf, VeniteAdoremus.ttf
├── android/
├── ios/
└── pubspec.yaml
```

---

## 🚀 How to Run the Project

### Prerequisites

Make sure you have the following installed:

- [Flutter SDK](https://flutter.dev/docs/get-started/install) (^3.7.2)
- [Android Studio](https://developer.android.com/studio) or [Xcode](https://developer.apple.com/xcode/) (for iOS)
- A physical device or emulator

### Steps

**1. Clone the repository**
```bash
git clone https://github.com/your-username/valo.git
cd valo
```

**2. Install dependencies**
```bash
flutter pub get
```

**3. Run the app**
```bash
# Debug mode
flutter run

# Release mode
flutter run --release
```

**4. Build APK (Android)**
```bash
flutter build apk --release
```

**5. Build for iOS**
```bash
flutter build ipa
```

> **Note for Android:** The app targets Android API 21+ and requires the following permissions in `AndroidManifest.xml`: `READ_MEDIA_IMAGES`, `WRITE_EXTERNAL_STORAGE`, `POST_NOTIFICATIONS`, `RECEIVE_BOOT_COMPLETED`.

---

## 🔮 Future Improvements

Here are some ideas planned for upcoming versions:

- **Search & Filter** — Add search functionality across agents, weapons, and other categories.
- **Favorites** — Allow users to bookmark their favorite agents, weapons, or skins, stored locally with SharedPreferences or Hive.
- **Patch Notes** — Integrate a patch notes section using the VALORANT blog or RSS feed.
- **Localization (i18n)** — Support multiple languages using Flutter's `intl` package (already in the dependencies!).
- **Caching** — Implement response caching with `dio_cache_interceptor` to reduce API calls and allow offline browsing.
- **Animations** — Add hero animations and page transitions to enhance the visual experience.
- **Unit & Widget Tests** — Increase test coverage with `bloc_test` and `flutter_test`.
- **iPad / Tablet Layout** — The tablet detection utility is already in place; dedicated layouts can be built for wider screens.
- **Agent Comparison** — Side-by-side comparison of two agents' abilities and stats.

---

## 📸 Screenshots

> Add your screenshots here after building the app.

| Home Screen | Agents | Agent Details |
|:-----------:|:------:|:-------------:|
| `screenshot_home.png` | `screenshot_agents.png` | `screenshot_agent_details.png` |

| Weapons | Maps | Settings |
|:-------:|:----:|:--------:|
| `screenshot_weapons.png` | `screenshot_maps.png` | `screenshot_settings.png` |

*To add screenshots: run the app, capture screenshots, and replace the placeholder filenames above with your actual image paths.*

---

## 🔗 Social Links

Made with ❤️ by **Abdelsatar | 3BS**

[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/your-username)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/your-profile)
[![Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/your-handle)

---

<div align="center">

**Data powered by [valorant-api.com](https://valorant-api.com) — an unofficial, fan-made API.**

*VALORANT is a registered trademark of Riot Games. This project is not affiliated with or endorsed by Riot Games.*

</div>
