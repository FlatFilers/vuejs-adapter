<template>
  <div>
    <button @click="launch">
      <slot></slot>
    </button>
  </div>
</template>

<script>
import { flatfileImporter } from "@flatfile/sdk";

export default {
  name: "flatfile-button",
  props: {
    token: {
      type: String,
      validator: function (value) {
        return value && value.length;
      },
    },
    mountUrl: String,
    apiUrl: String,
    onInit: Function,
    onUpload: Function,
    onLaunch: Function,
    onClose: Function,
    onComplete: Function,
    onError: Function,
  },
  data: () => ({
    flatfileImporter: null,
    loaded: false,
    importerLoaded: true,
  }),
  mounted() {
    this.loadImporter();
  },
  methods: {
    loadImporter: function () {
      if (this.flatfileImporter) {
        return;
      }

      const tempImporter = flatfileImporter(this.token, {
        ...(this.mountUrl ? { mountUrl: this.mountUrl} : {}),
        ...(this.apiUrl ? { apiUrl: this.apiUrl } : {}),
      });

      if (typeof this.onInit === 'function') {
        tempImporter.on('init', this.onInit);
      }
      if (typeof this.onUpload === 'function') {
        tempImporter.on('upload', this.onUpload);
      }
      if (typeof this.onLaunch === 'function') {
        tempImporter.on('launch', this.onLaunch);
      }
      if (typeof this.onClose === 'function') {
        tempImporter.on('close', this.onClose);
      }
      if (typeof this.onComplete === 'function') {
        tempImporter.on('complete', this.onComplete);
      }

      this.flatfileImporter = tempImporter;
      this.loaded = true;
    },
    launch: function () {
      this.validateInputs();

      this.flatfileImporter.launch().catch((e) => {
        if (typeof this.onError === 'function') {
          this.onError({ error: e });
        }
      });
    },
    validateInputs: function () {
      if (!this.token) {
        console.error("[Error] Flatfile VueJS Adapter - token not provided!");
        this.isImporterLoaded = false;
      }
    },
  },
};
</script>

<style></style>
