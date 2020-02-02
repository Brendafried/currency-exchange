<template>
  <div class='container'>
    <div class='step1' v-if="this.step===1">
      Choose the currency you would like to convert:
      
      <currency-grid @clickedRate="setConversionCurrency"/>
      
    </div>

    <div class='step2' v-else-if="this.step===2">
      Enter price
      <input class='input' type="number" v-model='price'/>
      <button class='button' @click='moveStep(3)'>Continue</button>
      <div class='back' @click="goBackStep">🔙</div>
    </div>  

    <div class='step3' v-else-if="this.step===3">
      Convert to:

      <currency-grid @clickedRate="getConversion" />
      <div class='back' @click="goBackStep">🔙</div>
    </div> 

    <div class='step4' v-else-if="this.step===4">
      <div class='result'> {{`${this.convertedPrice} ${this.base}`}} </div>
      <button class='button' @click="moveStep(1)"> Convert Again </button>
      <div class='back' @click="goBackStep">🔙</div>
    </div>

    <div v-else> 
      Oops something went wrong. 
      <div class='back' @click="goBackStep">🔙</div>
    </div>
        
  </div>
</template>

<script>
// @ is an alias to /src

import CurrencyGrid from '../components/CurrencyGrid'
import {convertCurrency} from '../utils/api.js'

export default {
  name: 'home',
  components: {
    CurrencyGrid
  },
  data () {
    return {
      price: 0.00,
      base: undefined,
      currencyToConvert: undefined,
      response: undefined,
      step: 1,
      convertedPrice: undefined
    }
  },
  methods: {
    exit() {
      this.show = false;
    },
    setConversionCurrency(code) {
      this.currencyToConvert = code
      this.moveStep(2)
    },
    moveStep(number) {
      this.step = number
    },
    getConversion(code) {
      this.base = code

      convertCurrency(this.base).then(res => {
        let response = res.data.rates
        let currencyRate = response[this.currencyToConvert]
        this.convertedPrice = this.price/currencyRate
        this.convertedPrice = Math.round((this.convertedPrice + Number.EPSILON) * 100) / 100
        this.moveStep(4)
      })
      
    },
    goBackStep() {
      this.step = this.step - 1
    }


  }
}
</script>
<style>
.container {
  width: 100%;
  height: 100%;
  font-size: 36px;
}
.grid {
  display: flex;
  flex-wrap: wrap;
}
.grid > div {
  width: 15%;
  min-width: 175px;
  height: 15%;
  margin: 5px;
  text-align: center;
  line-height: 75px;
  font-size: 30px;
}

.box:hover {
  background: rgb(170, 238, 250);
  cursor: pointer;
}

.text {
  font-size: 25.5vw;
}


.step2 {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
  font-size: 36px;
  align-items: center;

}

.step4 {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: column;
  font-size: 36px;
  align-items: center;
}

.input {
  width: 30%;
  border: 3px solid #00B4CC;
  padding: 5px;
  height: 20px;
  border-radius: 5px 5px 5px 5px;
  outline: none;
  color: black;
  margin: 20px;
}

.button {
  width: 10%;
  height: 30px;
  background: #00B4CC;
  border: 3px solid rgb(154, 239, 250);
  border-radius: 5px 5px 5px 5px;
  margin: 20px;
  color: rgb(154, 239, 250);
  cursor: pointer;
}

.back {
  position: absolute;
  top: 10%;
  left: 10%;
  cursor: pointer;
}

.result {
  color: rgb(170, 238, 250);
  font-size: 4em;
  font-weight: bold;
  font-family: Helvetica;
  text-shadow: 
    0 1px 0 rgb(140, 192, 201), 
    0 2px 0 #73a3af, 
    0 3px 0 rgb(91, 135, 148), 
    0 4px 0 #578694, 
    0 5px 0 rgb(74, 110, 124), 
    0 6px 1px rgba(0,0,0,.1), 
    0 0 5px rgba(0,0,0,.1), 
    0 1px 3px rgba(0,0,0,.3), 
    0 3px 5px rgba(0,0,0,.2), 
    0 5px 10px rgba(0,0,0,.25), 
    0 10px 10px rgba(0,0,0,.2), 
    0 20px 20px rgba(0,0,0,.15);
}
</style>