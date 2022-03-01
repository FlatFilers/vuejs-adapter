<template>
  <div>
    <button @click="launch">
      <slot></slot>
    </button>
  </div>
</template>

<script>
import FlatfileImporter from "@flatfile/adapter";

export default {
  name: "flatfile-button",
  props: {
    settings: {
      type: Object,
      validator: function (value) {
        return value && value.type && value.fields;
      },
    },
    customer: {
      type: Object,
      validator: function (value) {
        return value && value.userId;
      },
    },
    licenseKey: {
      type: String,
      validator: function (value) {
        return value && value.length;
      },
    },
    fieldHooks: Object,
    source: [String, Array],
    onCancel: Function,
    onInteractionEvent: Function,
    onBeforeFetch: Function,
    onData: Function,
    onRecordChange: Function,
    onRecordInit: Function,
    onRecordHook: Function,
    setLang: String,
    stepHooks: Object,
    render: Function,
    preload: {
      default: true,
      type: Boolean,
    },
    mountUrl: String,
  },
  data: () => ({
    flatfileImporter: null,
    loaded: false,
    importerLoaded: true,
  }),
  mounted() {
    if (this.preload) {
      this.loadImporter();
    }
  },
  methods: {
    loadImporter: function () {
      if (this.flatfileImporter) {
        return;
      }

      if (this.mountUrl) {
        FlatfileImporter.setMountUrl(this.mountUrl);
      }
      const tempImporter = new FlatfileImporter(
        this.licenseKey,
        this.settings,
        this.customer
      );

      if (this.fieldHooks) {
        for (const key in this.fieldHooks) {
          tempImporter.registerFieldHook(key, this.fieldHooks[key]);
        }
      }
      if (this.stepHooks) {
        for (const key in this.stepHooks) {
          tempImporter.registerStepHook(key, (payload) => this.stepHooks[key](payload, tempImporter))
        }
      }
      if (this.onBeforeFetch) {
        tempImporter.registerBeforeFetchCallback(this.onBeforeFetch);
      }
      if (this.onInteractionEvent) {
        tempImporter.registerInteractionEventCallback(this.onInteractionEvent);
      }
      if (this.onRecordChange || this.onRecordInit) {
        tempImporter.registerRecordHook(
          (
            record, // : ScalarDictionaryWithCustom,
            index, // : number,
            eventType // : 'init' | 'change'
          ) => {
            if (eventType === "init" && this.onRecordInit) {
              return this.onRecordInit(record, index);
            }
            if (eventType === "change" && this.onRecordChange) {
              return this.onRecordChange(record, index);
            }
          }
        );
      }
      if (this.onRecordHook) {
        tempImporter.registerRecordHook(this.onRecordHook);
      }
      if (this.setLang) {
        tempImporter.setLanguage(this.setLang);
      }

      this.flatfileImporter = tempImporter;
      this.loaded = true;
    },

    dataHandler: function (results) {
      this.flatfileImporter.displayLoader();
      this.onData?.(results).then(
        (optionalMessage) =>
          optionalMessage !== null
            ? this.flatfileImporter.current?.displaySuccess(optionalMessage || undefined)
            : this.flatfileImporter.current?.close(),
        (error /*: Error | string*/) =>
          this.flatfileImporter.current
            ?.requestCorrectionsFromUser(error instanceof Error ? error.message : error)
            .then(this.dataHandler, () => this.onCancel?.())
      );
    },

    launch: function () {
      this.validateInputs();

      var dataHandler = (results) => {
        this.flatfileImporter.displayLoader();

        if (this.onData) {
          this.onData(results).then(
            (optionalMessage) => {
              this.flatfileImporter.displaySuccess(optionalMessage || "Success!");
            },
            (error) => {
              console.error(`Flatfile Error : ${error}`);
              this.flatfileImporter
                .requestCorrectionsFromUser(error ? error.message : error)
                .then(dataHandler, () => this.onCancel?.());
            }
          );
        } else {
          this.flatfileImporter.displaySuccess("Success!");
        }
      };

      if (!this.flatfileImporter) {
        if (this.preload) {
          return;
        }
        this.loadImporter();
      }

      var loadOptions = this.source ? { source: this.source } : undefined;

      this.flatfileImporter
        .requestDataFromUser(loadOptions)
        .then(dataHandler, () => this.onCancel?.());
    },

    validateInputs: function () {
      if (!this.licenseKey) {
        console.error("[Error] Flatfile VueJS Adapter - licenseKey not provided!");
        this.isImporterLoaded = false;
      }
      if (!this.customer?.userId) {
        console.error("[Error] Flatfile VueJS Adapter - customer userId not provided!");
        this.isImporterLoaded = false;
      }
      if (!this.settings?.type || !this.settings?.fields) {
        console.error(
          "[Error] Flatfile VueJS Adapter - settings { type: String, fields: Array } not provided!"
        );
        this.isImporterLoaded = false;
      }
    },
  },
};
</script>

<style></style>
