<template>
  <div id="app" v-cloak>
    <ez-counter></ez-counter>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const modCounter = {
    namespaced:true,
    state:{counter:0},
    getters:{  doubled:state=>state.counter*2 },
    mutations:{ INCREASE:state => state.counter++ },
    actions:{ inc: context => context.commit('INCREASE')}
  }


  const store = new Vuex.Store({
    modules:{m1: modCounter}
  });

  const EzCounter = {
    template:'<div class="counter">{{counter}}</div>',
    computed:{
      counter(){ return this.$store.state.m1.counter }
    },
    methods:{
      inc(){ this.$store.dispatch('m1/inc')}
    },
    created(){
      setInterval(()=>this.inc(),1000)
    }
  }

  export default {
    name: 'App',
    store:store,
    components:{EzCounter}
  }

</script>

<style>
  .counter,.clock{
    font-family:LED;
    font-size:80px;
    padding:10px;
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
