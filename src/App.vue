<template>
  <div id="app" v-cloak>
    <ez-clock></ez-clock>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)
  const store = new Vuex.Store({
    state:{
      time:Date.now()
    },
    getters:{
      time_lts: state => moment(state.time).format('LTS')
    }
  });

  const EzClock = {
    template:'<div class="clock">{{time}}</div>',
    computed:{
      time(){ return this.$store.getters.time_lts}
    },
    methods:{
      setTime(){  this.$store.state.time = Date.now() }
    },
    created(){
      setInterval(()=>this.setTime(),1000)
    }
  };

  export default {
    name: 'App',
    store:store,
    components:{EzClock}
  }
</script>

<style>
  .clock{
    font-family:LED;
    font-size:80px;
    letter-spacing:10px;
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
