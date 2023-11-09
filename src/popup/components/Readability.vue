<template>
  <b-list-group-item>
    <b-form-checkbox v-model="readability" switch @change="onChange">
      {{ t('readability') }}
    </b-form-checkbox>
  </b-list-group-item>
</template>

<script lang="ts">
import Vue from 'vue';
import { ToggleReadabilityForTab } from '@stylebot/types';

export default Vue.extend({
  name: 'Readability',
  props: {
    tab: {
      type: Object,
      required: true,
    },

    initialReadability: Boolean,
  },

  data(): {
    readability: boolean;
  } {
    return {
      readability: this.initialReadability,
    };
  },

  // it's watching the prop but the prop is not being updated?
  watch: {
    initialReadability(newVal: boolean): void {
      this.readability = newVal;
    },
  },

  methods: {
    onChange(): void {
      // ### alternative: works but it seems to be persisting readability state across all tabs, instead of for one?
      // if (this.tab.id) {
      //   const message: ToggleReadabilityForTab = {
      //     name: 'ToggleReadabilityForTab',
      //   };

      //   console.log('hi readability 1')  // ✅
      //   console.log(this.tab.id)
      //   chrome.tabs.sendMessage(this.tab.id, message);
      // }
      // chrome.tabs.query({ active: true, currentWindow: true }, ([tab]) => {
      //   if (tab.id) {
      //     console.log(tab.id)
      //   }
      // });
      // ### original
      chrome.tabs.query({ active: true, currentWindow: true }, ([tab]) => {
        if (tab.id) {
          const message: ToggleReadabilityForTab = {
            name: 'ToggleReadabilityForTab',
          };
          console.log('hi readability 1')  // ✅
          console.log(tab.id)
          chrome.tabs.sendMessage(tab.id, message);
        }
      });
    },
  },
});
</script>
