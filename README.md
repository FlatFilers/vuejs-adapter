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
npm install @flatfile/vuejs@3
```

This will give you access to the `<flatfile-button />` component as well as the same basic functionality as our new SDK.

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

Now in your application simply utilize this new `<flatfile-button>` component, but make sure to pass in the 1 required prop, (and/or any optional ones you may need for your application).

### The 1 required property is

- `:token` (String) [ which you need to get from your backend ]

[Read more here](https://flatfile.com/docs/implementing-embeds/) on how to implement a secure token.

```html
<flatfile-button :token="token">
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
    token: 'Your_Token_You_Received_From_Your_Backend',    
  })
}
</script>
```

---

Here's an example passing down many of the other optional parameters/methods available to the adapter.

```html
<flatfile-button 
  :token="token"
  :onInit="onInit"
  :onUpload="onUpload"
  :onLaunch="onLaunch"
  :onClose="onClose"
  :onComplete="onComplete"
  :onError="onError" 
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
    token: 'Your_Token_You_Received_From_Your_Backend',
  }),
  methods: {
    onInit: function (data) {
      console.log('onInit')
      console.log(data)
    },
    onUpload: function (data) {
      console.log('onUpload')
      console.log('data')
    },
    onLaunch: function (data) {
      console.log('onLaunch')
      console.log('data')
    },
    onClose: function (data) {
      console.log('onClose')
      console.log('data')
    },
    onComplete: function (data) {
      console.log('onComplete')
      console.log('data')
    },
    onError: function (error) {
      console.log('onError')
      console.log(error)
    },
  }
}
</script>
```

---

#### Additional options

You can also pass down `mountUrl` and `apiUrl` to the `<flatfile-button>`.

```html
<flatfile-button 
  :token="token"
  :mountUrl="mountUrl"
  :apiUrl="apiUrl"
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
    token: 'Your_Token_You_Received_From_Your_Backend',
    mountUrl: 'mountUrl',
    apiUrl: '',
  }),
  // ... everything else
}
</script>
```

## Publishing

Update changelog (if needed), update package.json version (semver), add any updates needed for README (if needed), then run the following scripts:

```bash
npm run build:lib
npm publish
```