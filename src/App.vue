<template>
  <div id="app">
    <div class="counter"></div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)
  const store = new Vuex.Store({
    state:{
      counter:0
    },
  });

  new Vue().$watch(()=>{
    let el = document.querySelector('.counter');
    el.textContent = store.state.counter;
  })

  setInterval(()=>store.state.counter++,100);

  export default {
    name: 'App'
  }
</script>

<style>
  .counter{
    font-family: LED;
    font-size:100px;
  }
  [v-cloak]:after{
    content:' ';
    display:block;
    border-bottom:2px solid red;
    animation: progress 2s infinite;
  }
  @keyframes progress{
    0%{width:0%}
    100%{width:90%}
  }

</style>
