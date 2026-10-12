# Android Screen Link Companion

This is the Android companion app for Android Screen Link on macOS. It lets the Mac connect to the phone over local Wi-Fi without ADB.

## What it does

- Shares the Android screen using Android's MediaProjection permission.
- Serves the latest screen frame to the Mac on local HTTP port `49152`.
- Limits the stream to 30 fps and a 960-pixel longest edge to prioritize a stable local connection.
- Reuses local HTTP connections between frame requests to avoid repeated connection setup.
- Accepts paired local commands for tap, swipe, Back, Home, Recents, lock screen, and text entry.
- Uses an optional Accessibility Service for interaction. Without it, wireless mirroring still works.
- Shows a six-digit pairing code in the phone app. The Mac must send the same code.

## Build

Open this folder in Android Studio, let Gradle sync, and run the `app` configuration on your Android phone.

You can also build from the command line with:

```sh
./gradlew assembleDebug
```

The project uses Android Gradle Plugin `8.7.3`, Java source, `compileSdk 35`, and `minSdk 26`.

## Install

Use the generated debug APK for quick local testing. If ADB is installed:

```sh
adb install AndroidScreenLinkCompanion-debug.apk
```

## Use

1. Open Android Screen Link Companion on the phone.
2. Tap Start Screen Share and accept Android's screen capture prompt.
3. To control the phone from the Mac, tap Enable Touch Control and turn on Android Screen Link Companion in Accessibility settings.
4. Copy the shown `http://IP:49152` address and six-digit pairing code into the macOS app's Companion mode.
5. Press Connect on the Mac, then Mirror.

Keep this on a trusted Wi-Fi network. The pairing code prevents casual local access, but traffic is not encrypted.

## Sleep behavior

While screen sharing is active, the foreground service holds CPU and Wi-Fi locks so the local stream can continue after the phone display turns off. This uses extra battery; tap Stop Server when you are done.

Turn on **Keep phone awake while sharing** in the companion app to keep the display on and prevent its normal lock timeout while sharing. This setting is saved for the next session and is released as soon as sharing stops.

Android still controls lock-screen privacy. The switch does not override the physical power/lock button or managed-device policy. Some phones may blank or revoke a capture when the device is explicitly locked, and a system-killed MediaProjection cannot be restored without starting screen share again in the companion app.
