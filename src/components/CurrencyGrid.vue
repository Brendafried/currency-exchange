<template>
  <div class='currency_grid__container'>
    <div class='wrap'>
    <div class='search'>
        <input v-model='search' type="text" class="searchTerm" ref='search' placeholder="Type to search" />
        <button class="searchButton" >
          <i class="material-icons">
                    🕵️‍♀️
                </i>
        </button>
    </div>
    </div>
    <div class='grid'>
      <div class='grid_div' v-for="code in filteredCurrencies" :key="code.code" @click="setRate(code.code)">
        <currency class='box' :code='code.code' :text='code.currency' :country='code.country' ></currency>
      </div>
    </div>
  </div>
</template>

<script>

import currency from './currency'
import {codes} from '../utils/countries.js'

export default {
  name: 'currency_grid',
  components: {
    currency,
  },
  data () {
    return {
      search: ''
    }
  },
  computed: {    
    filteredCurrencies() {
      let filtered = [];
      if (this.currencies) {
        filtered = this.currencies.filter( currency => currency.code.toLowerCase().includes(this.search.toLowerCase()) ||
        currency.currency.toLowerCase().includes(this.search.toLowerCase()) || currency.country.toLowerCase().includes(this.search.toLowerCase()))
        return filtered
      } else return this.currencies
    },
    currencies() {
      return codes
    }
  },
  methods: {
    setRate(code) {
      this.$emit('clickedRate', code)
    }
  },
  
  }
</script>
<style>
.currency_grid__container {
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin: 5px;
  
}
.grid {
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 10%;
}
.grid_div {
  width: 10%;
  min-width: 50px;
  height: 15%;
  margin: 10px;
  text-align: center;
  line-height: 75px;
  font-size: 30px;
}

.box:hover {
  background: #00B4CC;
  cursor: pointer;
}

.text {
  font-size: 25.5vw;
}

.search {
  width: 100%;
  position: relative;
  display: flex;
}

.searchTerm {
  width: 100%;
  border: 3px solid #00B4CC;
  border-right: none;
  padding: 5px;
  height: 20px;
  border-radius: 5px 0 0 5px;
  outline: none;
  color: #9DBFAF;
}

.searchTerm:focus{
  color: #00B4CC;
}

.searchButton {
  width: 40px;
  height: 36px;
  border: 1px solid #00B4CC;
  background: #00B4CC;
  text-align: center;
  color: #fff;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  font-size: 20px;
}

/*Resize the wrap to see the search bar change!*/
.wrap{
  width: 60%;
  position: absolute;
  top: 20%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>