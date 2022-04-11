<template>
  <div class="grid gap-4 lg:grid-cols-3 lg:grid-rows-1 md:grid-cols-2 md:grid-rows-2 sm:grid sm:grid-cols-1 sm:grid-rows-3">
    <div v-for="value in currencies" :key="value.symbol">
      <CurrencyCard :ticker="tickers[value.symbol] || {}" :info="value"></CurrencyCard>
    </div>
  </div>
</template>



<script>
  import CurrencyCard from '../components/CurrencyCard.vue'
  import { mapState } from 'vuex'
  export default {
    name: 'CryptoBoard',
    components: {
      CurrencyCard
    },
    computed: {
      ...mapState(['tickers', 'currencies'])
    }
  }
</script>
