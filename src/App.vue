<template>
  <div id="app" v-cloak>
    <label>STEP</label>
    <input type="radio" value="1" v-model.number="step"> 1
    <input type="radio" value="6" v-model.number="step"> 6
    <input type="radio" value="10" v-model.number="step"> 10
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
      INCREASE(state,step){
        state.counter += step;
      }
    }
  });

  export default {
    name: 'App',
    store:store,
    data:function(){
      return{
        step:1
      }
    },
    computed:{
      counter(){ return this.$store.state.counter}
    },
    created(){
      setInterval(()=>this.$store.commit('INCREASE',this.step),1000);
    }
  }
  //原始的ES2015代码如下：
  // const store = new Vuex.Store({
  //   state:{
  //     counter:0
  //   },
  //   mutations:{
  //     INCREASE(state,step){
  //       state.counter += step;
  //     }
  //   }
  // });
  //
  // const vm = new Vue({
  //   el:'#app',
  //   store:store,
  //   data:{step:1},
  //   computed:{
  //     counter(){ return this.$store.state.counter}
  //   },
  //   created(){
  //     setInterval(()=>this.$store.commit('INCREASE',this.step),1000);
  //   }
  // })



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
