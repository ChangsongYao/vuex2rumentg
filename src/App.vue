<template>
  <div id="app" v-cloak>
    <ez-counter></ez-counter>
    <ez-clock></ez-clock>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const modCounter = {
    state:{counter:0},
    mutations:{ INCREASE(state){state.counter++} },
    actions:{ inc(context){context.commit('INCREASE') }}
  }

  const modClock = {
    state:{time:Date.now()},
    mutations:{SET_TIME(state,val){state.time = val}},
    actions:{ setTime(context,val){ context.commit('SET_TIME',val)}}
  }

  const store = new Vuex.Store({
    modules:{
      m1: modCounter,
      m2: modClock
    }
  });

  const EzCounter = {
    template:'<div class="counter">{{counter}}</div>',
    computed:{
      counter(){ return this.$store.state.m1.counter }
    },
    methods:{
      inc(){ this.$store.dispatch('inc')}
    },
    created(){
      setInterval(()=>this.inc(),1000)
    }
  }

  const EzClock = {
    template:'<div class="clock">{{time}}</div>',
    computed:{
      time(){ return moment(this.$store.state.m2.time).format('LTS')}
    },
    methods:{
      setTime(){
        this.$store.dispatch('setTime',Date.now())
      }
    },
    created(){
      setInterval(()=>this.setTime(),1000)
    }
  }

  export default {
    name: 'App',
    store:store,
    components:{EzCounter,EzClock}
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
