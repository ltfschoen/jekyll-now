---
layout: post
title: React Native
---

# Table of Contents
  * [Chapter 1 - Installation](#chapter-1)
  * [Chapter 2 - Troubleshooting](#chapter-2)
  * [Chapter 3 - ES6](#chapter-3)
  * [Chapter 4 - Debugging](#chapter-4)
  * [Chapter 5 - Code](#chapter-5)
  * [Chapter 6 - Testing](#chapter-6)
  * [Chapter 100 - Other](#chapter-100)

## Chapter 1 - Installation CLI <a id="chapter-1"></a>

* Setup guide https://github.com/start-react/native-starter-kit/blob/master/.gitignore

* Install Dependencies: Node.js, Watchman, React Native CLI, Yarn
    ```
    brew install node

    # install NVM https://github.com/creationix/nvm
    # install latest stable Node (includes NPM)
    nvm install v8.0.0; nvm use v8.0.0;
    which node
    # update PATH so can run `react-native` command
    echo 'export PATH="/usr/local/share/npm/bin:$PATH"' >> ~/.bash_profile
    source ~/.bash_profile
    nvm list

    brew install watchman; brew upgrade watchman; which watchman

    brew install yarn; brew upgrade yarn;

    yarn add global react-native-cli
    yarn add react-native

    npm install --save-dev jest-cli
    ```
* Debug in IDE
    * Press CMD + , and then Languages & Frameworks > Node and NPM > Choose v7.9.0 directory
    * Follow this guide: https://blog.jetbrains.com/webstorm/2016/12/developing-mobile-apps-with-react-native-in-webstorm/
* Install Xcode and Xcode Command Line Tools
    * Xcode > Preferences > Command Line Tools
* Create New React Native project for iOS using Yarn
    ```
    react-native init --veReactNativeTest
    ```

    ```
    To run your app on iOS:
       cd /Users/Ls/code/blockchain/clones/peer.ai/PeerAI
       react-native run-ios
       - or -
       Open ios/PeerAI.xcodeproj in Xcode
       Hit the Run button
    To run your app on Android:
       cd /Users/Ls/code/blockchain/clones/peer.ai/PeerAI
       Have an Android emulator running (quickest way to get started), or a device connected
       react-native run-android
    ```
* Add command to .bashrc, so can run with just `rni` and avoids errors
    ```
    echo "alias rni=\"kill $(lsof -t -i:8081); rm -rf ios/build/; react-native run-ios\"" >> ~/.bashrc
    source ~/.bashrc
    ```
* Run on iOS (using my Alias)
    * Turn off host hardware keyboard integration to show keypad
    Hardware > Keyboard > Connect Hardware Keyboard
    OR SHIFT + CMD + K
    ```
    cd ReactNativeTest
    yarn install
    npm install
    rni
    ```
    * Alternative
        * Open in Xcode and Run: ios/ReactNativeTest.xcodeproj
* Setup Android `android avd`
* Run on Android. Open Android Emulator or with device connected
    ```
    cd ReactNativeTest
    yarn install
    npm install

    # Check listed devices
    adb devices

    SEE PEER.AI README FOR DETAILS
    https://github.com/facebook/react-native/issues/3091


    react-native run-android
    ```
* Modify `index.ios.js`. Press CMD + R (Reload)

* Git add/commit, gitignore
    * Git init `git init`
    * Add Gitignore
        ```
        touch .gitignore
        ```
        * Copy/paste from
            * https://github.com/facebook/react-native/blob/master/.gitignore
            * https://github.com/start-react/native-starter-kit/blob/master/.gitignore
    * Commit
        ```
        git add --all .
        git commit
        git remote add origin https://github.com/ltfschoen/ReactNativeTest
        git checkout master
        git pull --rebase origin master
        git push origin master
        ```

## Chapter 2 - Troubleshooting <a id="chapter-2"></a>

* Troubleshooting
    * http://facebook.github.io/react-native/releases/0.19/docs/troubleshooting.html
    * Port in use (red screen)
        ```
        kill $(lsof -t -i:8081)
        ```
        OR
        ```
        sudo lsof -n -i4TCP:8081 | grep LISTEN
        kill -9 <cma process id>
        ```
    * No bundle URL present
        * `rm -rf ios/build/`

## Chapter 3 - ES6 <a id="chapter-3"></a>

* ES2015
    * https://babeljs.io/learn-es2015/

### Chapter 4 - Debugging <a id="chapter-4"></a>

* iOS Simulator
    * Cmd+R to reload
    * Cmd+D or shake for dev menu

* Debugging
    * Chrome localhost:8081/debugger-ui

## Chapter 5 - Code <a id="chapter-5"></a>

* Components
    * Component registered with `AppRegistry.registerComponent` tells React Native the root for the whole app
    * Props `props` customise Components with parameters when created
        * `Image` Component has `prop` named `source`

* HTTP
    * Fetch

* Images
    * Load icons from local device
    * Load large images from AWS S3

* Fonts
    * https://github.com/oblador/react-native-vector-icons
    * `react-native link` does it all automatically

## Chapter 6 - Code <a id="chapter-6"></a>

* Jest
    * Run with `npm test` OR `jest`


## Chapter 100 - Other <a id="chapter-100"></a>

* Tricks
    * Read release notes of breaking changes before updating project to new versions and ensure third-party libraries
    are up to date with latest release
    * Dependencies added to component.
    * Convention is to name file same as component name
    * Stateless components (pure functions) do not have any state
    and not support any lifecycle methods
    * Components have `style` property using Stylesheet API providing performance optimisations
    * Be careful setting `backgroundColor` of Text as `transparent`
    since if update component frequently may get performance issues if text is long
    since rendering engine has to calculate pixels around each letter
    * Snapshots using Jest
    * Important to explicitly require every image (i.e. no ternary operators
    for choosing icon to show) since when distributing the images are added automatically to bundle
    * Dismiss Keyboard when using Numeric, ensure to wrap `TouchableWithoutFeedback` around components
    so when you click them the keypad disappears
    http://stackoverflow.com/questions/29685421/react-native-hide-keyboard
    * Create Custom Buttons in a Utility
    * p194 Call method to render elements (i.e. `this.renderButton({name: ..`)
    * Different device orientations hide/show elements by import Device Orientation detection library,
    use for animations knowing where to start/end
    * Multiple animations

* ES6
    * https://gist.github.com/johnwook/09500f00c2e9a341ab4e37ae6cc7cce9
    * https://babeljs.io/blog/
    * http://moduscreate.com/using-es2016-decorators-in-react-native/

* IDE
    * Decoide decoide.org
    * Atom Nuclide https://nuclide.io/

* Photoshop
    * Video to GIF https://graphicdesign.stackexchange.com/questions/46656/how-to-convert-a-video-file-into-an-animated-gif-in-photoshop