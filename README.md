# Document Camera Frame

The `DocumentCameraFrame` package simplifies document scanning by providing a customizable camera
interface for capturing and cropping document images. It’s ideal for applications that require
efficient and user-friendly document capture.

![example.gif](https://github.com/AhmedZein1996/document_camera_frame/raw/main/example.gif)

## Features

- **Document Frame**: Customizable dimensions for focused document capture.
- **Camera Preview**: Real-time camera feed for instant feedback.
- **User-Friendly Controls**:
    - **Capture**: Take a snapshot of the document.
    - **Save**: Save the captured image.
    - **Retake**: Retake the image if unsatisfactory.
- **Customizable UI**:
    - Define frame dimensions, button styles, and positions.
    - Add optional titles with alignment and padding options.
- **Event Callbacks**: Handle events like `onCaptured`, `onSaved`, and `onRetake` easily.

---

## Installation

Add the package to your Flutter project using:

```bash
flutter pub add document_camera_frame
```

Then run:

```bash
flutter pub get
```

---

## Setup

### iOS Setup

Add the following keys to your `ios/Runner/Info.plist` file to request camera and microphone
permissions:

```xml

<plist version="1.0">
    <dict>
        <!-- Add the following keys inside the <dict> section -->
        <key>NSCameraUsageDescription</key>
        <string>We need camera access to capture documents.</string>
        <key>NSMicrophoneUsageDescription</key>
        <string>We need microphone access for audio-related features.</string>
    </dict>
</plist>
```

### Android Setup

1. Update the `minSdkVersion` to 21 or higher in `android/app/build.gradle`:

```gradle
android {
    defaultConfig {
        minSdk 21
    }
}
```

2. Add these permissions to your `AndroidManifest.xml` file:

```xml

<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

    <application android:label="MyApp" android:name="${applicationName}"
        android:icon="@mipmap/ic_launcher">
        <!-- Activities and other components -->
    </application>

</manifest>
```

---

## Handling Camera Access Permissions

Permission errors may occur when initializing the camera. You must handle them appropriately. Below
are the possible error codes:

| **Error Code**                    | **Description**                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------|
| `CameraAccessDenied`              | User denied camera access permission.                                                            |
| `CameraAccessDeniedWithoutPrompt` | iOS only. User previously denied access and needs to enable it manually via Settings.            |
| `CameraAccessRestricted`          | iOS only. Camera access is restricted (e.g., parental controls).                                 |
| `AudioAccessDenied`               | User denied microphone access permission.                                                        |
| `AudioAccessDeniedWithoutPrompt`  | iOS only. User previously denied microphone access and needs to enable it manually via Settings. |
| `AudioAccessRestricted`           | iOS only. Microphone access is restricted (e.g., parental controls).                             |

---

## Usage

### Import the Package

```dart
import 'package:document_camera_frame/document_camera_frame.dart';
```

### Example

```dart
import 'package:document_camera_frame/document_camera_frame.dart';
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: const HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  const HomeScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Document Capture Frame')),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            // Navigate to the DocumentCameraFrame example screen when the button is pressed
            Navigator.of(context).push(
              MaterialPageRoute(builder: (context) => const DocumentCameraFrameExample()),
            );
          },
          child: const Text('Start Document Capture'),
        ),
      ),
    );
  }
}

class DocumentCameraFrameExample extends StatelessWidget {
  const DocumentCameraFrameExample({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: DocumentCameraFrame(
        // Document frame dimensions
        frameWidth: 300.0,
        frameHeight: 200.0,

        // Title displayed at the top
        title: const Text(
          'Capture Your Document',
          style: TextStyle(
            color: Colors.white,
            fontSize: 20,
            fontWeight: FontWeight.bold,
          ),
        ),

        // Show Close button
        showCloseButton: true,

        // Callback when the document is captured
        onCaptured: (imgPath) {
          debugPrint('Captured image path: $imgPath');
        },

        // Callback when the document is saved
        onSaved: (imgPath) {
          debugPrint('Saved image path: $imgPath');
        },

        // Callback when the retake button is pressed
        onRetake: () {
          debugPrint('Retake button pressed');
        },

        // Optional: Customize Capture button, Save button, etc. if needed
      ),
    );
  }
}

```

---

## Widget Parameters

| Parameter                    | Type               | Description                                                                      | Required | Default Value                  |
|------------------------------|--------------------|----------------------------------------------------------------------------------|----------|--------------------------------|
| `frameWidth`                 | `double`           | Width of the document capture frame.                                             | ✅        | —                              |
| `frameHeight`                | `double`           | Height of the document capture frame.                                            | ✅        | —                              |
| `title`                      | `Widget?`          | Widget to display as the screen's title (optional).                              | ❌        | `null`                         |
| `screenTitleAlignment`       | `Alignment?`       | Alignment of the screen title (optional).                                        | ❌        | `Alignment.topCenter`          |
| `screenTitlePadding`         | `EdgeInsets?`      | Padding for the screen title (optional).                                         | ❌        | `EdgeInsets.zero`              |
| `captureButtonText`          | `String?`          | Text for the "Capture" button.                                                   | ❌        | `"Capture"`                    |
| `captureButtonTextStyle`     | `TextStyle?`       | Text style for the "Capture" button text (optional)                              | ❌        | `null`                         |
| `captureInnerCircleRadius`   | `double?`          | Radius of the inner circle of the capture button (optional).                     | ❌        | `59`                           |
| `captureOuterCircleRadius`   | `double?`          | Radius of the outer circle of the capture button (optional).                     | ❌        | `70`                           |
| `captureButtonStyle`         | `ButtonStyle?`     | Style for the "Capture" button (optional).                                       | ❌        | `null`                         |
| `captureButtonAlignment`     | `Alignment?`       | Alignment of the "Capture" button (optional).                                    | ❌        | `Alignment.bottomCenter`       |
| `captureButtonPadding`       | `EdgeInsets?`      | Padding for the "Capture" button (optional).                                     | ❌        | `null`                         |
| `captureButtonWidth`         | `double?`          | Width for the "Capture" button (optional).                                       | ❌        | `null`                         |
| `captureButtonHeight`        | `double?`          | Height for the "Capture" button (optional).                                      | ❌        | `null`                         |
| `onCaptured`                 | `Function(String)` | Callback triggered when an image is captured.                                    | ✅        | —                              |
| `saveButtonText`             | `String?`          | Text for the "Save" button.                                                      | ❌        | `"Save"`                       |
| `saveButtonTextStyle`        | `TextStyle?`       | Text style for the "Save" button text (optional).                                | ❌        | `null`                         |
| `saveButtonStyle`            | `ButtonStyle?`     | Style for the "Save" button (optional).                                          | ❌        | `null`                         |
| `saveButtonAlignment`        | `Alignment?`       | Alignment of the "Save" button (optional).                                       | ❌        | `Alignment.bottomRight`        |
| `saveButtonPadding`          | `EdgeInsets?`      | Padding for the "Save" button (optional).                                        | ❌        | `null`                         |
| `saveButtonWidth`            | `double?`          | Width for the "Save" button (optional).                                          | ❌        | `null`                         |
| `saveButtonHeight`           | `double?`          | Height for the "Save" button (optional).                                         | ❌        | `null`                         |
| `onSaved`                    | `Function(String)` | Callback triggered when an image is saved.                                       | ✅        | —                              |
| `retakeButtonText`           | `String?`          | Text for the "Retake" button.                                                    | ❌        | `"Retake"`                     |
| `retakeButtonTextStyle`      | `TextStyle?`       | Text style for the "Retake" button text (optional).                              | ❌        | `null`                         |
| `retakeButtonStyle`          | `ButtonStyle?`     | Style for the "Retake" button (optional).                                        | ❌        | `null`                         |
| `retakeButtonAlignment`      | `Alignment?`       | Alignment of the "Retake" button (optional).                                     | ❌        | `Alignment.bottomLeft`         |
| `retakeButtonPadding`        | `EdgeInsets?`      | Padding for the "Retake" button (optional).                                      | ❌        | `null`                         |
| `retakeButtonWidth`          | `double?`          | Width for the "Retake" button (optional).                                        | ❌        | `null`                         |
| `retakeButtonHeight`         | `double?`          | Height for the "Retake" button (optional).                                       | ❌        | `null`                         |
| `onRetake`                   | `VoidCallback?`    | Callback triggered when the "Retake" button is pressed.                          | ❌        | `null`                         |
| `frameBorder`                | `BoxBorder?`       | Border for the displayed frame (optional).                                       | ❌        | `null`                         |
| `capturingAnimationDuration` | `Duration?`        | Duration for the capturing animation (optional).                                 | ❌        | `Duration(milliseconds: 1000)` |
| `capturingAnimationColor`    | `Color?`           | Color for the capturing animation (optional).                                    | ❌        | `Colors.black26`               |
| `capturingAnimationCurve`    | `Curve?`           | Curve for the capturing animation (optional).                                    | ❌        | `Curves.easeInOut`             |
| `outerFrameBorderRadius`     | `double`           | Radius of the outer border of the frame.                                         | ❌        | `12.0`                         |
| `innerCornerBroderRadius`    | `double`           | Radius of the inner corners of the frame.                                        | ❌        | `8.0`                          |
| `animatedFrameDuration`      | `Duration`         | Duration for the frame animation (optional).                                     | ❌        | `Duration(milliseconds: 400)`  |
| `animatedFrameCurve`         | `Curve`            | Curve for the frame animation (optional).                                        | ❌        | `Curves.easeIn`                |
| `bottomFrameContainerChild`  | `Widget?`          | Custom content for the bottom container (optional).                              | ❌        | `null`                         |
| `showCloseButton `           | `bool`             | Flag to control the visibility of the CloseButton (optional).                    | ❌        | `false`                        |
| `cameraIndex `               | `int`              | Index to specify which camera to use (e.g., 0 for back, 1 for front) (optional). | ❌        | `0` (back)                     |

---

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
