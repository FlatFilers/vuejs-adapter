# Changelog

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
