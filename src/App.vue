<template>
  <div id="app">
    <Calculator v-on:api-call="apiCall"></Calculator>
    <Quote v-if="quote" v-bind:quote="quote"></Quote>
  </div>
</template>

<script>
//imports
import Calculator from './components/Calculator'
import Quote from './components/Quote'
import axios from 'axios'
export default {
  name: 'app',
  components:{
    Calculator,
    Quote
  },
  data () {
    return {
      quote:null
    }
  },
  methods:{
    apiCall(result){
      //The Api requires an integer
      let integer = parseInt(result)
      axios.get(`http://numbersapi.com/${integer}`)
      .then(response=>this.quote=response.data)
      .catch(err=>console.log(err))
    }
  }
}
</script>

<style lang="scss">
</style>
