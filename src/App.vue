<template>
  <div id="app" v-cloak>
    <button @click="register">添加计数器</button>
    <button @click="unregister">删除计数器</button>
    <div>
      <ez-counter v-for="counter in counters" :ns="counter"></ez-counter>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const modCounter = {
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

  const EzCounter = {
    props:['ns'],
    template:'<div class="counter">{{counter}}</div>',
    computed:{
      counter(){ return this.$store.state[this.ns].counter }
    },
    methods: {
      inc(){ this.$store.dispatch(this.ns+'/inc') }
    },
    created(){
      setInterval(() => this.inc(),100)
    }
  }


  const store = new Vuex.Store({});

  export default {
    name: 'App',
    data:function () {
      return{
        counters:[]
      }
    },
    store:store,
    methods:{
      register(){
        let name = 'm' + (this.counters.length+1);
        this.$store.registerModule(name,modCounter);
        this.counters.push(name);
      },
      unregister(){
        let name = 'm' + this.counters.length;
        this.counters.pop();
        this.$store.unregisterModule(name);
      }
    },
    components:{EzCounter}
  }

</script>

<style>
  .counter{
    font-family:LED;
    font-size:60px;
    margin:5px;
    padding:5px;
    background:#f0f0f0;
    display:inline-block;
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
