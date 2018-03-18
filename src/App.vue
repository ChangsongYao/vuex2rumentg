<template>
  <div id="app">
    <ez-counter></ez-counter>
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
      }
    },
    actions:{
      inc(context){
        context.commit('INCREASE');
      }
    }
  });

  const EzCounter = {
    template: '<div class="counter">{{counter}}</div>',
    computed:Vuex.mapState(['counter']),
    methods:{
      inc(){ this.$store.dispatch('inc') }
    },
    created(){
      setInterval(()=>this.inc(),100);
    }

  };

  export default {
    name: 'App',
    store:store,
    components:{EzCounter}
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
