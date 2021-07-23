# detox-expo-helpers

Utilities for using [detox](http://github.com/wix/detox) in your Expo/Create React Native App apps.

## Use it

### Install it

```
yarn add fschoenfeldt/detox-expo-helpers -D
 # or
npm i fschoenfeldt/detox-expo-helpers --save-dev
```

Also, declare `expo-detox-hooks` in your project's `package.json`.

### Set up detox on your project

Follow the steps in the detox [Getting Started](https://github.com/wix/detox/blob/master/docs/Introduction.GettingStarted.md) guide.

### Download the Expo app to some directory in your project and configure in package.json

You can download the Expo app from the [Expo Tools page](https://expo.io/tools). See an [example package.json configuration](https://github.com/expo/with-detox-tests/blob/033020b165452d641f512a9b1a8a291632ce8e8f/package.json#L21-L29)

### Use detox-expo-helpers in your app

> ⚠️ Important: This fork disables `deviceSynchronization` because it was the only way we could figure out getting Detox@18 to work. When launching the App with `reloadApp`, you have to wait for the first element to ensure the app is properly booted. (See Example)

All you really need to use is `reloadApp`, like so:

```js
const { reloadApp } = require('detox-expo-helpers');

describe(`Example Tests`, () => {
    beforeAll(async () => {
      await reloadApp({
        // Optionally, you can also set some parameters when loading the app
        // see https://github.com/wix/Detox/blob/master/docs/APIRef.DeviceObjectAPI.md
        args: {
          disableTouchIndicators: true,
          delete: true,
          languageAndLocale: {
            language: "de-DE",
            locale: "de-DE",
          },
        },
        launchArgs: {
          // see https://github.com/wix/Detox/blob/master/docs/APIRef.LaunchArgs.md
        }
      });
      // ⚠️ Important! Wait for first element to ensure that app is booted
      const confirmButton = $('confirm_button');
      await waitFor(confirmButton).toBeVisible().withTimeout(10000);
    });

    // Your tests
    it('can do this and that', async () => { ... }
}
```

## Example app

You can find the latest example app here, which is working with Expo@42 and Detox@18: https://github.com/fschoenfeldt/clean-expo-detox-testing

Original Example App:
Try out an example app with this already configured at https://github.com/expo/with-detox-tests
