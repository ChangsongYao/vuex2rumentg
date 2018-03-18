<template>
  <div id="app" v-cloak>
    <ez-note-list></ez-note-list>
    <ez-note-editor></ez-note-editor>
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
    }
  });

  const EzNoteList = {
    template:'#tpl-list',
    computed:{
      notes:{
        get(){ return store.state.notes;},
        set(v){ store.state.notes = v;}
      },
      active:{
        get(){ return store.state.active;},
        set(v){ store.state.active = v;}
      }
    },
    methods:{
      add(){
        let note = {
          text: '备忘 - ' + (this.notes.length + 1) + '#'
        };
        this.notes.unshift(note);
        this.active = note;
      }
    },
    filters:{
      trim(v){
        return v.trim();
      },
      slice(v,start,end){
        return v.slice(start,end);
      }
    }
  };

  const EzNoteEditor = {
    template:'#tpl-editor',
    computed:{
      note:{
        get(){ return store.state.active;},
        set(v){ store.state.active = v;}
      }
    }
  };

  export default {
    name: 'App',
    components:{ EzNoteList,EzNoteEditor }
  }

</script>

<style>
  html,body,#app{
    height:100%;
    padding:0;
    margin:0;
    overflow:hidden;
  }
  #app{
    display:flex;
  }
  .note-list{
    width:230px;
    display:flex;
    flex-direction:column;
    background:#fafafa;
  }
  .note-list .toolbar{
    height:30px;
    line-height:30px;
    border-bottom:1px solid #ccc;
  }
  .note-list ul{
    flex:1;
    list-style:none;
    padding:0;
    margin:0;
    overflow-y:auto;
  }
  .note-list li{
    cursor:pointer;
    padding:5px;
  }
  .note-list li.active{
    background:blue;
    color:white;
  }
  .note-editor{
    flex:1;
  }
  .note-editor textarea{
    width:100%;
    height:100%;
    resize:none;
    outline:none;
    border:none;
    border-left:1px solid #ccc;
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
