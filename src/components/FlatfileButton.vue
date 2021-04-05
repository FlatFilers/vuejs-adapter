<template>
  <div>
    <button @click="launch">
      Start FF Button
    </button>
    <slot></slot>
  </div>
</template>

<script>
  import FlatfileImporter from '@flatfile/adapter';

  export default {
    name: 'flatfile-button',
    props: {
      settings: {
        type: Object,
        validator: function (value) {
          return value && value.type && value.fields;
        }
      },
      customer: {
        type: Object,
        validator: function (value) {
          return value && value.userId;
        }
      },
      licenseKey: {
        type: String,
        validator: function (value) {
          return value && value.length;
        }
      },
      fieldHooks: Function,
      source: [String, Array],
      onCancel: Function,
      onInteractionEvent: Function,
      onBeforeFetch: Function,
      onData: Function,
      onRecordChange: Function,
      onRecordInit: Function,
      render: Function,
      preload: Boolean,
      mountUrl: String,
    },
    data: () => ({
      importerRef: null,
      loaded: false,
      preload: true
    }),
    // created() {
    // },
    mounted() {
      if (this.$data.preload) {
        this.loadImporter();
      }
    },
    methods: {
      loadImporter: function () {
        if (this.$data.importerRef.current) {
          return;
        }
        if (this.mountUrl) {
          FlatfileImporter.setMountUrl(this.mountUrl);
        }
        const tempImporter = new FlatfileImporter(this.licenseKey, this.settings, this.customer);
        if (this.fieldHooks) {
          for (const key in this.fieldHooks) {
            tempImporter.registerFieldHook(key, this.fieldHooks[key]);
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
              record,    // : ScalarDictionaryWithCustom,
              index,     // : number,
              eventType, // : 'init' | 'change'
            ) => {
              if (eventType === 'init' && this.onRecordInit) {
                return this.onRecordInit(record, index);
              }
              if (eventType === 'change' && this.onRecordChange) {
                return this.onRecordChange(record, index);
              }
            }
          );
        }
        this.importerRef.current = tempImporter;
        this.$data.loaded = true;
      },
      // const dataHandler = (results/*: FlatfileResults*/) => {
      //     this.$data.importerRef.current?.displayLoader();
      //       onData?.(results).then(
      //         (optionalMessage) =>
      //           optionalMessage !== null
      //             ? this.$data.importerRef.current?.displaySuccess(optionalMessage || undefined)
      //             : this.$data.importerRef.current?.close(),
      //         (error/*: Error | string*/) =>
      //           this.$data.importerRef.current
      //             ?.requestCorrectionsFromUser(
      //               error instanceof Error ? error.message : error
      //             )
      //             .then(dataHandler, () => this.onCancel?.())
      //       );
      //     });
      // ,[onData, onCancel]
      launch: function () {
        console.log('launch hit!');

        if (!this.$data.importerRef.current) {
          if (this.preload) {
            return;
          }
          this.loadImporter();
        }
        this.$data.importerRef.current
          ?.requestDataFromUser({ source: this.source })
          .then(this.dataHandler, () => this.onCancel?.());
        }
    }
  };
</script>

<style>
</style>