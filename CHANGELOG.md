# Changelog

## 3.0.0

BREAKING CHANGES:
Note that the latest version of `@flatfile/vuejs` 3+ uses the new `@flatfile/sdk` underneath which changes the API surface of interacting with the flatfile adapter entirely.

[Read more about these changes here](https://flatfile.com/docs/implementing-embeds/)

There is now only 1 required input, and that is `:token` (which you must receive from your backend), more information in the link above.

[Read more about generating a Token here](https://flatfile.com/docs/sdk/)

#### New methods:

The lifecycle methods have changed a bit, and below are all the available methods:

`onInit | onUpload | onLaunch | onClose | onComplete | onError`

2 new properties can be passed down as well are: `mountUrl` and `apiUrl`

Example of everything in action.
(More info in the README also).

```html
<flatfile-button 
  :token="token"
  :onInit="onInit"
  :onUpload="onUpload"
  :onLaunch="onLaunch"
  :onClose="onClose"
  :onComplete="onComplete"
  :onError="onError" 
  :mountUrl="mountUrl"
  :apiUrl="apiUrl"
>
  Upload to Flatfile!
</flatfile-button>
```

## 0.3.6

Fix `setLang` bug `tempImporter.setLang` is not a function.

## 0.3.5

Fix bug with `setLang` and `onRecordHook` not functioning properly.

## 0.3.4

New additions & more information / example use-case in README.

1. `stepHooks` & `Virtual Fields`

We can now create StepHooks & VirtualFields easily by passing down the props to the flatfile-button

```
stepHooks: {
  review: (payload, importer) => {
    // do something

    // if you want to add VirtualFields, use the importer being passed into this Function
    importer.addVirtualField({
      // ...
    })
  }
}
```

2. `setLang`

You can now `setLang` by simply passing down the attribute to the flatfile-button. 

3. `onRecordHook`

You can now `registerRecordHook` by passing down a callback Function to `onRecordHook` prop (within flatfile-button).

## 0.3.2

Bug fix where default text from library wasn't being removed.

## 0.3.0

🚀🚀 Production ready 🚀🚀

- Updated to use the latest _base_ `@flatfile/adapter`. No breaking changes, etc.

## 0.2.0

🚀 Initial release
