
# UI platform for Ergo dApps

## Introduction

Ergo blockchain is open which is one of its core values. It should be accessible from any
device and any platform. The requirement for hardware must be as low as possible. The
basic development framework needs to be flexible and robust to allow a single codebase to
run on mobile, web and desktop. In addition, security of the user application is
especially important to guarantee safety of user secrets (private keys, mnemonic phrases
etc).

Ergo smart contracts are based on the extended UTXO model, which allows complex
multi-stage dApp scenarios like DEX, ErgoMix, Crowd Funding, Notary, ICO, LETS, Ring
Signatures etc. All this scenarios require full
[ErgoTree](https://ergoplatform.org/docs/ErgoTree.pdf) interpreter to be available as part
of the Ergo UI framework to sign transaction locally, without sending any secrets out of
the user device. This is because all for each input the Ergo application need to execute
the guarding script to sign the input. This is requirement of Ergo's extended UTXO model.

True cross-platform UI application should run in the following environments:
- Android 5.x of newer (ARM)
- iOS 8 or newer (iPhone 4S or newer)
- Web (Firefox, Chrome, Safari, Egde)
- Desktop (macOS, Windows, Linux)

## The Problem with ErgoTree Interpreter

Since the ErgoTree interpreter is implemented in Scala it may be compiled using Scala 2.11
which procudes Java compatible library. The library can be used in Android
project, [this works well](https://github.com/aslesarenko/ergo-android), but what about
iOS. The first idea which comes to mind is to try [Graal
native-image](https://www.graalvm.org/docs/reference-manual/native-image/) to generate
native library with ErgoTree interpreter, which can be used in a native iOS application.
Unfortunately [native-image doesn't support
iOS](https://github.com/oracle/graal/issues/373#issuecomment-563435260) and will not have
it in the foreseen future.
[Existing solution](https://github.com/gluonhq/client-samples) is closed source and cannot
be used for security reasons.
This leave us with the problem of porting ErgoTree interpreter into a language which can
generate iOS compatible library (C/C++, Rust, Swift etc).

Let's say we did it, now what about Web? We cannot use native library in a browser, should
we consider porting ErgoTree Interpreter to JavaScript as well?

Next idea is [Kotlin for
cross-platform](https://www.jetbrains.com/lp/mobilecrossplatform/), we can port the
interpreter to pure Kotlin and [compile it to
JavaScript](https://kotlinlang.org/docs/reference/js-overview.html) library to be used in
the web application.
Then we can compile the same code using
[kotlin-native](https://kotlinlang.org/docs/reference/native-overview.html) and use the
resulting library in the iOS application.
And of cause the can use Kotlin in the Android application.

This unfortunately doesn't work either. The obstacle, is the
[BouncyCastle](https://www.bouncycastle.org/java.html) Java library which is used by the
interpreter for elliptic curve cryptography and hash functions. This library cannot be
transpiled neigher to JavaScript nor a native library.

Surely, we can find and use elliptic curve and hash implementations in JavaScript and
C/C++. This, however, will make porting a more complex task, considering the strict
security requirements of this component.

And finally, even after solving the interpreter problem, there is one more thing. 
If we want consistent GUI accross all platforms (Android, iOS, Web, Desktop) should we 
develop GUI components separately for each platform? Ideally not.
It turns out, as we will see later, we can solve both the problem of interpreter and 
the cross-platform GUI in a elegant, albeit somewhat unexpected way.

## Cross-platform SDKs for UI

SDK     | Android | iOS | Web | MacOS | Windows | Linux
--------|---------|-----|-----|-------|---------|------
[React Native](https://react-native.org/)      | + | +   |     |       |         |
[Native Script](https://www.nativescript.org/) | + | +   |     |       |         |
[Flutter](https://flutter.dev/)                | + | +   | +   | +     | +       | +
[Ionic](https://ionicframework.com/)           | + | +   | +   | +     | +       | +

React Native and Native Script don't support Web and desktop applications (and don't plan
to support). We will not further consider them as the UI platform for Ergo applications.
The following subsections summarize Flatter and Ionic - the two true cross
platform SDKs.

### Comparisons

There are many, here are some of them to start with:
- [Flutter, React Native, Ionic or NativeScript?](https://www.youtube.com/watch?v=PKRXbLnfXXk)
- [React Native vs. Ionic vs. Flutter](https://codeburst.io/react-native-vs-ionic-vs-flutter-comparison-of-top-cross-platform-app-development-tools-71c8011309ac)
- [React Native vs Ionic: Comparing performance, User Experience and much more!](https://www.simform.com/react-native-vs-ionic/)
- [Ionic vs. Flutter: Which one Works for You?](https://www.clariontech.com/blog/ionic-vs.-flutter-which-one-works-for-you)

## Flutter
 Backed by Google, actively developed and promoted since 2017.
 Now Flutter is not only among [top and trending
 projects](https://octoverse.github.com/#footnote--top-and-trending-projects) on GitHub,
 but also it is second the fastest growing open source project by contributors. 
 It’s not surprising that Dart gained contributors in 2019 and became the fastest growing
 language of the year. This also correlates with
 [google trends](https://trends.google.com/trends/explore?date=today%205-y&q=%2Fg%2F11f03_rzbg,React%20Native,Native%20Script,%2Fg%2F1q6l_n0n0).
 
 [What devices and OS versions does Flutter run on?](https://flutter.dev/docs/resources/faq#what-devices-and-os-versions-does-flutter-run-on)
 
#### Web
 Does Flutter run on the web? Yes, see [instructions](https://flutter.dev/docs/get-started/web)
 
#### Desktop
 Can I use Flutter to build desktop apps? Yes, but right now it’s not very well supported.
 [current progress](https://github.com/flutter/flutter/wiki/Desktop-shells)
 macOS is the most developed of the desktop platforms, see
 [documentation](https://flutter.dev/desktop)
 
#### Cryptography
 The only cryptographic library used in ErgoTree Interpreter is
 [BouncyCastle](https://www.bouncycastle.org/java.html). It is ported to Dart and can be
 used in Flutter apps via [PointyCastle package](https://pub.dev/packages/pointycastle),
 which is available for Android, iOS and Web.
 (see also [GitHub repository](https://github.com/bcgit/pc-dart)). The library is
 maintained by the same team and tested against Java version.
 
#### Security
Flutter compiles and packages all dependencies into native code. You can control, in a simple
way, which code you package. This is important for security. This may be less flexible
then using standard Web technologies of Ionic, but it is definitely more secure.

#### Flutter References
  - [FAQ](https://flutter.dev/docs/resources/faq)
  - [Flutter for the JS Developer](https://www.youtube.com/watch?v=7sJZi0grFR4)
  
#### Why Flutter and Dart

The Dart 1.x is quite old language which google has created for general purpose.
However starting from Dart 2.x the language was re-purposed specifically for UI development.
Flutter is UI SDK which unleashes the full potential of both Dart and 
the cross-platform UI development approach.
Since then the Dart language and the Flutter SDK evolve very fast. What is even more
important is that Flutter and Dart co-evolve, so that Flutter is often motivates evolution
of Dart.

Flutter have a unique declarative UI programming model based on reusable Widgets. This
programming style is directly supported by new features added to Dart in the 2.x
generation.

The distinctive features of Dart:
- sub-second hot-reload
- 2-3 second fast hot-restart
- Well known Java libs ported (like retrofit, bouncycastle, etc)
- If you are coming from JavaScript, Dart has a lot of JS idioms built into robust tools.
  Well known JS libs migrated (like redux, reselect, etc)
- unified and centralized [package catalog](https://pub.dev/) and [API
reference](https://api.dart.dev/stable/2.7.2/index.html)
- automated [quality scoring](https://pub.dev/help#scoring) for published packages with
[supported plaforms annotation](https://pub.dev/flutter/packages?platform=android+ios+web)
- out of the box support in IDEs (Idea IntelliJ, AndroidStudio, VSCode) with auto-complete
and full code navigation up to the code in the SDK packages.
- well written and complete documentation of the SDK

More details:
- [Why Flutter uses Dart](https://hackernoon.com/why-flutter-uses-dart-dd635a054ebf)
- [What’s Revolutionary about Flutter](https://hackernoon.com/whats-revolutionary-about-flutter-946915b09514)
- [2016 - Flutter's Rendering Pipeline](https://www.youtube.com/watch?v=UUfXWzp0-DU&feature=youtu.be)
- [Why native app developers should take a serious look at Flutter](https://hackernoon.com/why-native-app-developers-should-take-a-serious-look-at-flutter-e97361a1c073)

## Ionic 

In principle Ionic can be considered as a true cross-platform SDK for Ergo UIKit, however
there are obstacles here and there which prevent me from recommending it. 
[This comparison](https://ionicframework.com/resources/articles/ionic-vs-flutter-comparison-guide)
cleary highlights benefits of both Ionic and Flutter (especially Ionic) over alternatives,
like React Native. The post is somewhat biased towards Ionic for obvious reasons.

Basically the only advantage of Ionic is complete reliance on standard web technologies
(including JavaScript) which are familiar to the masses. I would say JS is more like a
disadvantage, but nevertheless it is generally considered a plus. That is it, with all
those Web tech Ionic inherits all of the drawbacks.

So, here comes a list of drawbacks, which make the Ionic story not so rosy:
- Despite basing Ionic on open standards, the framework itself feels proprietary, backed
by a company with [pricing plans](https://ionicframework.com/pricing). Try
watching this [webinar](https://ionicframework.com/resources/webinars/ionic-react).
- The security of Ionic app is basically the security of a web site. We will need follow
at least [this security
guidelines](https://www.joshmorony.com/basic-security-for-ionic-cordova-applications/),
feels scary, isn't it?
- JavaScript is semantically different language, porting ErgoTree Interpreter may become a
challenging adventure.
- There is not BouncyCastle port, which will require reseach and evaluation of elliptic
curve and hashing libraries.
- Desktop support is based on [Electron](https://www.electronjs.org/) which has its own
security concerns.

## Conclusion

Flutter is the best fit for Ergo UIKit:
- Single codebase can run on Android, iOS, Web browser, macOS (later Window and Linux).
- Coincidentally, BouncyCastle crypto library is already ported to Dart and can run on Android, iOS, Web.
  It is maintaines by the same team and well tested against Java version. 
- In the presence of PointyCastle, porting of ErgoTree Intepreter to Dart will be simple because 1) Dart is semantically very
  close to Java; 2) PointyCastle is a clone of BouncyCastle.
- Appkit can be ported to Dart even easier since it is implemented in Java. In fact
Appkit-dart will have simpler implementation because Java-to-Scala bridge is not necessary.
- Plugins allow platform specific customization of application
- Flutter compiles and packages all dependencies into native code. You can control, in a simple
way, which code you package. This is important for security. This may be less flexible
then using standard Web technologies of Ionic, but it is definitely more secure.
- So, keeping all the above in mind, Dart itself turns out to be a benefit.

## References
- [Flutter Examples](https://github.com/flutter/samples)
- [Awesome Flutter](https://github.com/Solido/awesome-flutter)
- [Building a (large) Flutter app with Redux](https://hillel.dev/2018/06/01/building-a-large-flutter-app-with-redux/)
- [Flutter Architecture Samples](http://fluttersamples.com/)
- [Flutter vs React Native: A Developer’s Perspective](https://medium.com/flawless-app-stories/flutter-vs-react-native-a-developers-perspective-8914ca240a89)

