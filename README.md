# GreetingKMP

A sample Kotlin Multiplatform project targeting Android and iOS. It shares core business logic and data layers in the `shared` module to keep consistency and maximize code reuse across platforms.

## Features

- Kotlin Multiplatform: Share code in `shared` across `commonMain`, `androidMain`, and `iosMain`.
- Android app: Implemented in the `composeApp` module using Jetpack Compose.
- iOS app: Native Swift/SwiftUI in `iosApp`, integrating shared logic.
- Gradle Kotlin DSL and centralized dependency versions via `gradle/libs.versions.toml`.

## Project Structure
├── .gitignore
├── README.md
├── build.gradle.kts
├── composeApp/
│   ├── build.gradle.kts
│   └── src/
│       ├── androidMain/
│       └── androidUnitTest/
├── gradle.properties
├── gradle/
│   ├── libs.versions.toml
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── iosApp/
│   ├── Configuration/
│   │   └── Config.xcconfig
│   ├── iosApp/
│   │   ├── Assets.xcassets/
│   │   ├── ContentView.swift
│   │   ├── Info.plist
│   │   ├── Preview Content/
│   │   └── iOSApp.swift
│   └── iosApp.xcodeproj/
│       ├── project.pbxproj
│       └── project.xcworkspace/
├── settings.gradle.kts
└── shared/
├── build.gradle.kts
└── src/
├── androidMain/
├── commonMain/
├── commonTest/
└── iosMain/

## Prerequisites

- macOS (for developing/running both Android and iOS)
- Android Studio (latest stable recommended; install Kotlin plugin and Android SDK)
- Xcode (15+ recommended), with iOS Simulator or a physical device set up
- JDK 17 (recommended), build managed via Gradle Wrapper
- Kotlin/Gradle versions as defined in `gradle.properties` and `gradle/libs.versions.toml`

## Getting Started

### 1. Clone & Initialize

- Open the project root in Android Studio/IntelliJ and wait for Gradle sync to complete.
- If using the Gradle Wrapper from terminal for the first time, make it executable:
  ```bash
  chmod +x gradlew
  ```

### 2. Run Android

- Using Android Studio:
1) Select the `composeApp` run configuration  
2) Connect a device or start an emulator  
3) Click Run

- Using terminal:
  ```bash
  ./gradlew :composeApp:installDebug
  ```
  The APK will be generated in the `composeApp` build outputs. If available, you can also use `installDebug` to install directly to a connected device.

### 3. Run iOS

- Using Xcode:
1) Open `iosApp/iosApp.xcodeproj`  
2) Select the `iosApp` target and a simulator/device  
3) Click Run

- If your iOS app integrates the KMP shared framework, it may help to perform a Gradle sync/build on the Android Studio side first, then switch back to Xcode and run again.

## Modules

- `shared/`
- `commonMain`: Shared business logic and models
- `androidMain`, `iosMain`: Platform-specific implementations (e.g., filesystem, networking, storage)
- `commonTest`: Shared tests

- `composeApp/` (Android app)
- UI built with Jetpack Compose
- Depends on `shared` for core logic

- `iosApp/` (iOS native app)
- Swift/SwiftUI UI
- Reuses core logic via the KMP output (framework/xcframework) from `shared`

## Build & Test

- Run all tests (depending on configuration, may include common, Android, and/or iOS host tests):
  ```bash
  ./gradlew test
  ```

- Clean build cache:
  ```bash
  ./gradlew clean
  ```

- Android release build (example; signing and variants depend on your setup):
  ```bash
  ./gradlew :composeApp:assembleRelease
  ```
  
- iOS archiving/release: Use Xcode’s Archive flow for signing and distribution.

## Dependency Management

- Centralized versions in `gradle/libs.versions.toml`
- Module-specific dependencies declared in each module’s `build.gradle.kts`
- To upgrade dependencies:
1) Update versions in `libs.versions.toml`  
2) Sync/build  
3) Run tests to ensure compatibility

## Troubleshooting

- Xcode can’t find the shared framework or build fails:
- Go back to Android Studio, run a Gradle sync/build in the KMP project
- Clean Xcode Derived Data and retry

- Android build failures:
- Ensure matching Android SDK/build tools are installed
- Check available tasks via `./gradlew tasks` or use the Gradle tool window in the IDE

- Version incompatibilities:
- Align versions to `gradle/libs.versions.toml` and `gradle.properties`, and use a known compatible set when necessary

## Roadmap (Optional)

- Add CI (GitHub Actions/GitLab CI, etc.)
- Integrate static analysis (Detekt/Ktlint) and formatting tasks
- Introduce DI, networking, and database (e.g., Ktor/SQLDelight)

## License

Choose one as appropriate (e.g., Apache-2.0 / MIT). Currently unspecified.