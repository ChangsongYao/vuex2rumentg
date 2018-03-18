<template>
  <div id="app" v-cloak>
    <ez-counter></ez-counter>
    <ez-fast-counter></ez-fast-counter>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const modStateFactory = {
    namespaced:true,
    state(){
      return {counter:0}
    },
    mutations:{
      INCREASE:state => state.counter++
    },
    actions:{
      inc: context => context.commit('INCREASE')
    }
  }

  const modFactory = function(){
    return {
      namespaced:true,
      state:{
        counter:0
      },
      mutations:{
        INCREASE :state => state.counter ++
      },
      actions:{
        inc: context => context.commit('INCREASE')
      }
    }
  }

  const store = new Vuex.Store({
    modules:{
      m1: modStateFactory,
      m2: modStateFactory
    }
  });

  const EzCounter = {
    template:'<div class="counter">{{counter}}</div>',
    computed:Vuex.mapState('m1',['counter']),
    methods: Vuex.mapActions('m1',['inc']),
    created(){
      setInterval(() => this.inc(),1000)
    }
  }

  const EzFastCounter = {
    template:'<div class="counter">{{counter}}</div>',
    computed:Vuex.mapState('m2',['counter']),
    methods: Vuex.mapActions('m2',['inc']),
    created(){
      setInterval(() => this.inc(),100)
    }
  }

  export default {
    name: 'App',
    store:store,
    components:{EzCounter,EzFastCounter}
  }

</script>

<style>
  .counter{
    font-family:LED;
    font-size:80px;
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
