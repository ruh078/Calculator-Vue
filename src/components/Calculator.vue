<template>
  <div class="calculator">
     <div class="display">{{value || '0'}}</div>
     <div class="btn" @click="clear">C</div>
     <div class="btn" @click="sign_change">±</div>
     <div class="btn" @click="percent">%</div>
     <div class="btn operator" @click="divide">÷</div>
     <div class="btn" @click="append('7')">7</div>
     <div class="btn" @click="append('8')">8</div>
     <div class="btn" @click="append('9')">9</div>
     <div class="btn operator" @click="multiply">×</div>
     <div class="btn" @click="append('4')">4</div>
     <div class="btn" @click="append('5')">5</div>
     <div class="btn" @click="append('6')">6</div>
     <div class="btn operator" @click="subtract">-</div>
     <div class="btn" @click="append('1')">1</div>
     <div class="btn" @click="append('2')">2</div>
     <div class="btn" @click="append('3')">3</div>
     <div class="btn operator" @click="add">+</div>
     <div class="btn col2" @click="append('0')">0</div>
     <div class="btn" @click="dot">.</div>
     <div class="btn operator" @click="equal">=</div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      value: '',
      value1: '',
      value2: '',
      fvalue: '',
      prevalue: '',
      preoperator: null,
      operator_sign:'',
      preoperator_sign:'/',
      op: false,
      operator: null,
      bodmas: false,
      change: false,
    }
  },
  methods: {
    clear(){
      this.value= '',
      this.value1= '',
      this.value2= '',
      this.fvalue= '',
      this.prevalue= '',
      this.preoperator= null,
      this.operator_sign='',
      this.preoperator_sign='/',
      this.op= false,
      this.operator= null,
      this.bodmas= false,
      this.change=false
    },
    append(character){
      if(character==='0'){
        if((this.value!=='' && parseFloat(this.value)!==0) || this.value.indexOf('.')!==-1){
          this.value = `${this.value}${character}`;
        }
      }
      else{
        this.value = `${this.value}${character}`;
      }
      if(this.op){
        if(this.bodmas){
        this.bodmas=false
        this.value2 = `${this.value2}${character}`;
        this.change=true;
        }
        else{
          this.value2 = `${this.value2}${character}`;
        }
      }
    },
    dot(){
      if (this.op===false && this.value.indexOf('.')===-1){
        this.append('.');
      }
      else if(this.op===true && this.value2.indexOf('.')===-1){
        this.append('.')
      }
    },
    sign_change(){
      if(this.value!=='' && parseFloat(this.value)!==0 && this.op===false){
      if(this.value[0]==='-'){
        this.value = this.value.substring(1);
      }
      else{
        this.value = `${'-'}${this.value}`
      }
    }
    },
    add(){
      if(this.change){
        this.value2 = this.operator(this.prevalue, this.value2);
        this.operator=this.preoperator;
        this.operator_sign = this.preoperator_sign; 
        this.change=false;
      }
      if(this.operator){
          if(this.fvalue===''){
            this.fvalue = this.operator(parseFloat(this.value1), parseFloat(this.value2));
          }
          else{
            this.fvalue = this.operator(parseFloat(this.fvalue), parseFloat(this.value2));
          }
      }
      this.value1=this.value;
      this.value = `${this.value}${'+'}`;
      this.op = true;
      this.operator = (a,b)=>a+b;
      this.operator_sign = '+';
      this.value2='';
    },
    subtract(){
      if(this.change){
        this.value2 = this.operator(this.prevalue, this.value2);
        this.operator=this.preoperator;
        this.operator_sign = this.preoperator_sign; 
        this.change=false;
      }
      if(this.operator){
          if(this.fvalue===''){
            this.fvalue = this.operator(parseFloat(this.value1), parseFloat(this.value2));
          }
          else{
            this.fvalue = this.operator(parseFloat(this.fvalue), parseFloat(this.value2));
          }
      }
      this.value1=this.value;
      this.value = `${this.value}${'-'}`;
      this.op = true;
      this.operator = (a,b)=>a-b;
      this.operator_sign = '-';
      this.value2='';
    },
    multiply(){
      if(this.change){
        this.value2 = this.operator(this.prevalue, this.value2);
        this.operator=this.preoperator;
        this.operator_sign = this.preoperator_sign; 
        this.change=false;
      }
      if(this.operator){
          if(this.fvalue===''){
            this.fvalue = this.operator(parseFloat(this.value1), parseFloat(this.value2));
          }
          else{
            this.fvalue = this.operator(parseFloat(this.fvalue), parseFloat(this.value2));
          }
      }
      if(this.operator_sign==='+'){
        this.fvalue=parseFloat(this.fvalue)-parseFloat(this.value2);
        this.preoperator=(a,b)=>a+b;
        this.preoperator_sign='+'
        this.prevalue=this.value2;
        this.bodmas=true;
        console.log(this.fvalue)
        console.log(this.prevalue)
      }
      else if(this.operator_sign==='-'){
        console.log(this.fvalue)
        console.log(this.value2)
        this.fvalue=parseFloat(this.fvalue)+parseFloat(this.value2);
        this.preoperator=(a,b)=>a-b;
        this.preoperator_sign='-'
        this.prevalue=this.value2;
        this.bodmas=true;
        console.log(this.fvalue)
        console.log(this.prevalue)
      }
      this.value1=this.value;
      this.value = `${this.value}${'×'}`;
      this.op = true;
      this.operator = (a,b)=>a*b;
      this.operator_sign = '*';
      this.value2='';
    },
    divide(){
      if(this.change){
        this.value2 = this.operator(this.prevalue, this.value2);
        this.operator=this.preoperator;
        this.operator_sign = this.preoperator_sign; 
        this.change=false;
      }
      if(this.operator){
          if(this.fvalue===''){
            this.fvalue = this.operator(parseFloat(this.value1), parseFloat(this.value2));
          }
          else{
            this.fvalue = this.operator(parseFloat(this.fvalue), parseFloat(this.value2));
          }
      }
      if(this.operator_sign==='+'){
        this.fvalue=parseFloat(this.fvalue)-parseFloat(this.value2);
        this.preoperator=(a,b)=>a+b;
        this.preoperator_sign='+'
        this.prevalue=this.value2;
        this.bodmas=true;
        console.log(this.fvalue)
        console.log(this.prevalue)
      }
      else if(this.operator_sign==='-'){
        console.log(this.fvalue)
        console.log(this.value2)
        this.fvalue=parseFloat(this.fvalue)+parseFloat(this.value2);
        this.preoperator=(a,b)=>a-b;
        this.preoperator_sign='-'
        this.prevalue=this.value2;
        this.bodmas=true;
        console.log(this.fvalue)
        console.log(this.prevalue)
      }
      this.value1=this.value;
      this.value = `${this.value}${'÷'}`;
      this.op = true;
      this.operator = (a,b)=>a/b;
      this.operator_sign = '/';
      this.value2='';
    },
    equal(){
      if(this.change){
        this.value2 = this.operator(this.prevalue, this.value2);
        this.operator=this.preoperator;
        this.operator_sign = this.preoperator_sign; 
        this.change=false;
      }
       if(this.operator){
          if(this.fvalue===''){
            this.fvalue = this.operator(parseFloat(this.value1), parseFloat(this.value2));
          }
          else{
            this.fvalue = this.operator(parseFloat(this.fvalue), parseFloat(this.value2));
          }
      }
       this.value = this.fvalue
       this.value1 = '';
       this.value2 = '';
       this.fvalue= '',
       this.prevalue= '',
       this.preoperator= null,
       this.operator_sign='',
       this.preoperator_sign='/',
       this.op= false,
       this.operator= null,
       this.bodmas= false,
       this.change=false
    },
    percent(){
      if(this.value2){
        this.value = this.value.substring(0,this.value.lastIndexOf(this.value2));
        this.value2=parseFloat(this.value2/100);
        this.value=this.value+this.value2;
      }
      else if(!this.op){
        this.value=parseFloat(this.value/100)
        this.value1=parseFloat(this.value1/100)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator{
  width:30%;
  margin:auto;
  font-size:45px;
  display: grid;
  grid-template-columns: repeat(4,1fr);
  grid-auto-rows: minmax(50px);
}
.display {
  grid-column: 1/5;
  background-color:rgb(226, 226, 226);
}
.btn {
  background-color: rgb(169 169 169);
  border: 1px solid;
  border-color:rgb(182, 182, 182); 
}
.btn:hover{
  background-color:rgb(194, 192, 192)
}
.operator { 
   background-color: rgb(231, 141, 6);
   border-color: rgb(231, 126, 6)
}
.operator:hover { 
   background-color: rgb(231, 126, 6);
}
.fvalue {
  font-size:25px;
}
.col2{
  grid-column: 1/3;
}

@media screen and (max-width: 1000px) {
  [class=calculator]{
    width:60%;
  }
}
</style>
