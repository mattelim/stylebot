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
    initialReadability: Boolean,
  },

  data(): {
    readability: boolean;
  } {
    return {
      readability: this.initialReadability,
    };
  },

  // 🤔 it's watching the prop but the prop is not being updated?
  watch: {
    initialReadability(newVal: boolean): void {
      this.readability = newVal;
    },
  },

  methods: {
    onChange(): void {
      chrome.tabs.query({ active: true, currentWindow: true }, ([tab]) => {
        if (tab.id) {
          const message: ToggleReadabilityForTab = {
            name: 'ToggleReadabilityForTab',
          };
          chrome.tabs.sendMessage(tab.id, message);
        }
      });
    },
  },
});
</script>
