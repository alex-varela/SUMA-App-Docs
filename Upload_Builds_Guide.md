
# Manual Upload - Builds for iOS and Android (Manually)

## steps:

1. Modify the Android app version following [versioning semantic]().
1. To prepare builds for any platform is necessary select the environment target `dev`,`qa` or `prod` using [Environment Commands Table]().
2. Restart and reset metro cache using `npx react-native start --reset-cache`.
3. Follow steps for each platform

>Recomendations:
> - Test build in both platform.
> - In case send builds before accepting the current pull request with new changes create builds from your current branch if it passes the previous smoke test for each environment.

### IOS
> IOS builds are send to testflight to distribute to testers 
>
> Definition:
> - IPA: is an iPhone application archive file that stores an iPhone app. It is usually encrypted with Apple's FairPlay DRM technology. Each .ipa file is compressed with a binary for the ARM architecture and can only be installed on an iPhone, iPod Touch, or iPad.

1. Open rootFolder/ios/sumaPoC.xcworkspace
1. Increase the `build number` 30, 31,..,N.  
2. Choose the right scheme `SUMA Wealth Dev`, `SUMA Wealth QA`, or `sumaPoC` and change the current target to any IOS device (arm64).
3. Click on Product/archive and wait for this process
4. When the archive process finishes, it will open the Organizer (rute Windows/organizer) with a list of archive builds.
5. Click on “Distribute App” and follow the steps: 
	- Choose “App Store Connect” and click on next.
	- Choose “Upload” and click on “next” and wait for fetching AppStore configuration
	- Click on “next”.
	- Choose the Distribution Certificate and the profile.
	- Click on “upload” and wait until it finishes.
7. And then go to [SUMA AppStoreConnect profile](https://appstoreconnect.apple.com/apps).
8. Select application according to the app scheme uploaded.
> Sometimes the new version doesn't appears instantly after uploading process finishes in this case is necessary wait and refresh appStore connect.  
> The most recent build could be in “processing” or “missing compliance” state, if it is "processing" just need wait for that process finishes and when it change to "missing complaince" the build is ready to distribute to testers.

9. Go to the version and add a changelog on "Test details":
```
Changelog:
- Feature
- Feature
```
10. Click on Missing Compliance and follow the steps and then start for internal testers (In those questions the answer should be YES because we use HTTPS)

### Android

> Definition:
> - AAB: An Android App Bundle is a publishing format that includes all your app’s compiled code and resources, and defers APK generation and signing to Google Play.
> - APK: An Android Package Kit is the file format for applications used on the Android operating system. APK files are compiled with Android Studio, which is the official integrated development environment (IDE) for building Android software.
> 
> Recomentations: 
> 1. Create a folder in your computer where do you will save .aab and .apk. In this folder you should create a new folder per each new build using the version name example `x.x.x.x`. 
> 2. Flavor folders `dev`, `qa` and `prod` are create automatically after generate an `.aab` or `.apk` and it generate build variant folder depending on your election `release` or `debug`



1. Open Android studio and select rootFolder/android.
2. Go to `Build` and select `Generate Signed Bundle/APK`.
3. Choose the option that you need `AAB` or `APK`.
4. And then add the keystore password.
5. Select a folder to save your builds and select the build variant that you need `release` or `debug`

##### Android builds output structure
```
AndroidBuilds
	|
	|-/version
		|
		|-/dev
			|-/release
			|-/debug
		|/qa
			|-/release
			|-/debug
		|/prod
			|-/release
			|-/debug
```
6. Go to your android build output folder and go to `x.x.x.x`/`flavor folder`/`variant folder`
and change your build name from `relase.aab` to `(App Name)_(Flavor)_(App version).aab`.
7. And then go to [SUMA Googleplay console](https://play.google.com/console/developers/app/releases/overview) and select the application that you need send to test
8. Click on "Internal testing"
9. Click on "Create new release"
9. In "App bundles" click on upload and select your app version and then wait for the upload finishies
11. Change release name to the [app versioning]() convention. By deafult android applies this format `VERSION_CODE(VERSION_NAME)`.
12. And the add your changelog to Release notes.
13. Click on "Review release"





-----

