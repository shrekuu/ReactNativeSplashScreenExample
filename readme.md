# react-native-splash-screen demo

## Install and Link

```bash
yarn add react-native-splash-screen
react-native link react-native-splash-screen
```

## Generate launch images from one Image (Optional if you have created your splash images)

[https://apetools.webprofusion.com/app/#/tools/imagegorilla](https://apetools.webprofusion.com/app/#/tools/imagegorilla)

## Hide splash screen when component is ready. Changes in `App.js`

  ```diff
  @@ -9,6 +9,7 @@

  import React, {Component} from 'react';
  import {Platform, StyleSheet, Text, View} from 'react-native';
  +import SplashScreen from 'react-native-splash-screen';

  const instructions = Platform.select({
    ios: 'Press Cmd+R to reload,\n' + 'Cmd+D or shake for dev menu',
  @@ -19,6 +20,13 @@ const instructions = Platform.select({

  type Props = {};
  export default class App extends Component<Props> {
  +
  +  componentDidMount() {
  +    // hide the splashscreen when the app is ready
  +    // you can do some async actions before this
  +    SplashScreen.hide()
  +  }
  +
    render() {
  ```

## For Android

* Copy launch images into directories in `android/app/src/main/res` respactively
* Create `launch_screen.xml` file in `android/app/src/main/res/layout/launch_screen.xml` (create `layout` directory if it does not exist)

* Copy This into `launch_screen.xml` file. Note the `android:src="@mipmap/launch_screen"` part, `mipmap` is the dir name prefixes in `res` directory. We use `ImageView` to [maitain the aspect ratio](https://github.com/crazycodeboy/react-native-splash-screen/issues/252).

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <ImageView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:scaleType="centerCrop"
    android:src="@mipmap/launch_screen" />

  ```

* (optional) Give statusbar a background deeppink color when app launches. Create `android/app/src/main/res/values/colors.xml` and paste in the following code. The `primary_dark` color here seems useless for now, but it is metioned in the doc. We add a color for statusbar here.

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <resources>
      <color name="primary_dark">#000000</color>
      <color name="status_bar_color">#ff1493</color>
  </resources>
  ```

* Add launch status style. Changes in `android/app/src/main/res/values/styles.xml`

  ```diff
          <!-- Customize your theme here. -->
      </style>

  +    <style name="SplashScreenTheme" parent="SplashScreen_SplashTheme">
  +        <item name="colorPrimaryDark">@color/status_bar_color</item>
  +    </style>
  +
  </resources>
  ```

* Apply the statusbar style and show the launch image. Changes in `android/app/src/main/java/com/reactnativesplashscreenexample/MainActivity.java`

```diff

@@ -2,8 +2,17 @@ package com.reactnativesplashscreenexample;

 import com.facebook.react.ReactActivity;

+import android.os.Bundle;
+import org.devio.rn.splashscreen.SplashScreen;
+
 public class MainActivity extends ReactActivity {

+    @Override
+    protected void onCreate(Bundle savedInstanceState) {
+        SplashScreen.show(this, R.style.SplashScreenTheme);
+        super.onCreate(savedInstanceState);
+    }
```

Now you can run `react-native run-android`.

## For iOS

* Remember to config signing.

  ![ios-launch-image-config-1.png] (screenshots/ios-launch-image-config-1.png)

* Add Launch images.

  ![ios-launch-image-config-2.png](/screenshots/ios-launch-image-config-2.png)

* Config launch screen.

  ![ios-launch-image-config-3.png](/screenshots/ios-launch-image-config-3.png)

* show the launch image. Changes in `AppDelegate.m`

  ```diff
  #import <React/RCTBundleURLProvider.h>
  #import <React/RCTRootView.h>
  +#import "RNSplashScreen.h"

  @implementation AppDelegate

  @@ -29,6 +30,7 @@
    rootViewController.view = rootView;
    self.window.rootViewController = rootViewController;
    [self.window makeKeyAndVisible];
  +  [RNSplashScreen show];
    return YES;
  }
  ```




