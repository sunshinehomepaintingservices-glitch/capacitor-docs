# 📱 README-android.md

## Attendance App – Android Setup (Capacitor)

This guide explains how to run and build the **Attendance App** as an Android APK / AAB using Capacitor.

---

## 🔧 Requirements

- [Node.js](https://nodejs.org/) (v16+ recommended)  
- [Android Studio](https://developer.android.com/studio)  
- Java JDK (11 or newer)  
- Backend server (OTP + Attendance API). For local dev, expose with **ngrok**.  

---

## ⚙️ 1. Capacitor Config

Create or edit `capacitor.config.json` in project root:

```json
{
  "appId": "com.example.attendance",
  "appName": "attendance-app",
  "webDir": "build",
  "bundledWebRuntime": false
}
```

👉 If you use Vite, change `"webDir": "dist"`.

---

## 📂 2. Package.json Scripts

In `frontend/package.json`, add:

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview",
  "android:copy": "npx cap copy android",
  "android:open": "npx cap open android",
  "android:sync": "npx cap sync android"
}
```

---

## 🌐 3. API Base URL

Create a config file `src/config.js`:

```js
export const API_BASE_URL =
  import.meta.env.VITE_API_URL || "http://localhost:4000";
```

Usage example:

```js
import { API_BASE_URL } from "./config";

await fetch(`${API_BASE_URL}/send-otp`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ username, phone }),
});
```

For dev: run backend locally and expose it:

```bash
ngrok http 4000
```

Set `.env` in frontend:

```
VITE_API_URL=https://<your-ngrok-id>.ngrok.io
```

---

## 📱 4. Add Android Platform

From `frontend/`:

```bash
npx cap add android
```

---

## 🏗️ 5. Build & Run (Dev Mode)

1. Build frontend:
   ```bash
   npm run build
   ```

2. Copy to Android project:
   ```bash
   npm run android:copy
   ```

3. Open in Android Studio:
   ```bash
   npm run android:open
   ```

4. Run ▶ on emulator or real device.

---

## 📜 6. Android Permissions

In `android/app/src/main/AndroidManifest.xml` add:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

---

## 📦 7. Build Release APK / AAB

### From Android Studio:
- **Build > Generate Signed Bundle / APK**
- Select **Android App Bundle (AAB)** for Play Store  
- Create or use existing keystore  
- Generate signed file (`.aab` / `.apk`)

### From CLI:
```bash
cd android
./gradlew bundleRelease   # Generate AAB
./gradlew assembleRelease # Generate APK
```

---

## 🚀 8. Play Store Upload

- Go to [Google Play Console](https://play.google.com/console/)  
- Create app, upload **AAB**  
- Fill in app details, screenshots, privacy policy (mandatory if collecting personal data).  

---

✅ Now your **Attendance App** is ready for Android release!
