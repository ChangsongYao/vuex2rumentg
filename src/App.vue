<template>
  <div id="app">
    <button onclick="inc()">INCREMENT</button>
    <div class="counter"></div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)
  const store = new Vuex.Store({
    strict:true,
    state:{
      counter:0
    },
    mutations:{
      INCREASE(state){
        state.counter++;
      }
    }
  })

  const inc = ()=>{
    store.state.counter++
  };

  store._vm.$watch(()=>{
    let el = document.querySelector('.counter');
    el.textContent = store.state.counter;
  })

  export default {
    name: 'App'
  }
</script>

<style>
  .counter{
    font-family:LED;
    font-size:100px;
  }

</style>
