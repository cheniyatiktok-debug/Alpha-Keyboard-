# Alpha Keyboard

Alpha Keyboard is a native Android Input Method Editor (IME), not a website or a keyboard drawn inside another app. It provides direct English and Sinhala script key layouts, local color themes, and an optional personal-photo background.

## Privacy model

- The Android manifest requests no `INTERNET` permission.
- The keyboard does not collect, retain, transmit, or log typed text.
- Password fields do not show the personal-photo background.
- Theme choice and the selected photo URI are stored locally in app preferences. The photo itself is not copied by Alpha Keyboard.
- The system photo picker/document picker grants access to the image selected by the user.
- Avoid adding analytics, crash payloads, clipboard capture, autocomplete history, or network calls to the keyboard service.

Android displays a standard system warning when a third-party keyboard is enabled. This is expected for any IME.

## Android development requirements

1. Install Android Studio and its bundled JDK 17. The build uses Android Gradle Plugin 8.7.3, which requires Gradle 8.9 or later.
2. In Android Studio's SDK Manager, install Android SDK Platform 35 and a compatible Android build-tools version.
3. Install Gradle 8.9 or later. From the `alpha-keyboard` directory, run `gradle wrapper --gradle-version 8.9` to generate the Gradle wrapper files (they are not included in this source scaffold).
4. Open the `alpha-keyboard` directory as an Android Studio project and allow Gradle to sync.
5. Run on an Android emulator or a physical Android device. For physical-device testing, enable Developer options and USB debugging.
6. Build a debug APK with Android Studio's **Build > Build Bundle(s) / APK(s) > Build APK(s)** action. From a terminal, `./gradlew :app:assembleDebug` also builds it.
7. Install the APK, launch Alpha Keyboard, tap **Enable keyboard**, enable it in Android settings, then tap **Choose keyboard** and select Alpha Keyboard.
8. Test both languages in ordinary text fields and verify password fields, app switching, photo access, and keyboard dismissal on multiple Android versions before release.

The project targets SDK 35, uses minimum Android API 26, and requires Java 17 for builds. Replit's standard preview is not an Android emulator; final IME behavior must be installed and checked on an Android emulator/device. This workspace currently has no Android SDK, Java runtime, or Gradle installation, so it cannot produce or run an APK here.

## Code walkthrough

1. `app/build.gradle.kts` sets the Android application ID, SDK levels, and Java/Kotlin 17 toolchain.
2. `app/src/main/AndroidManifest.xml` registers the launcher activity and the protected `InputMethodService`. The service requires Android's `BIND_INPUT_METHOD` permission and has no internet permission.
3. `app/src/main/res/xml/method.xml` declares English and Sinhala as selectable Android keyboard subtypes.
4. `KeyboardPreferences.kt` stores only the theme, accent color, and user-selected image URI in local app preferences.
5. `AlphaKeyboardService.kt` renders the key layout and commits the selected key directly to the active text field. Its input handling does not log or persist typed content; password fields omit the photo background.
6. `MainActivity.kt` provides setup links to Android's keyboard settings, theme and accent controls, photo selection/removal, and the privacy explanation.
7. Build and install the project using the Android Studio steps above, then enable and select the IME on a device to test it.

## Sinhala layout scope

The Sinhala layout enters Unicode Sinhala characters directly; it does not transliterate Latin phonetics into Sinhala, predict words, or provide an alternate legacy encoding. A production release should validate key placement with Sinhala typists and test grapheme deletion and cursor behavior on supported Android releases.

## Google AdMob integration plan

AdMob is intentionally not included in the keyboard service. If ads are added in a later release:

1. Put ads only in the companion settings/onboarding activity, never on the typing surface or in a way that can be mistaken for a key.
2. Create the AdMob app and ad unit in the user's Google AdMob account, then configure the Android app ID and test ad units through the official Google Mobile Ads SDK.
3. Add the required `INTERNET` permission only when the SDK is introduced, and review its data collection and consent requirements before release.
4. Complete Google Play Data safety disclosures and any required consent flow. Do not send typed text, password state, or keyboard content to advertising or analytics services.
5. Keep development on test ad units; use production ad units only after the app and disclosures are approved.

## Project structure

- `app/src/main/java/com/alpha/keyboard/AlphaKeyboardService.kt` — native Android keyboard service and key handling.
- `app/src/main/java/com/alpha/keyboard/MainActivity.kt` — setup, theme selection, photo picker, and privacy information.
- `app/src/main/java/com/alpha/keyboard/KeyboardPreferences.kt` — local theme and photo-reference preferences.
- `app/src/main/res/xml/method.xml` — English and Sinhala IME subtype declarations.
