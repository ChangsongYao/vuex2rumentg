<template>
  <div id="app" v-cloak>
    <div class="counter">{{counter}}</div>
    <pre>{{ $store._actions | dump }}</pre>
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

  export default {
    name: 'App',
    store:store,
    computed:{
      counter(){ return this.$store.state.counter }
    },
    methods:{
      inc(){ this.$store.dispatch('inc') }
    },
    created(){
      setInterval(()=>this.inc(),100);
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
