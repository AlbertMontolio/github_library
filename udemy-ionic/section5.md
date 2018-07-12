# Module Content

test your application in real devices

running the app on ios

running the app on android

# 95. Where to get started

ionic documentation, setup, deploying

installation, at the very end, two links to the Cordova documentation

# 96. Building for iOS

```
xcode-select --install
```

```
npm install -g ios-deploy
```


if its not working, force the instlalation

```
--unsafe-perm=true
```

we need to open the app in xcode for the first time

```
ionic platform add ios
```

```
The platform command has been renamed. To find out more, run:

  ionic cordova platform --help
```

```
ionic cordova platform add ios
```

```
ionic build ios
```

```
ionic cordova build ios
```


open xcode, navigate to your app, platform, open .xcodeproj

we dont see a team in the project, in signing

go to xcode/preferences, apple id, we have one

view details, provisioning profiles

you need to setup a signing identity,

manage certificates
ios Development

select on the top the device and click the play


holy shit, is working

run the same from the terminal

we need the singing certificate

```
ionic cordova run ios --device
```
we need to connect our device

```
ionic emulate ios
```

# 98. Lists & Performance Issues

When using lists in your application, you can hit performance issues - especially if those lists are very long and complex.

Typically, such lists are not rendered as whole but only in fragments - only the part currently shown on the screen. In web apps (and therefore in your Angular app), this does not work like this - lists are loaded as whole (unless you implement you own lazy-loading mechanism).

Ionic 2 offers a special directive you may use to fix potential performance issues => VirtualScroll.

On a basic list, you use it like this:

```
<ion-list [virtualScroll]="items">
    <ion-item *virtualItem="let item">
        {‌{ item }}
    </ion-item>
</ion-list>
```

This will ensure, that only the visible part of the screen gets rendered. Dive into the above article to learn more about this functionality.




























