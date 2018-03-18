<template>
  <div id="app" v-cloak>
    <input type="number" v-model="counter">
    <div class="counter">{{counter}}</div>
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
    mutations:{
      INCREASE(state){
        state.counter++;
      },
      RESET(state,val){
        state.counter = val;
      }
    }
  });

  export default {
    name: 'App',
    store:store,
    computed:{
      counter:{
        get() { return this.$store.state.counter},
        set(v){ this.$store.commit('RESET',v)}
      }
    }
  }

</script>

<style>
  .counter{
    font-family:LED;
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
