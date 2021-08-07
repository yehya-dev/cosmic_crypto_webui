<template>
  <section>
    <header class="m-5">
      <h1
        style="font-family: Megrim"
        class="text-center text-3xl lg:text-4xl font-bold text-gray-800"
      >
        Cosmic Crypto
      </h1>
    </header>
    <main class="flex flex-col gap-4">
      <AddTSL @add_tsl="add_tsl" />
      <Card v-for="(tsl, index) in active_tsls" :key="index">
        <div class="flex justify-between items-center">
        <h3 class="text-2xl">{{ tsl.symbol }}</h3>
        <span class="font-bold text-green-400 text-lg">{{tsl.new_price}}</span>
        </div>
        <div class="pt-1 text-center flex flex-col">
        <span class="text-sm font-bold">Trigger</span>
        <p class="font-bold text-lg text-red-500">{{tsl.trigger_price}}</p>
        </div>
      </Card>
    </main>
  </section>
</template>

<script>
import AddTSL from "./components/AddTSL.vue";
import Card from "./components/Card.vue";

import axios from "axios";
import { io } from "socket.io-client";

export default {
  components: {
    AddTSL,
    Card,
  },
  data() {
    return {
      active_tsls: {
        /* 0: {
          'symbol': BTCUSDT,
          'new_price': 213.0,
          'trigger_price': 1123.3
        }
        */
      },
      sock_io: null,
    };
  },
  methods: {
    async add_tsl(tsl_data) {
      axios
        .post("http://127.0.0.1:8000/add_tsl", tsl_data)
        .then((response) => {
          console.log(response);
        })
        .catch((error) => {
          console.log(error.response.data);
        });
      // Init Websocket
      // TODO Try to connect ws only if the post request was a success
      await this.connect_sock("http://0.0.0.0:8080", tsl_data.cc_api_key);
    },

    // TODO Adding again and again cause more sockets to be connected. prevent that
    async connect_sock(url, api_key) {
      this.sock_io = io(url, {
        query: { api_key: api_key }, // This is sending api key as plaintext ehm
      });
      this.sock_io.on("new_price", (data) => {
        let id, tsl_data, key, value;

        for ([id, tsl_data] of Object.entries(data)) {
          if (!(this.active_tsls.hasOwnProperty(id))) {
            this.active_tsls[id] = {}
          }
          for ([key, value] of Object.entries(tsl_data)) {
            this.active_tsls[id][key] = value;
          }
        }
      });
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Megrim&display=swap");
</style>
