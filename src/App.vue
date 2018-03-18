<template>
  <div id="app" v-cloak>
    <button @click="add">新建备忘</button>
    <span>{{active | json}}</span>
    <ul>
      <li v-for="note in notes">{{note.text}}</li>
    </ul>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const store = new Vuex.Store({
    state:{
      notes:[],
      active:{}
    },
    mutations:{
      NEW_NOTE(state,note){
        state.notes.unshift(note);
      },
      ACTIVATE_NOTE(state,note){
        state.active = note;
      }
    }
  });

  export default {
    name: 'App',
    store:store,
    computed:{
      notes(){ return this.$store.state.notes},
      active(){return this.$store.state.active}
    },
    methods:{
      add(){
        let note = { text: '备忘 - ' + (this.notes.length + 1) + '#'};
        this.$store.commit('NEW_NOTE',note);
      }
    },
    filters:{
      json(v){ return JSON.stringify(v)}
    }
  }

</script>

<style>
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
