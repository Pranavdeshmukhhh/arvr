# AR Studio — Android AR Lab

**Developed for Pranav Deshmukh.** This is the standalone native Android edition of AR Studio. It uses Java, OpenGL ES and Google ARCore, and builds without Unity activation. The Unity 2022.3.19f1 project is preserved separately; this APK is not a Unity build.

## Install and use

1. Download and open `AR-Studio-Android-v1.0.1.apk` on an Android 7.0+ phone. Android may ask you to allow installation from the browser or Drive app.
2. Allow Camera. If prompted, install or update Google Play Services for AR. The phone must be supported by ARCore for AR features; unsupported devices retain the catalog and help screens.
3. Scan slowly in good lighting. Browse an exhibit, choose Surface or Air Mode, aim the visible target and tap Place.

Surface Mode uses tracked floor/table polygons and compatible walls. Air Mode places an anchored floating exhibit 0.35–5 metres ahead along the target ray. First move the phone sideways about 10–20 cm while viewing textured surroundings, and allow at least 1.5 seconds of uninterrupted tracking. Air Mode does not need a detected plane, but it still needs room mapping. Choosing another exhibit keeps the session and mapping active.

Placement requires a tracking native anchor; failed anchor creation cancels placement or preserves the previous position during a move. Nearby Air exhibits share an anchor within four metres, with separate local offsets, so anchor corrections preserve their relative arrangement. Exhibits whose anchors pause or stop are hidden with recovery guidance instead of being drawn at stale world coordinates. These policies reduce avoidable drift; they cannot remove ARCore's physical tracking error.

Tap an exhibit to select it; drag to move, pinch to resize and twist to rotate. The main panel offers Move, Adjust and Delete. Adjust opens rotation, size and optional animation controls while camera tracking continues. The compact main panel keeps the placement button visible on shorter screens. Dragging uses the phone's touch sensitivity threshold to reduce accidental moves. Add multiple objects (up to 30), view Info, delete one or confirm Reset. Tracking loss pauses editing and animations. Air distance changes affect future targets, not existing objects. Hiding surface outlines keeps tracking active. Objects are session-only; no saved scenes or cloud anchors are promised.

The camera projection and placement ray use the same viewport. The target moves into the space between the top and bottom panels so controls do not cover the aim point on smaller screens. Surface poses use local +Y as the outward contact axis for floors and walls.

## Build

Open this folder in Android Studio, or use JDK 17 and Android SDK 35:

```bash
./gradlew assembleDebug testDebugUnitTest lintDebug
```

The APK is `app/build/outputs/apk/debug/app-debug.apk`. The checked-in Gradle wrapper verifies Gradle 8.9's distribution checksum. The GitHub Actions workflow builds and uploads the APK without a Unity license. Publishing to the requested GitHub repository still requires account write access.

Package: `com.pranavdeshmukh.arstudio.nativeapp`; version `1.0.1-native` (code 2); minimum API 24; target API 35; ARM64, ARM32 and x86_64; Google ARCore 1.56.0. This development APK uses a debug signing certificate. A store release should use the owner's protected release keystore; builds with a different signing key require uninstalling the previous development build.

## Accuracy and verification

This is a visualization app, not a precision measurement instrument. Phone hardware, lighting, texture, distance and AR tracking affect placement accuracy. Featureless, reflective or moving surfaces may not track. Anchors can drift or stop. A compiled/signed APK does not establish physical-device accuracy.

Build/test results and any emulator observations are recorded in `VALIDATION.md`. No physical Android phone is connected to this workspace, so camera/anchor accuracy on the user's phone has not been verified.

If a problem persists, tap the tracking status text to open **Tracking report**, then Copy. The report contains the phone model, Android version, camera tracking state, mapping readiness, frame count and object-anchor states. It contains no camera image, GPS location or account credentials, and is only copied locally at the user's request.

## Credits and rights

The native application code, procedural exhibits and thumbnails were created for this task. This APK contains no ZeroGrid furniture or unverified artwork from the original MebelAR repository. The separate Unity adaptation credits [AlexDim1/MebelAR](https://github.com/AlexDim1/MebelAR), whose missing repository license remains documented in that project.

Google ARCore SDK is subject to [ARCore Additional Terms](https://developers.google.com/ar/develop/terms). Google Play Services for AR is provided by Google and governed by the [Google Privacy Policy](https://policies.google.com/privacy). The app adds no upload or analytics service and requests only Camera permission. Full bundled dependency notices appear in `app/src/main/assets/THIRD_PARTY_NOTICES.txt` and in-app Help → Credits. No blanket license for third-party material is inferred.
