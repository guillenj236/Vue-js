<template>
  <div class="calculadora">
    <h1>{{ msg }}</h1>
    <div >
        <table class="table table-bordered">
          <tbody>
            <tr class="resultado">
              <td colspan="4">
                {{ resultado || 0 }}
              </td>
            </tr>
            <tr>
              <td v-on:click="clearField">C</td>
              <td v-on:click="setNegativeOrPositive">+/-</td>
              <td v-on:click="calculatePercentage">%</td>
              <td v-on:click="processresultado('divide')" class="right-column">/</td>
            </tr>

            <tr>
              <td v-on:click="getNumber('7')">7</td>
              <td v-on:click="getNumber('8')">8</td>
              <td v-on:click="getNumber('9')">9</td>
              <td v-on:click="processresultado('multiply')" class="right-column">x</td>
            </tr>
            <tr>
              <td v-on:click="getNumber('4')">4</td>
              <td v-on:click="getNumber('5')">5</td>
              <td v-on:click="getNumber('6')">6</td>
              <td v-on:click="processresultado('subtract')" class="right-column">-</td>
            </tr>
            <tr>
              <td v-on:click="getNumber('1')">1</td>
              <td v-on:click="getNumber('2')">2</td>
              <td v-on:click="getNumber('3')">3</td>
              <td v-on:click="processresultado('add')" class="right-column">+</td>
            </tr>
            <tr>
              <td v-on:click="getNumber('0')" colspan="2">0</td>
              <td v-on:click="getDot()">.</td>
              <td v-on:click="updateresultado" class="right-column">=</td>
            </tr>
          </tbody>
        </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalculatorComponent',
  props: {
    msg: String
  },
  data(){
    return{
      resultado:'',
      previousValue:null,
      operationFired:false
    }
  },
  methods:{
    clearField(){
      this.resultado = '';
    },
    setNegativeOrPositive(){
      this.resultado = this.resultado[0] === '-'? this.resultado.slice(1):`-${this.resultado}`;
    },
    calculatePercentage(){
      this.resultado = parseFloat(this.resultado)/100
    },
    getNumber(number){
      if(this.operationFired){
        this.resultado = '';
        this.operationFired=false;
      }
      this.resultado=`${this.resultado}${number}`
    },
    getDot(){
      if(this.resultado.indexOf('.') === -1){
        this.resultado = this.resultado+'.'
      }
    },
    processresultado(operation){

      if(operation==='add'){
        this.operation=(a, b)=>{
          return parseFloat(a)+parseFloat(b);
        }
      }
      else if(operation==='subtract'){
        this.operation=(a, b)=>{
          return parseFloat(a)-parseFloat(b);
        }
      }
      if(operation==='divide'){
        this.operation=(a, b)=>{
          return parseFloat(a)/parseFloat(b);
        }
      }
      if(operation==='multiply'){
        this.operation=(a, b)=>{
          return parseFloat(a)*parseFloat(b);
        }
      }
      this.previousValue = this.resultado;
      this.operationFired=true;
    },
    updateresultado(){
      this.resultado= `${this.operation(this.previousValue, this.resultado)}`;
      this.previousValue=null;
    }

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 h3 {
  margin: 40px 0 0;
  }
  ul {
  list-style-type: none;
  padding: 0;
  }
  li {
  display: inline-block;
  margin: 0 10px;
  }
  a {
  color: #42b983;
  }
table{
  font-weight: bold;
  font-size: 23px;
  margin: 0 auto;
  width: 500px;
  height: 500px;
  background-color:#708090 ;
  border: 1px solid rgba(0, 0, 0, 0.331);
  }
  td:hover{
    background-color: #2F4F4F
  }

.resultado{
  text-align: right;
  background-color:#28363980;
  }

  .right-column{
  background-color:#BF1622;
  }
.resultado:hover{
  background-color: #28363980;
}

.right-column:active{
  background-color: #008080;
}
</style>
  