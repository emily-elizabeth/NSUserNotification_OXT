# NSUserNotification (Legacy)

A LiveCode Builder extension for posting local notifications on macOS 10.8–10.13 using the legacy `NSUserNotificationCenter` API.

> **⚠️ Legacy Notice**
> `NSUserNotificationCenter` was deprecated in macOS 10.14 and is non-functional on macOS 12 and later. For macOS 10.14 and above, use the [UNUserNotification](https://github.com/emily-elizabeth/UserNotification_OXT) extension instead. This library is provided for apps that still need to support older macOS versions.

---

## Requirements

- macOS 10.8 through 10.13
- OpenXTalk 1.14/LiveCode 9.6 or later

---

## Installation

Copy `NSUserNotification.lcb` into your project and compile it using the LiveCode Extension Builder, or install the pre-built `.lce` file via the Extension Manager.

---

## Usage

No authorization request is needed. Simply call `PostUserNotification` directly.

```livecode
PostUserNotification "Backup Complete", "Your files have been saved", "3 files backed up to /Volumes/Backup", "com.myapp.backup"
```

---

## API Reference

### `RequestNotificationAuthorization`

**Type:** command

**Syntax:** `RequestNotificationAuthorization`

**Summary:** No-op stub provided for call-site compatibility with the modern UNUserNotification library.

**Description:**

This command is provided so that call sites are drop-in compatible with the modern `UNUserNotification` library. It does nothing.

`NSUserNotificationCenter` does not require a runtime authorization request. Notification permission is controlled solely via System Preferences on macOS 10.8 through 10.13.

**Example:**
```livecode
RequestNotificationAuthorization
```

---

### `PostUserNotification`

**Type:** command

**Syntax:** `PostUserNotification <pTitle>, <pSubTitle>, <pInformativeText>, <pIdentifier>`

**Summary:** Posts a local notification via the legacy NSUserNotificationCenter API (macOS 10.8–10.13).

**Parameters:**

| Parameter | Description |
|---|---|
| `pTitle` | The bold heading text displayed at the top of the notification. Required. |
| `pSubTitle` | A secondary heading displayed below the title. Pass empty to omit. |
| `pInformativeText` | The body text of the notification. Pass empty to omit. |
| `pIdentifier` | A unique string identifier for this notification. Pass empty to omit. |

**Description:**

Use `PostUserNotification` to post a local notification via the `NSUserNotificationCenter` API, available on macOS 10.8 through 10.13.

Unlike the modern `UNUserNotification` library, no prior call to `RequestNotificationAuthorization` is required. Notifications are delivered immediately rather than after a delay.

> **Note:** `NSUserNotificationCenter` was deprecated in macOS 10.14 and is non-functional on macOS 12 and later. For macOS 10.14 and above, use the `UNUserNotification` extension instead.

This command has no effect on non-Mac platforms.

**Examples:**
```livecode
PostUserNotification "Backup Complete", "Your files have been saved", "3 files backed up to /Volumes/Backup", "com.myapp.backup"
```
```livecode
PostUserNotification "Hello", "", "", ""
```

---

## Migrating to UNUserNotification

This library exposes identical handler names to the modern `UNUserNotification` extension, so migration is straightforward:

1. Replace this extension with `UNUserNotification.lcb` in the Extension Manager
2. Add a call to `RequestNotificationAuthorization` in your `openStack` handler
3. Ensure your app has a bundle identifier set in Application Settings

No changes to your `PostUserNotification` call sites are required.
