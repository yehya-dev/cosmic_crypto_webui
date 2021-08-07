<template>
  <Card>
    <div class="flex justify-between">
      <h2 class="font-thin text-2xl">New Trailing Stoploss</h2>
      <button @click="toggleTSLelement">
        <svg
          :class="show_tsl ? 'rotate-180 bg-red-600' : 'bg-green-600'"
          class="w-8 h-8 rounded-full p-1"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M19 9l-7 7-7-7"
          ></path>
        </svg>
      </button>
    </div>
    <TSLForm v-show="show_tsl" @add-tsl="(data) => $emit('add_tsl', data)" />
  </Card>
</template>

<script>
import TSLForm from "./TSLForm.vue";
import Card from "./Card.vue";

export default {
  data() {
    return {
      show_tsl: false,
    };
  },
  components: {
    TSLForm,
    Card,
  },
  methods: {
    async toggleTSLelement() {
      this.show_tsl = !this.show_tsl;
    },
  },
  emits: ["add_tsl"],
};
</script>
