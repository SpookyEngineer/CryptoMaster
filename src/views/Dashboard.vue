<template>
  <div class="min-h-screen">
    <div class="flex flex-wrap justify-center items-center mt-5 px-2">
      <v-select
        class="h-full w-96 rounded-md bg-gray-200 overflow-ellipsis"
        id="base"
        :options="currencyList[quote]['pairs']"
        :clearable="false"
        v-model="baseCurrency"
        placeholder="Select Token"
      ></v-select>
      <div
        class="flex justify-center items-center ml-4 mt-3 md:mt-0 text-white text-sm"
      >
        <button
          class="focus:outline-none font-medium rounded-lg px-5 py-2.5 mr-2 bg-gray-800 hover:bg-gray-700 focus:ring-gray-700 border-gray-700"
          @click="addCoinPair"
        >
          Add
        </button>
        <button
          class="focus:outline-none font-medium rounded-lg px-5 py-2.5 mr-2 bg-gray-800 hover:bg-gray-700 focus:ring-gray-700 border-gray-700"
          @click="clear"
        >
          Reset
        </button>
      </div>
    </div>
    <CryptoBoard></CryptoBoard>
  </div>
</template>

<script>
import vSelect from "vue-select";
import coins from "@/assets/group.json";
import CryptoBoard from "@/views/CryptoBoard.vue";
import { isEmpty } from "../util/Utility";
import { subscribeSymbol } from "../services/binance";
import { mapState } from "vuex";

export default {
  name: "dashboard",
  data() {
    return {
      currencyList: coins,
      quote: "USDT",
      baseCurrency: {},
    };
  },
  mounted() {
    if (this.currencies) {
      this.currencies.forEach((currency) => {
        subscribeSymbol(currency.symbol);
      });
    }
  },
  computed: {
    ...mapState(["currencies"]),
    quoteOptions() {
      return Object.keys(coins);
    },
  },
  components: {
    vSelect,
    CryptoBoard,
  },
  methods: {
    resetBase() {
      this.baseCurrency = {};
    },
    clear() {
      localStorage.clear();
      location.reload();
    },
    addCoinPair() {
      if (!isEmpty(this.baseCurrency)) {
        const symbol = `${this.baseCurrency.value}${this.quote}`;
        subscribeSymbol(symbol);
        this.$store.commit("ADD_COIN_PAIR", {
          symbol: symbol,
          base: this.baseCurrency.value,
          quote: this.quote,
          name: this.baseCurrency.name,
          cid: this.baseCurrency.cid,
        });
      }
    },
  },
};
</script>
