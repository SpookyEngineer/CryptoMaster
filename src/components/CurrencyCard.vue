<template>
    <div class="coin-box" @dblclick.stop="openDetails">
        <div class="row no-gutters coin-info">
            <div class="col-7">
                <div class="font-weight-bold">{{info.name}}</div>
                <div class="row no-gutters mt-1">
                    <div class="box-icon">
                        <span :style="{ backgroundImage : 'url('+ iconbase +')' }"></span>
                    </div>
                    <div class="col text-left">
                        <div><b>{{info.base}}</b>/{{info.quote}}</div>
                        <div class="coin-price" v-if="ticker.price">{{ticker.price || '' }}<span style="font-size: x-small; font-weight: 700; padding-left: 3px;">{{info.quote}}</span></div>
                    </div>
                </div>
            </div>
            <div class="dd-container" :class="[{'show': showDropDown}]">
                    <span role="button" class="menu-btn" @click.stop="onDropDown">
                        <i class="fa fa-ellipsis-v" aria-hidden="true"></i>
                    </span>
                <div class="dd-menu" v-if="showDropDown">
                    <span class="dd-item" @click="openDetails">Open</span>
                    <span class="dd-item" @click="removeCard">Delete</span>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
  import {unSubscribeSymbol} from '../services/binance'
  export default {
    props: ['ticker', 'info'],
    data() {
      return {
        showDropDown: false
      }
    },
    computed: {
        iconbase() {
            return `https://s2.coinmarketcap.com/static/img/coins/64x64/${this.info.cid}.png`
        }
    },
    methods: {
      onDropDown() {
        this.showDropDown = true;
      },
      removeCard() {
        this.showDropDown = false;
        unSubscribeSymbol(this.info.symbol);
        this.$store.commit('REMOVE_COIN_PAIR', this.info.symbol)
      },
      closeDropDown() {
        this.showDropDown = false;
      }
    }
  }
</script>
