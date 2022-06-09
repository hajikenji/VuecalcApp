<template>
  <div class="card alert-primary">
    <div class="card-body text-left">
      <h2 class="card-title text-center">{{ title }}</h2>
      <p class="card-text h5">{{ message }}</p>
      <hr>
      <div>
        <div class="form-group">
          <label>Formula:</label>
          <textarea class="form-control mb-2" v-model="formula"></textarea>
        </div>
        <div class="text-center">
          <button class="btn btn-primary" v-on:click="calcAction">calc</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalcSystem',
  props: {
    title: String
  }
  ,
  data(){
    return {
      message: 'enter',
      formula: ''
    }
  }
  ,
  methods: {
    calcAction(){
      // console.log(this.formula)
      let calc = new calcMethods(this.formula);
      // calc.sayConsole();
      calc.shapeOfData();
      calc.makeSentence();
      calc.excuteSentence();
      calc.outputText()

      this.$emit('result-event', calc.getOutputArray(), calc.getAnswer())
    }
  },
}

class calcMethods{

  constructor(input){

    if (input.match(/\w/)) {
      this._mathData = input;
    }
  }

  shapeOfData(){
    this._arrayData = this._mathData.trim().split('\n');
    this._lastData = this._arrayData.pop();
  }

  makeSentence(){
    let sentence = '';
    this._arrayData.forEach(element => {
      if (element.trim() != '') {
        sentence += 'let ' + element + ';';
      }
    });
    
    sentence += 'return ' + this._lastData + ';';

    this._executableText = 'function f(){' + sentence + '} f();';
  }

  excuteSentence(){
    this._answer = eval(this._executableText);
  }

  outputText() {
    // this.message = 
    this._outputArray = this._arrayData.join(';').trim();
    if (this._outputArray != '') {
      this._outputArray += ';'
    }
    this._outputArray += this._lastData;
  }

  getAnswer(){
    return this._answer
  }
  getOutputArray(){
    return this._outputArray
  }

}


</script>