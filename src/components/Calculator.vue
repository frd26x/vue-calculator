<template>
  <div id="Calculator">
    <div class="display">{{total}}</div>
    <div v-on:click='pressNumber'   class="btn">7</div>
    <div v-on:click='pressNumber'   class="btn">8</div>
    <div v-on:click='pressNumber'   class="btn">9</div>
    <div v-on:click='pressOperator' class="btn operator">+</div>
    <div v-on:click='pressNumber'   class="btn">4</div>
    <div v-on:click='pressNumber'   class="btn">5</div>
    <div v-on:click='pressNumber'   class="btn">6</div>
    <div v-on:click='pressOperator' class="btn operator">-</div>
    <div v-on:click='pressNumber'   class="btn">1</div>
    <div v-on:click='pressNumber'   class="btn">2</div>
    <div v-on:click='pressNumber'   class="btn">3</div>
    <div v-on:click='pressOperator' class="btn operator">*</div>
    <div v-on:click='pressNumber'   class="btn">0</div>
    <div v-on:click='pressClear'    class="btn operator">C</div>
    <div v-on:click='pressResult'   class="btn operator">=</div>
    <div v-on:click='pressOperator' class="btn operator">/</div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
    return {
     total:'0' 
    }
  },
  methods:{
    pressNumber(event){
      if(this.total==='0'){
        this.total=''
      }
      let number = event.target.innerText
      this.total += number
    },
    pressOperator(event){
      let idxLastLetter = this.total.length-1
      let operator = event.target.innerText
      //Don't allow two operator in a row
      if(!this.total[idxLastLetter].match(/[/+*-]/)){
      this.total += operator
      }
      this.total = this.total.slice(0,-1) + operator
      
    },
    pressResult(){
      this.total = eval(this.total).toString()
      this.$emit('api-call')

    },
    pressClear(){
      this.total='0'
    }
  }
}
</script>

<style lang="scss">
#Calculator{
  margin: 0 auto;
  width: 400px;
  font-size: 40px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  width: 400px;
  background-color: rgb(152, 161, 172);
  border-radius: 10%;
  padding: 3%;
}

.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
  border-radius: 10%;
  padding: 1%;
  margin-bottom: 3%;
}
.btn {
  background-color: #F2F2F2;
  border: 1px solid #999;
  border-radius: 30%;
  text-align: center;
  margin: 2%;
}

.operator{
  background-color: orange;
}

</style>
