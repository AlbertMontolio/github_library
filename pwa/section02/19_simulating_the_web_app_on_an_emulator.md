

android studio

we want android studio because of the android virtual device

tools / android / AVD Manager

more info here
https://developer.android.com/studio/run/managing-avds

in case it does not work

Easier Way To Access On Mobile
Elvis · Lecture 19 · 7 months ago
If you want to avoid having to set up Android Studio or use an emulator at all, you can simply navigate to the ip address of your computer from your phone directly.

First find the ip address of your computer (there will be many unique ip addresses for all devices connected to your network, so make sure you find your desktop specifically!) and then go to chrome and search for: "http://[your-ip-address]:[port]", filling in your details in the brackets of course.


but it works

cmd + shift + a -> avd

virtual devices allow you to test your application without having to own the physical devices

virtual devices,
click the play

in the phone, we visit

10.0.2.2:8080

(the server must be running, npm start)

3 dots (in android) add to home screen


Updating Chrome on the Virtual Device

With an emulated device up and running, you're well-prepared to test your manifest.json file. For other features you learn about in this course, the pre-installed Chrome browser is too old though (at least at the point of time this course is created).

You can easily update Chrome on your virtual device though. Get an updated APK (basically the app installation file) from this link: https://www.apkmirror.com/apk/google-inc/chrome/#variants

Feel free to choose the latest one, download and install it. Give the device the permission to install from "unsafe" sources. If it fails, try a different APK version.







