# Flutter Firebase FCM Integration

This project demonstrates how to integrate Firebase Cloud Messaging (FCM) into a Flutter application for push notifications.

## Features

- User authentication with Firebase
- Push notifications with FCM

1. Set Up Firebase Project:

Go to the Firebase Console.
Create a new project or use an existing one.
Add your Android and/or iOS app to the Firebase project by following the setup wizard. Download the google-services.json file for Android and GoogleService-Info.plist for iOS, and place them in the appropriate directories of your Flutter project.

2. Add Firebase Dependencies:

Open your pubspec.yaml file and add the necessary dependencies: 
  firebase_core: latest_version
  firebase_messaging: latest_version
Run flutter pub get to install the dependencies.

3.Configure Firebase for Android:

In your android/build.gradle file, add the Google services classpath:
dependencies {
    classpath 'com.google.gms:google-services:4.3.10'
}

4. Configure Firebase for iOS:

Open the ios/Runner.xcworkspace in Xcode.
In the project navigator, select your project, then the target, and add the GoogleService-Info.plist file to the Runner target.
Ensure your Podfile has the Firebase dependencies:
platform :ios, '10.0'

target 'Runner' do
  use_frameworks!
  use_modular_headers!

  pod 'Firebase/Core'
  pod 'Firebase/Messaging'
end

5. Initialize Firebase in Your Flutter App:
   import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_messaging/firebase_messaging.dart';
import 'package:flutter/material.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  FirebaseMessaging.onBackgroundMessage(_firebaseMessagingBackgroundHandler);
  runApp(MyApp());
}

Future<void> _firebaseMessagingBackgroundHandler(RemoteMessage message) async {
  await Firebase.initializeApp();
  print("Handling a background message: ${message.messageId}");
}

6. Set Up FCM Notifications:

Configure Firebase Messaging to handle foreground, background, and terminated notifications:
class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  late FirebaseMessaging _messaging;

  @override
  void initState() {
    super.initState();
    _messaging = FirebaseMessaging.instance;

    FirebaseMessaging.onMessage.listen((RemoteMessage message) {
      RemoteNotification? notification = message.notification;
      AndroidNotification? android = message.notification?.android;
      if (notification != null && android != null) {
        // Handle foreground notification
      }
    });

    FirebaseMessaging.onMessageOpenedApp.listen((RemoteMessage message) {
      // Handle notification when app is opened from terminated state
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('FCM Example')),
        body: Center(child: Text('Welcome to Flutter FCM')),
      ),
    );
  }
}
