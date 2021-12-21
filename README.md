# Flatfile Vue.js Component - @flatfile/vuejs

[![npm](https://img.shields.io/npm/v/@flatfile/vuejs.svg?label=npm%20version&color=2EBF6A&style=for-the-badge)](https://www.npmjs.com/@flatfile/vuejs)
[![Minzipped Size](https://img.shields.io/bundlephobia/minzip/@flatfile/vuejs?color=794cff&style=for-the-badge)](https://bundlephobia.com/result?p=@flatfile/vuejs)
[![NPM Downloads](https://img.shields.io/npm/dw/@flatfile/vuejs.svg?color=8c66ff&style=for-the-badge)](https://www.npmjs.com/@flatfile/vuejs)
[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg?style=for-the-badge&color=794cff)](/LICENSE)
--

## Getting Started with Flatfile & Vue.js

We've made it really simple for you to get started with Flatfile with our new Flatfile Component. Here's what you'll need to know to get started.

First, install the dependency via npm:

```bash
npm install @flatfile/vuejs
```

This will give you access to the `<flatfile-button />` component as well as the same basic functionality as our Adapter.

Simply add the import to a component where you want to include the Flatfile vuejs adapter via

```html
import { FlatfileButton } from '@flatfile/vuejs';

export default {
  name: 'DemoComponent',
  components: {
    FlatfileButton,
  },
  // ...
}
```

Now in your application simply utilize this new `<flatfile-button>` component, but make sure to pass in the 3 required props, (and/or any optional ones you may need for your application).

#### The 3 required properties are:

- `:licenseKey` (String) [ get this from your Flatfile account ]
- `:customer` (Object)
- `:settings` (Object)



```html
<flatfile-button 
  :licenseKey="licenseKey"
  :customer="customer"
  :settings="settings"
>
  Upload to Flatfile!
</flatfile-button>

<script>
import { FlatfileButton } from '@flatfile/vuejs';

export default {
  name: 'App',
  components: {
    FlatfileButton,
  },
  data: () => ({
    licenseKey: 'Your_License_Key_Here',
    customer: {
      userId: '12345',
    },
    settings: {
      type: 'test import',
      fields: [
        { label: 'Name', key: 'name' },
        { label: 'Email', key: 'email' }
      ]
    },
  })
}
</script>
```

---

Here's an example passing down many of the other optional parameters/methods available to the adapter.

```html
<flatfile-button 
  :licenseKey="licenseKey"
  :customer="customer"
  :settings="settings"
  :fieldHooks="fieldHooks"
  :stepHooks="stepHooks"
  :setLang="setLang"
  :onData="onData"
  :onRecordInit="onRecordInit"
  :onRecordChange="onRecordChange"
  :onRecordHook="onRecordHook"
  :onCancel="onCancel" 
  class="ff-button"
>
  Upload to Flatfile!
</flatfile-button>

<script>
import { FlatfileButton } from '@flatfile/vuejs';

export default {
  name: 'App',
  components: {
    FlatfileButton,
  },
  data: () => ({
    licenseKey: 'Your_License_Key_Here',
    customer: {
      userId: '12345',
    },
    settings: {
      type: 'test import',
      fields: [
        { label: 'Name', key: 'name' },
        { label: 'Email', key: 'email' }
      ]
    },
    setLang: "", // language
    stepHooks: {
      review: (payload, importer) => {
        // do something

        // if you want to add VirtualFields, use the importer being passed into this Function
        importer.addVirtualField({
          // ...
        })
      }
    }
    fieldHooks: {
      email: (values) => {
        console.log({values});
        return values.map(([item, index]) => [
        {
          value: item + '@',
          info: [{ message: 'added @ after the email', level: 'warning' }],
        },
        index,
      ]);
      }
    }
  }),
  methods: {
    onData: function (results) {
      let errorState = false;
      console.log('onData results{}')
      console.log(results);
      console.log(results.$data)

      return new Promise((resolve, reject) => {
        setTimeout(() => {
          if (errorState) {
            reject('rejected - this text is controlled by the end-user');
            errorState = false;
          } else {
            resolve(
              'Flatfile upload successful - this text is controlled by the end-user'
            );
          }
        }, 3000);
      });
    },

    onRecordInit: function (
      record,
      /*index*/
    ) {
      console.log('onRecordInit')
      return {
        email: {
          value: record.email + '@',
          info: [{ message: 'added @ on init', level: 'info' }],
        },
      };
    },

    onRecordChange: function (
      record,
      /*index*/
    ) {
      console.log('onRecordChange')
      return {
        email: {
          value: record.email + '#',
          info: [{ message: 'added # on change', level: 'warning' }],
        },
      };
    },

    onRecordHook: function(record) {
      // do something
    },

    /*
    * @Output() handlers
    */
    onCancel: function () {
      console.log('canceled!');
    }
  }
}
</script>
```

---

## Codesandbox Demo

Try it for yourself in the [CodesandBox here](https://codesandbox.io/s/nostalgic-johnson-2wgqn?file=/src/App.vue).

---
