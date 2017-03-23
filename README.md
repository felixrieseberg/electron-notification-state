# electron-notification-state
Should you display a notification to your user? This module allows you to check.

## Usage

### Do Not Disturb / Quiet Hours
```js
const { getDoNotDisturb } from 'electron-notification-state'
// On Windows, logs `true` if "quiet hours" are enabled
// On macOS, logs `true` if "do not disturb" is enabled
console.log(getDoNotDisturb());
```

### Session State
On Windows, you likely want to check [SHQueryUserNotificationState](https://msdn.microsoft.com/en-us/library/windows/desktop/bb762242(v=vs.85).aspx), checking whether or not a screensaver is running, if the screen is locked, or if a presentation is running. On macOS, you likely want to check if the screen is locked and if the if the user has an active screen session.

The call below logs a different string, depending on the platform:
#### macOS
- `SESSION_SCREEN_IS_LOCKED` The screen is locked.
- `SESSION_ON_CONSOLE_KEY` The user currently has console/graphical control.

#### Windows
- `QUNS_NOT_PRESENT` A screen saver is displayed, the machine is locked, or a nonactive Fast User Switching session is in progress.
- `QUNS_BUSY` A full-screen application is running or Presentation Settings are applied. Presentation Settings allow a user to put their machine into a state fit for an uninterrupted presentation, such as a set of PowerPoint slides, with a single click.
- `QUNS_RUNNING_D3D_FULL_SCREEN` A full-screen (exclusive mode) Direct3D application is running.
- `QUNS_PRESENTATION_MODE` The user has activated Windows presentation settings to block notifications and pop-up messages.
- `QUNS_ACCEPTS_NOTIFICATIONS` None of the other states are found, notifications can be freely sent.
- `QUNS_QUIET_TIME` Introduced in Windows 7. The current user is in "quiet time", which is the first hour after a new user logs into his or her account for the first time. During this time, most notifications should not be sent or shown. This lets a user become accustomed to a new computer system without those distractions. Quiet time also occurs for each user after an operating system upgrade or clean installation, according to Windows. This is _not_ the same as "quiet hours".
Note that during quiet time, if the user is in one of the other blocked modes (QUNS_NOT_PRESENT, QUNS_BUSY, QUNS_PRESENTATION_MODE, or QUNS_RUNNING_D3D_FULL_SCREEN) SHQueryUserNotificationState returns only that value, and does not report QUNS_QUIET_TIME.
- `QUNS_APP` Introduced in Windows 8. A Windows Store app is running.

```js
const { getSessionState } from 'electron-notification-state'

console.log(getSessionState());
```

## License
MIT. Please see LICENSE for details.