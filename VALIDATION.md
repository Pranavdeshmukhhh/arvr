# Native Android APK verification

Version `1.0.1-native` (code 2), built on 16 October 2026 using JDK 17, Android Gradle Plugin 8.7.3, Gradle 8.9, Android SDK 35 and Google ARCore 1.56.0.

- `assembleDebug`: passed, actual APK generated.
- `testDebugUnitTest`: 26 tests passed, zero failures/errors. Includes the original placement math tests, camera-frame freshness, Air mapping readiness/recovery, shared-anchor ownership/offsets, failed placement/move rollback and Android view measurement on a 360 × 640 dp screen.
- The selected-object phone layout test initially reproduced a clipped placement button (35 of its 50 pixels visible). It passes after moving size/rotation/animation into Adjust and compacting the main panel. Placement and selected-object layouts both pass.
- Mocked ARCore tests confirm failed anchor creation commits no unanchored object, a failed move preserves its previous anchor/pose, a paused new anchor is rejected, nearby Air objects share an anchor, deleting one preserves the others, and catalog selection preserves session/readiness. These are application-policy tests, not a simulated physical AR session.
- `lintDebug`: passed with zero errors. Remaining warnings concern the intentional portrait phone interface, English UI strings, backup metadata and world-view accessibility. All manipulations also have native buttons/sliders.
- `apksigner verify --verbose --print-certs`: passed; APK Signature Scheme v2. The certificate matches version 1.0.0, permitting an in-place development update. This is not a store-release keystore.
- `zipalign -c -P 16 4`: passed, including native-library ZIP alignment.
- Package metadata: `com.pranavdeshmukh.arstudio.nativeapp`, version `1.0.1-native`, minimum API 24, target API 35; ARM64, ARM32 and x86_64.
- Requested application permission: Camera only. The app itself has no Internet permission and adds no image upload or analytics service. Google Play Services for AR runs under Google's terms/privacy policy.
- Emulator launch testing: blocked. This environment has no KVM acceleration; the software-emulated Android 35 system did not finish initializing. Installation attempts failed in the emulator's package/storage services before the application could launch. This is not a passed installation or AR session test.
- The user reported idle object drift/jumps and scanning confusion after catalog selection on a Samsung Galaxy S24 FE with version 1.0.0. The revision addresses early Air placement, unanchored fallback, independent nearby Air anchors, stale-anchor rendering, stale camera frames, overly sensitive dragging and compact-screen layout. The physical drift report has not been reproduced here or verified fixed on that phone.
- No physical phone is attached. Real camera/anchor accuracy, gestures, tracking recovery and UI fit on the user's S24 FE remain unverified. Robolectric measures native views and uses mocked ARCore for policy checks; it does not test the camera, native GPU rendering or real anchors.

Machine-readable unit test, lint, signature and package reports are in `verification/`. They substantiate build/package checks, not physical-device accuracy or Google Play acceptance.

## Remaining phone checks

1. Install version 1.0.1 over the previous APK; confirm Camera is allowed and Google Play Services for AR is current.
2. In a bright, textured room, move the phone sideways 10–20 cm. Air placement should wait for continuous tracking and translation, then enable Place without requiring a detected plane.
3. Browse and select multiple exhibits; the camera should continue updating. Place two nearby Air objects, hold still, then walk around them. Confirm their positions in the room and relative arrangement; animation starts only when requested.
4. Test Surface placement, object selection, deliberate drag, Adjust controls, Delete and Reset. Background/restore the app and temporarily interrupt tracking. Recovering objects should hide rather than float at stale positions.
5. If freezing/drift persists, tap the tracking status and Copy the report. Compare camera-frame age, mapping readiness and object anchor states while the failure occurs.
