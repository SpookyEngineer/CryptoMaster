<template>
    <div>
        <div class="flex mt-5">
            <v-select class="ml-5 w-64 rounded-md bg-gray-200" id="base" :options="currencyList[quote]['pairs']" :clearable="false" v-model="baseCurrency"
                      placeholder="Select Token"></v-select>
            <button class="ml-4 text-white focus:outline-none font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2  bg-gray-800  hover:bg-gray-700  focus:ring-gray-700  border-gray-700" @click="addCoinPair"><i class="fa fa-plus fa-lg" aria-hidden="true">Add</i></button>
            <button class="ml-4 text-white focus:outline-none font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2  bg-gray-800  hover:bg-gray-700  focus:ring-gray-700  border-gray-700" @click="clear">Clear</button>
        </div>
        <CryptoBoard></CryptoBoard>
    </div>
</template>



<script>
    import vSelect from 'vue-select'
    import coins from '@/assets/group.json'
    import CryptoBoard from '@/views/CryptoBoard.vue'
    import {isEmpty} from '../util/Utility'
    import {subscribeSymbol} from '../services/binance'
    import {mapState} from 'vuex'

    export default {
        name: 'dashboard',
        data() {
            return {
                currencyList: coins,
                quote: 'USDT',
                baseCurrency: {}
            }
        },
        mounted() {
            if (this.currencies) {
                this.currencies.forEach(currency => {
                    subscribeSymbol(currency.symbol);
                });
            }
        },
        computed: {
            ...mapState(['currencies']),
            quoteOptions() {
                return Object.keys(coins)
            }
        },
        components: {
            vSelect,
            CryptoBoard
        },
        methods: {
            resetBase() {
                this.baseCurrency = {}
            },
            clear() {
                localStorage.clear();
                location.reload();
            },
            addCoinPair() {
                if (!isEmpty(this.baseCurrency)) {
                    const symbol = `${this.baseCurrency.value}${this.quote}`;
                    subscribeSymbol(symbol);
                    this.$store.commit('ADD_COIN_PAIR', {
                        "symbol": symbol,
                        "base": this.baseCurrency.value,
                        "quote": this.quote,
                        "name": this.baseCurrency.name,
                        "cid": this.baseCurrency.cid
                    })
                }
            }
        }
    }
</script>
