# CI CD for Xamarin.Forms Hello world project

---

## Description

This repository contains Jenkins config with the Xamarin.Forms **HelloWorld** app(https://github.com/adityaoberai/xamarin-forms-helloworld) for Android and IOS. This provides basic visibility of CI CD for xamarin forms apps.

<br>
<br>

---

## Prerequisites For This Activity

* Knowledge of **Jenkins** helps

---

## Developing Android App

### Prerequisites

1. Docker with linux os
2. Jenkins
3. Nexus

### Step 1: Create a new Xamarin.Forms Android app

1. Pull container - tafilz/xamarin-android (which is contained mono framwork, Android Studio, Nuget)

2. Update docker config by installing required libs like JDK11, Git, Maven, Openssh etc.

3. Write entrypoint or CMD to copy SSH_PUB_KEY through env vars.

4. Build Docker image by using above config

5. Use custom docker image to connect Jenkins and to build Android Xamarin.Forms App (Please refer jenkinsfile in the repo)

---

## Developing IOS App

### Prerequisites

1. Mac OS
2. Jenkins
3. Nexus

### Step 1: Create a new Xamarin.Forms app

1. Setup Xamarin build environment by installing tools/libs - .Net, Visusla Studio, Mono framework, Nuget, Xamarin profile etc.

2.Build Xamarin IOS as mentioned in Jenkinsfile in the project. 

#### Note: Not implemented in realtime. 

---

## Resources
**Source Code** : https://github.com/adityaoberai/xamarin-forms-helloworld
**For Android build env** : https://hub.docker.com/r/tafilz/xamarin-android

