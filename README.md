[![Digital public good](https://github.com/cboard-org/cboard/blob/master/public/images/dpg-badge.png)](https://digitalpublicgoods.net/registry/)
[![Crowdin](https://d322cqt584bo4o.cloudfront.net/cboard/localized.svg)](https://crowdin.com/project/cboard)
[![Backers on Open Collective](https://opencollective.com/cboard/backers/badge.svg)](#backers)
[![Sponsors on Open Collective](https://opencollective.com/cboard/sponsors/badge.svg)](#sponsors)
[![cboard-org](https://circleci.com/gh/cboard-org/cboard.svg?style=shield)](https://app.circleci.com/pipelines/github/cboard-org/cboard)
# Cboard - AAC Communication Board for browsers

[Cboard](https://app.cboard.io) is an augmentative and alternative communication (AAC) web application, allowing users with speech and language impairments (autism, cerebral palsy) to communicate with symbols and text-to-speech.

![Cboard GIF demo](public/videos/demo.gif)

The app uses the browser's Speech Synthesis API to generate speech when a symbol is clicked. There are thousands of symbols from the most popular AAC symbol libraries to choose from when creating a board. Cboard is available in 40 languages (support varies by platform - Android, iOS, Windows).

**We're using Discord to collaborate, join us at: https://discord.gg/TEH8uxh**

## How does it work?

This video shows Srna. She is one of the children who have received the Cboard Communicator thanks to UNICEF's ["For every child, a voice"](https://www.unicef.org/innovation/stories/giving-every-child-voice-aac-technology) project.

<a href="https://youtu.be/wqLauXnyLhY"><img src="https://img.youtube.com/vi/wqLauXnyLhY/0.jpg" alt="Real Look Autism Episode 8" width="480" height="360"></a>

## Translations

The app supports 40 languages.
Languages were machine translated and require proofreading: if you want to help proofread, please use our translation management platform: https://crowdin.com/project/cboard

**You do not need to be a programmer!**

Translations play a major role in this project and they contribute a lot for the inclusion of children, specially in non developed countries. Please consider collaborating with us!

### Translations for developers

To add support to a new language, [follow this guide](https://github.com/cboard-org/cboard/wiki/How-to-Add-a-New-Language).

#### Pulling translations from CrowdIn

In order to pull the latest translations from CrowdIn into the codebase, you can run `yarn translations:pull`. This will update all language files such as `en.json` as well as the central `cboard.json` file. Please note that this requires the CrowdIn API key to be available in the `.private` config file. Refer to [Secrets Management](#secrets-management). After the script completes, changes to the translation files will need to be committed to the repo by the usual process.

## Getting Started

### `yarn start`

Runs the app in development mode.<br>
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br>
You will see the build errors and lint warnings in the console.

### `yarn test`

Runs the test watcher in an interactive mode.<br>
By default, runs tests related to files changed since the last commit.

[Read more about testing.](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#running-tests)

### `yarn build`

Builds the app for production to the `build` folder.<br>
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br>
By default, it also [includes a service worker](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#making-a-progressive-web-app) so that Cboard loads from local cache on future visits.

Cboard is ready to be deployed.

### `yarn build-cordova-debug`

Use this to produce non-minified build for use in debugging within Cordova. It uses `craco` & `craco.config` to customize webpack operation without ejecting react.

See [CBoard](https://github.com/nous-/cboard) repo for packaging this CBoard application within Cordova.

## Docker getting started

### `make image`

Creates a Docker image with cboard built for production. The image is tagged as cboard:latest.

### `make run`

Runs the cboard:latest Docker image on port 5000.

## Cordova Setup

Cboard can be packaged as a native mobile application using Apache Cordova. This allows you to build and deploy Cboard to Android, iOS, and Electron (Windows) platforms.

### Prerequisites

Before setting up Cordova, ensure you have:
- Node.js and npm installed
- Cordova CLI: `npm install -g cordova`
- Platform-specific requirements:
  - **Android**: Android Studio, Java JDK 8+
  - **iOS**: macOS with Xcode
  - **Electron**: No additional requirements

### Option 1: Using the Separate Cordova Repository (Recommended)

The easiest way to build Cboard with Cordova is to use the dedicated [ccboard repository](https://github.com/cboard-org/ccboard):

1. Clone the ccboard repository:
   ```bash
   git clone https://github.com/cboard-org/ccboard.git
   cd ccboard
   ```

2. Follow the setup instructions in the ccboard README for your target platform.

### Option 2: Setting up Cordova in this Repository

If you want to set up Cordova directly in this repository:

#### 1. Prepare the Cboard React App

Before building with Cordova, you need to modify some files:

1. In `package.json`, change the homepage:
   ```json
   "homepage": "."
   ```

2. In `public/index.html`, add the Cordova script tag inside the `<head>` section:
   ```html
   <script src="cordova.js"></script>
   ```

#### 2. Build for Cordova

Use the special Cordova build command that produces a non-minified build suitable for debugging:

```bash
yarn build-cordova-debug
```

For production builds:
```bash
yarn build
```

#### 3. Create Cordova Project Structure

```bash
# Create www directory for Cordova
mkdir -p www

# Copy the build output to www
cp -r build/* www/

# Initialize Cordova project
cordova create cboard-cordova com.cboard.app Cboard
cd cboard-cordova

# Copy the www folder
cp -r ../www/* www/
```

#### 4. Add Platforms

```bash
# For Android
cordova platform add android

# For iOS
cordova platform add ios

# For Electron (Windows)
cordova platform add electron@2.0.0
```

#### 5. Add Required Plugins

```bash
# Speech synthesis plugin for text-to-speech
cordova plugin add phonegap-plugin-speech-synthesis

# Other plugins as needed
cordova plugin add cordova-plugin-file
cordova plugin add cordova-plugin-device
```

#### 6. Build and Run

**Android:**
```bash
# Run on emulator
cordova run android --emulator

# Build for release
cordova build android --release
```

**iOS:**
```bash
# Open in Xcode
cordova prepare ios
# Then open platforms/ios/*.xcworkspace in Xcode

# Or build directly
cordova build ios
```

**Electron:**
```bash
# Build for release
cordova build electron --release

# Build with dev tools
cordova build electron --debug
```

### Platform-Specific Notes

#### Android
- For release builds, you'll need to sign the APK. See the [Android documentation](https://developer.android.com/studio/publish/app-signing) for details.
- Debug output can be viewed in Chrome DevTools at `chrome://inspect`

#### iOS
- Requires macOS with Xcode installed
- You'll need an Apple Developer account for device testing and distribution
- Configure signing certificates in Xcode

#### Electron
- The `settings.json` file in the root contains BrowserWindow configurations
- Suitable for Windows desktop deployment

### Troubleshooting

1. **Build errors**: Ensure all platform requirements are installed
2. **White screen**: Check that `cordova.js` is included in `index.html`
3. **API issues**: Some browser APIs may behave differently in Cordova. The codebase includes `cordova-util.js` to handle platform differences

For more detailed instructions and platform-specific configurations, refer to the [ccboard repository](https://github.com/cboard-org/ccboard).

## Secrets Management

Some external services have APIs we need to access, and these require API keys. To prevent open disclosure of these keys in the public repository, while still tracking them with the code, we encrypt some secrets into a GPG file. These files are `env/local-private.gpg` and `env/prod-private.gpg`.

In order to access the secrets, you must request the `ENCRYPTION_KEY` from @shaycojo and then run the decrypt script: `ENCRYPTION_KEY={key-goes-here} yarn decrypt:local` (or `prod`), which will create the file `.private/local.js` with the secrets in plain text where the scripts can access them. **The files in `.private` should never be committed to the repository.**

If you need to add or change a secret, make the change to the `.private/local.js` file, and then run the encryption script: `ENCRYPTION_KEY={key-goes-here} yarn encrypt:local` (or `prod`).

_Note: These keys/secrets are *not* required to run or develop Cboard._ They are used with scripts by some team members.

## About the Cboard community

[![Cauldron dashboard and metrics for the Cboard project community](https://cauldron.io/project/1683/stats.svg)](https://cauldron.io/project/1683 "Cauldron dashboard and metrics for the Cboard project community")

## Thanks

### Symbols sources

<img src="https://mulberrysymbols.org/assets/examples/hello.svg" href="https://mulberrysymbols.org" alt="Mulberry" width="40" height="40"> [Mulberry](https://mulberrysymbols.org/)

<img src="https://static.arasaac.org/images/arasaac-logo.svg" href="https://mulberrysymbols.org" alt="ARASAAC" width="40" height="40"> [ARASAAC](http://www.arasaac.org/)

<img src="https://globalsymbols.com/assets/logo-with-text-5c57659e34824e7b2907a36895745f9e39e7f1c015ea77d6968eb75a52c8389f.svg" href="https://globalsymbols.com" alt="Global Symbols" width="40" height="40"> [Global Symbols](https://globalsymbols.com/)

### Translation

<img src="https://support.crowdin.com/assets/logos/crowdin-symbol.png" href="https://crowdin.com/" alt="Crowdin" width="40" height="40">[  Crowdin](https://crowdin.com/) - for providing the localization management platform.

### Testing platform

<img src="https://avatars2.githubusercontent.com/u/1119453?s=200&v=4" href="https://www.browserstack.com/" alt="Browserstack" width="40" height="40">[  Browserstack](https://www.browserstack.com/) - for providing the automation infrastructure for testing.

### Development

<img src="./public/images/sponsers/css-tricks.svg" alt="CSS-Tricks" width="120" height="39">[  CSS Tricks](https://css-tricks.com) - for providing feedback and support from the early stage.

## Contributors

This project exists thanks to all the people who contribute. [[Contribute](CONTRIBUTING.md)].
<a href="https://github.com/cboard-org/cboard/graphs/contributors"><img src="https://opencollective.com/cboard/contributors.svg?width=890&button=false" /></a>

## Backers

Thank you to all our backers! 🙏 [[Become a backer](https://opencollective.com/cboard#backer)]

<a href="https://opencollective.com/cboard#backers" target="_blank"><img src="https://opencollective.com/cboard/backers.svg?width=890"></a>

## Sponsors

Support this project by becoming a sponsor. Your logo will show up here with a link to your website. [[Become a sponsor](https://opencollective.com/cboard#sponsor)]

<a href="https://opencollective.com/cboard/sponsor/0/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/1/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/2/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/3/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/4/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/5/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/6/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/7/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/8/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/cboard/sponsor/9/website" target="_blank"><img src="https://opencollective.com/cboard/sponsor/9/avatar.svg"></a>


## :memo: Legal & licenses

Copyright © 2017-2024, Assistive Technology LLC & Cboard contributors.

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License version 3 as published by the Free Software Foundation.

* Code - [GPLv3](https://github.com/cboard-org/cboard/blob/master/LICENSE.txt)
* Mulberry Symbols - [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
* ARASAAC Symbols - [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)
