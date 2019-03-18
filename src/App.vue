<template>
  <div id="app">
    <Calculator v-on:api-call="apiCall"></Calculator>
    <Quote v-bind:quote="quote"></Quote>
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
      quote:''
    }
  },
  methods:{
    apiCall(){
      axios.get('https://api.kanye.rest')
      .then(response=>this.quote=response.data.quote)
      .catch(err=>console.log(err))
    }
  }
}
</script>

<style lang="scss">
</style>
