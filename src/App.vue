<template>
  <div id="app" v-cloak>
    <button>复位计数器</button>
    <ez-counter></ez-counter>
    <ez-clock></ez-clock>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const modCounter = {
    namespaced:true,
    state:{counter:0},
    mutations:{
      INCREASE:state => state.counter++,
      RESET:state => state.counter = 0
    },
    actions:{
      inc:context => context.commit('INCREASE'),
      reset: context => context.commit('RESET')
    }
  }

  const modClock = {
    namespaced:true,
    state:{time:Date.now()},
    getters:{time_lts: state => moment(state.time).format('LTS') },
    mutations:{SET_TIME:(state,val) => state.time = val},
    actions:{setTime : (context,val) => context.commit('SET_TIME',val) }
  }

  const store = new Vuex.Store({
    modules:{
      m1: modCounter,
      m2: modClock
    }
  });

  const EzCounter = {
    template:'<div class="counter">{{counter}}</div>',
    computed: Vuex.mapState('m1',['counter']),
    methods:Vuex.mapActions('m1',['inc']),
    created(){
      setInterval(()=>this.inc(),1000)
    }
  }

  const EzClock = {
    template:'<div class="clock">{{time}}</div>',
    computed:Vuex.mapGetters('m2',{time:'time_lts'}),
    methods:Vuex.mapActions('m2',['setTime']),
    created(){
      setInterval(()=>this.setTime(Date.now()),1000)
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
