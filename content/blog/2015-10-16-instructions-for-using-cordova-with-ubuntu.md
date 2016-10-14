+++
title = "Make an Android app using Cordova in 15 minutes from freshly installed Ubuntu [Full Instructions]"
date = "2015-10-16"
+++

## Setup

### Install Java 7

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer
sudo apt-get install oracle-java7-set-default
```

And check if its installed using `javac --version`.

### Install Android Studio

First install some dependencies for Ubuntu (or wait until install fails to install...)_

```
sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1
```
[Click here](http://developer.android.com/sdk/index.html#Other) to download the latest Android Studio package. Unpack the downloaded ZIP file into an appropriate location for your applications.

Add this to your `.profile`:

```
export PATH=$PATH:/path/to/android-studio/bin # where you unzipped the package
export ANDROID_HOME=/home/XXX/Android/Sdk
source ~/.profile
```

Start Android studio

```
studio.sh
```

### Enable a emulator (optional)

```
sudo apt-get install qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils
```

Restart computer, hit DEL and enable Virtualization.

Start Android Studio and then use ```Tools -> Android -> AVD Manager```. Setup an Android using Lollipop x86.

### Install Cordova

```
sudo npm install -g cordova
```

## Setup app

### Generate new app

```
cordova myapp myapp com.myname.myapp myapp
cd fingerprinting
```

### Install plugins

```
cordova plugin add nl.nielsad.cordova.wifiscanner
cordova plugin add https://github.com/katzer/cordova-plugin-background-mode.git
```

### Run / deploy

```
cordova platform add android
cordova build android
cordova emulate android       #to deploy the app on a default android emulator
cordova run android --device  #to deploy the app on a connected device
```
