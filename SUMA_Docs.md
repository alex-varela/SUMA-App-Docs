# General

## Install and  setup react-native
- [Follow this guide for React Native CLI - macOS - iOS and android](https://reactnative.dev/docs/environment-setup)


#### Recommended versions
``` plainText

 - React Native: v0.65.1
 - Node: 14.18.1 * we recommend using nvm
 - Yarn: 1.22.10 
 - npm: 6.14.15 
 - Watchman: 2022.02.14.00 
 - CocoaPods: 1.11.3

```
> (optional to run from terminal instead of xcode) ios-deploy tool: `npm install -g ios-deploy` (this option is not working for now)

## Setup the repo 

 1. Clone the repo: `git clone git@github.com:Suma-Wealth/Px2-SumaWealthMobile.git`
 2. Move to repo folder: `cd ~/path/To/Repo/Folder`
 3. From project root folder install npm packages: `npm install`
 4. From project root folder install pods: `npx pod-install`
 5. Add environment folder in the root folder.
 6. Select and set a enviroment using those follow commands:


#### Environment commands
  | Environment |Command                          |
  |-------------|---------------------------------|
  | Development | `yarn generate-dev-credentials` |
  | QA     | `yarn generate-qa-credentials`  |
  | Production  | `yarn generate-prod-credentials`|


 > *For development purposes you should always use dev credentials*

---

## Run the SUMA App

> Before select an specific environment make sure to generate the corresponding credentials (development, staging or production).

### Steps:
1. Start react-native `npx react-native start --reset-cache`
2. Follow specific ways to run the project for android and IOS:

#### IOS
> For ios is a must before to open the workspace run `npx pod-install` in the root project

 1. Move to folder ~root/ios
 2. Open sumaPoC.xcworkspace
 3. Change app scheme according your previus environment selected (SUMA Wealth dev, SUMA Wealth QA or sumaPoC).
 4. Select a target emulator or a physic device and press run.

#### Android

For android we have 2 ways to run SUMA app first one is using **Android Studio**:
 1. Open android studio and select rooFolder/android
 1. Select android flavor according your previus environment selected (dev, qa or prod).
 1. Select a target emulator or a physic device and press run.

the second option is using some commands previously set up (those commands should be used on the root SUMA app folder and select an env)


#### Android flavor commands
  |Environment  |Command                 |
  |-------------|------------------------|
  | Development | `yarn run-android-dev` |
  | QA     | `yarn run-android-qa`  |
  | Production  | `yarn run-android`|



> If android install the apk but doesn't connect with metro follow these steps:
>	1. Check if your device is connected to the same net that you computer.
>	1. Check if your device is connected to the computer using `adb devices`.
>	1. Reset metro cache using `npx react-native start --reset-cache`. 
>	1. Run SUMA app for android using previus command showed or using android studio.
>	1. Run this commmand `yarn adb-setup`.
>	1. Close and open SUMA app again.



---

# SUMA app environments 

Environment files are managed by generate-env-credentials.sh this script loads environment variables and replaces specific files for each platform android or ios using commands defined on the [environment commands table](#environment-commands).
This shellscript adds a specific file for any environment per each operating system is necessary to define the file origin dev, qa or prod from the environment folder as route and where that file will be replaced, and finally, this script cleans the android gradle. example:


|Environment  |Command                                         |
|-------------|------------------------------------------------|
|Add          | `yes \| cp -rf <origin/fileName> <targetRoute>`|
|Remove       | `yes \| rm -f -- <targetRoute/fileName>`       |

### Environment folder basic structure:

 ``` 
/env
   |
   |- /dev
		|- .env
		|- /ios
		|- /android
   |
   |- /qa
		|- .env
		|- /ios
		|- /android
   |
   |- /prod
		|- .env
		|- /ios
		|- /android

```

> Cosiderations:
> 1. Other files or keys are added depending on feature implementations or library requirements.  
> 2. New environmental keys/variables/files must be added in each .env file per evironment using same key name.
> 3. After add a new file for every environment is a must add the command on `generate-env-credentials.sh` and define where this file should be replaced and from what environment it will take that file.


- Folders (`/folder name`):
	- **env** : Root folder it must be paste on the root project necessary to change environment credentials/keys/values/files.
	- **dev, qa & prod** : Default environmental folders contains specific folders for each operating system and a general .env file.
	- **ios & android**: Those folders manage specific files per platform by deafult have:
		- **ios**: `info.plist`.
		- **android**: `gradle.properties`.


```
VERSION_CODE=XX
VERSION_NAME='X.X.X'
VERSION_BUILD=XX
```

### SUMA App Semantic Versioning
Versioning is very important when testing and eventually when you release. Usually versioning depends on project, but you can follow a basic template that we have used on Iconic before:
``` plainText
V1.2.3 (4)
```

1. Used for major changes, when a whole new feature is added for example. Usually when there is going to be a major release that changes a major component of the application.

2. Used for minor range changes that does not affect a major component. Like a small feature.

3. Bug fixes and patch changes.

4. The build number, this one is used for testing. This number can either be reset after a release or be continuously incremental throughout the app's development.

You usually don't change the first 3 numbers while you are developing the same fix or feature, instead you use the build number while creating builds for testing. This is because of AppStoreConnect once you are done with everything and are going to release, you want to release the next app version, which you can see on AppStoreConnect. Here, on the App Store tab, you see the latest released build with the last version, but it doesn't show the build number. You can see all of your created builds on the Testflight tab, where you can see the Versions and under them, the builds.

#### Differences between android and iOS versioning

- Android:
	- **Version Code**: `VERSION_CODE` is a positive integer used as an internal version number. This number is used only to determine whether one version is more recent than another, with higher numbers indicating more recent versions. This value increases gradually per build (only for android).
	- **Version Name**: `VERSION_NAME` is a string used as the version number shown to users. The value is a string so that you can describe the app version as a `<major>.<minor>.<patch>`. The versionName has no purpose other than to be displayed to users.

	- **Version build**: `VERSION_BUILD` is a positive integer used for testing. This number can either be reset after a release or be continuously incremental throughout the app’s development.
	> This is a special value for android (not official) added  to customize the user agent name for keep same version displayed for android and iOS, this value should be setting up using the same ios `Build Number`.

- IOS:
	- **Build Version**: CFBundleShortVersionString (String - iOS, OS X) number you are using to denote this version of your app. Usually this follows a `<major>.<minor>.<patch>` version scheme to let users know which releases are smaller maintenance updates and which are big deal new features.
	- **Build Number**: CFBundleVersion (String - iOS, OS X) specifies the build version number of the bundle, which identifies an iteration (released or unreleased) of the bundle. The build version number should be a string comprised of three non-negative, period-separated integers with the first integer being greater than zero.









