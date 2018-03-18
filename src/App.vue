<template>
  <div id="app" v-cloak>
    <button @click="reset">复位计数器</button>
    <button @click="clearLogs">清理日志</button>
    <ez-counter></ez-counter>
    <div v-for="log in logs" class="log" @click="time_travel(log)">
      {{log.ts | tformat }} {{log.mutation.type}}
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const _vm = new Vue({
    data:{logs:[]}
  });

  const ezLoggerPlugin = store => {
    store.subscribe((mutation,state)=>{
      _vm.logs.unshift({mutation:mutation,ts:Date.now()})
      if(_vm.logs.length>20) _vm.logs.pop();
    })
  };

  const store = new Vuex.Store({
    plugins:[ezLoggerPlugin],
    state:{
      counter:0
    },
    mutations:{
      INCREASE(state){ state.counter++; },
      RESET(state){ state.counter = 0}
    },
    actions:{
      inc(context){ context.commit('INCREASE') },
      reset(context) { context.commit('RESET')}
    }
  });

  const EzCounter = {
    template:'<div class="counter">{{counter}}</div>',
    computed: Vuex.mapState(['counter']),
    methods:Vuex.mapActions(['inc']),
    created(){
      setInterval(()=>this.inc(),2000)
    }
  }

  export default {
    name: 'App',
    store:store,
    computed:{
      logs(){ return _vm.logs }
    },
    methods: {
      ...Vuex.mapActions(['reset']),
      clearLogs(){
        _vm.logs = [];
      }
    },
    filters:{
      tformat(v){ return moment(v).format('HH:mm:ss') }
    },
    components:{EzCounter}
  }

</script>

<style>
  .counter{
    font-family:LED;
    font-size:100px;
  }
  .log{
    cursor:pointer;
    padding:5px;
  }
  .log:hover{
    background:#f0f0f0;
  }
  .log span{
    display:none;
    float:right;
  }
  .log:hover span{
    display:block;
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
