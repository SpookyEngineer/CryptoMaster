<template>
  <div>
    <div>
      <div
        class="bg-gray-900 h-56 rounded-md flex justify-center items-center relative"
      >
        <div>
          <img
            class="px-2 w-20 h-auto mr-5 rounded-full"
            :src="iconbase"
            alt="currency icon"
          />
        </div>
        <div class="row no-gutters mt-1">
          <div class="col text-left">
            <div class="text-white">{{ info.name }}</div>
            <div class="text-white">
              <b class="text-white">{{ info.base }}</b
              >/{{ info.quote }}
            </div>
            <div class="text-white" v-if="ticker.price">
              {{ ticker.price || ""
              }}<span
                style="font-size: x-small; font-weight: 700; padding-left: 3px"
                >{{ info.quote }}</span
              >
            </div>
          </div>
        </div>
        <button class="h-6 w-6 absolute top-4 right-4" @click="removeCard()">
          <img
            src="../images/close-button.svg"
            alt="x button"
            title="X Button"
          />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { unSubscribeSymbol } from "../services/binance";
export default {
  props: ["ticker", "info"],
  data() {
    return {
      showDropDown: false,
    };
  },
  computed: {
    iconbase() {
      return `https://s2.coinmarketcap.com/static/img/coins/64x64/${this.info.cid}.png`;
    },
  },
  methods: {
    onDropDown() {
      this.showDropDown = true;
    },
    removeCard() {
      this.showDropDown = false;
      unSubscribeSymbol(this.info.symbol);
      this.$store.commit("REMOVE_COIN_PAIR", this.info.symbol);
    },
    closeDropDown() {
      this.showDropDown = false;
    },
  },
};
</script>
