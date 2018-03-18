<template>
  <div id="app" v-cloak>
    <div class="counter">{{counter}}</div>
    <pre>{{ $store._mutations | dump }}</pre>
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
      INCREASE:state => state.counter++,
      RESET: state => state.counter = 0
    }
  });

  export default {
    name: 'App',
    store:store,
    computed:{
      counter(){ return this.$store.state.counter}
    },
    created(){
      setInterval(()=>this.$store.commit('INCREASE'),100);
    },
    filters:{
      dump(o){
        return JSON.stringify(o,(k,v)=>{
          if(Array.isArray(v)){
            return '['+ v.join(',') + ']'
          }
          return v;
        },'\t')
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
