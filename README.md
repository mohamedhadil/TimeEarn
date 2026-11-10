# TimeEarn

TimeEarn is an Android app that locks your distracting apps until you complete your tasks. Finish a task, earn minutes, then spend them on the apps you chose to limit.

## Features

- **Tasks and rewards** – Create daily or scheduled tasks with a time value. Complete a task to earn screen time.
- **Task timers** – Run a timer per task, with Pause / Resume / Claim buttons right in the notification.
- **Spend your time** – Choose how to use the time you earned (Instagram, Movies, YouTube, Games, or your own apps).
- **App blocking** – Blocked apps are covered by a full-screen lock screen when you have no screen time.
- **Focus mode** – While a task timer is running, all blocked apps stay blocked, even if you have screen time available.
- **Block schedules** – Choose when blocking is active each day.
- **Daily app limits** – Set a maximum number of minutes per app per day.
- **Plan-tomorrow lock** – Apps stay locked after a chosen time until you plan tomorrow.
- **Pause blocking** – Temporarily pause blocking for 5, 15 or 30 minutes.
- **Reminders and alerts** – Notifications for tasks, end-of-day summary, and 5-minute / 1-minute warnings before screen time ends.
- **Stats and history** – Track your progress, log and statistics.
- **Backup and restore** – Save and restore your data as a file.

## How blocking works

TimeEarn detects the foreground app using **Usage Access** and the **Accessibility Service**, and covers blocked apps with an overlay. The Accessibility Service only reads the package name of the app in front. It does not read screen content, text, taps or key presses, and nothing is stored or sent anywhere.

## Permissions

| Permission | Why it is needed |
|---|---|
| Usage Access | Detect which app is in the foreground and measure daily app usage |
| Accessibility Service | Instantly detect app switches so blocking reacts immediately |
| Display over other apps | Show the lock screen over blocked apps |
| Notifications | Timers, reminders and screen-time alerts |
| Foreground service | Keep the blocker running in the background |
| Run at startup | Restart blocking after the phone reboots |

All data stays on your device.

## Tech

- Android (Kotlin), `minSdk 26`, `targetSdk 34`
- UI is a local web app (`app/src/main/assets/index.html`) shown in a WebView, connected to native code through a JavaScript bridge
- Native services: `BlockerService` (overlay and blocking logic), `AppWatchService` (accessibility), notification and reminder receivers
- Package: `com.earnscreentime`

## Project structure

```
app/src/main/
├── assets/index.html            # Web UI and app logic
└── java/com/earnscreentime/
    ├── MainActivity.kt          # WebView host and JS bridge
    ├── BlockerService.kt        # Foreground-app detection and lock overlay
    ├── AppWatchService.kt       # Accessibility service (package names only)
    ├── Store.kt                 # Blocking rules and state
    ├── Perms.kt                 # Permissions and notifications
    ├── NotifActions.kt          # Pause / Resume / Claim notification buttons
    ├── Reminders.kt             # Reminders and end-of-day summary
    ├── Usage.kt                 # Usage statistics
    ├── Backup.kt                # Backup and restore
    ├── PickerActivity.kt        # App picker
    └── BootReceiver.kt          # Restart after reboot
```

## Build

Requirements: JDK 17 and Gradle 8.7.

```bash
gradle assembleDebug
```

The APK is created at `app/build/outputs/apk/debug/app-debug.apk`.

### Automatic builds (GitHub Actions)

Pushing to `main` runs the **Build APK** workflow, which builds the project and publishes `app-debug.apk` as a GitHub Release.

## Install and set up

1. Download `app-debug.apk` from the latest Release and install it.
2. Open TimeEarn and grant the requested permissions (Usage Access, Accessibility, Display over other apps, Notifications).
3. Pick the apps you want to block.
4. Add tasks, complete them, and spend the time you earned.

## Privacy

TimeEarn works fully offline. It has no accounts, no analytics and no network access. Your tasks, balance and history are stored locally on your device.
