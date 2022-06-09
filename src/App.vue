<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <CalcSystem title="Welcome to Your Vue.js App" v-on:result-event="appAction" />
  <div class="mt-3 text-left">
    <table class="table" v-html="log"></table>
  </div>
  <div>
    <button class="btn btn-danger" v-on:click="doCrear">消すぜ！</button>
  </div>
</template>

<script>
import CalcSystem from './components/Calc.vue'

export default {
  name: 'App',
  components: {
    CalcSystem
  }
  ,
  data(){
    return {
      message: 'calc',
      result: []
    };
  }
  ,
  computed: {
    log(){
      let table = '<tr><th>Expression</th><th>Value</th></tr>';
      if (this.result.length > 0) {
        for (const key in this.result) {
          table += '<tr><td>' + this.result[key][0] + '</td><th>' 
            + this.result[key][1] + '</th></tr>';
        }
      }
      return table;
    }
  }
  ,
  created() {
    let items = localStorage.getItem('log');
    let logs = JSON.parse(items);
    if(logs != null){ this.result = logs } 
    console.log(logs);
  }
  ,
  methods: {
    appAction(answer, literal){
      this.result.unshift([answer, literal]);
      const log = JSON.stringify(this.result);
      localStorage.setItem('log', log);
    }
    ,
    doCrear(){
      localStorage.removeItem('log');
      this.result = [];
    }
  }
  ,
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
