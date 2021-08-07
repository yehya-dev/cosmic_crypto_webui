<template>
  <form class="flex flex-col gap-6 my-6">
    <div class="flex flex-col gap-1">
      <label
        class="font-mono font-light text-xs lg:text-sm text-gray-200"
        for="api_key"
        >cc_api_key</label
      >
      <input
        v-model="api_key"
        class="
          block
          text-gray-800
          font-bold
          px-2
          lg:px-3
          py-[1px]
          lg:py-1
          rounded-sm
          bg-gray-300
        "
        type="text"
        id="cc_api_key"
      />
    </div>
    <div class="flex flex-col gap-1">
      <label
        class="font-mono font-light text-xs lg:text-sm text-gray-200"
        for="symbol"
        >symbol</label
      >
      <input
        v-model="symbol"
        class="
          block
          text-gray-800
          font-bold
          px-2
          lg:px-3
          py-[1px]
          lg:py-1
          rounded-sm
          bg-gray-300
        "
        placeholder="BTCUSDT"
        type="text"
        id="symbol"
      />
    </div>
    <div class="flex flex-col gap-1">
      <label
        class="font-mono font-light text-xs lg:text-sm text-gray-200"
        for="follow_percentage"
        >follow_percentage</label
      >
      <input
        v-model="follow_percentage"
        class="
          block
          text-gray-800
          font-bold
          px-2
          lg:px-3
          py-[1px]
          lg:py-1
          rounded-sm
          bg-gray-300
        "
        type="number"
        id="follow_percentage"
      />
    </div>
    <div class="flex flex-col gap-1">
      <label
        class="font-mono font-light text-xs lg:text-sm text-gray-200"
        for="amount"
        >amount</label
      >
      <input
        v-model="amount"
        class="
          block
          text-gray-800
          font-bold
          px-2
          lg:px-3
          py-[1px]
          lg:py-1
          rounded-sm
          bg-gray-300
        "
        type="number"
        id="amount"
      />
    </div>
    <Button @click.prevent="add_tsl" />
  </form>
</template>

<script>
import Button from "./Button.vue";

export default {
  name: "TSLForm",
  components: {
    Button,
  },
  data() {
    return {
      api_key: "",
      symbol: "",
      follow_percentage: 0.0,
      amount: 0.0,
    };
  },
  methods: {
    async add_tsl() {
      if (
        this.api_key &&
        this.symbol &&
        this.follow_percentage &&
        this.amount
      ) {
        // TODO Perfrom checks to make sure the values are valid
        let new_tsl = {
            cc_api_key: this.api_key,
            symbol: this.symbol,
            follow_percentage: this.follow_percentage,
            amount: this.amount
        }
        this.$emit('add-tsl', new_tsl);
      }
      else {
          // TODO Change alert with better view
          alert('Make sure all the provided values are valid!')
      }
    },
  },
  emits: ["add-tsl"],
};
</script>
