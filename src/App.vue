<template>
  <div id="app" v-cloak>
    <input :value="q" @change="search($event.target.value)" placeholder="search sth.">
    <div v-html="status" class="status"></div>
    <div class="result">
      <div class="item" v-for="item in result">
        {{item.title}}
      </div>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const fakeSearch = (q,cb) => {
    let ret = [];

    for(let i=1;i<11;i++) ret.push({
      title:'result item ' + i + '#'
    });

    setTimeout(()=>cb(ret),2000)
  }

  const store = new Vuex.Store({
    state:{
      q:'',
      result:[],
      status:''
    },
    mutations:{
      UPDATE_QUERY:(state,value) => state.q = value,
      UPDATE_STATUS:(state,value) => state.status = value,
      UPDATE_RESULT:(state,value) => state.result = value
    },
    actions:{
      search(context,q){
        context.commit('UPDATE_RESULT',[])
        context.commit('UPDATE_QUERY',q)
        context.commit('UPDATE_STATUS','searching <b>' + q +'</b>...')

        fakeSearch(q,result =>{
          context.commit('UPDATE_STATUS','')
          context.commit('UPDATE_RESULT',result)
        })
      }
    }
  })

  export default {
    name: 'App',
    store:store,
    computed:{
      ...Vuex.mapState(['result','status']),
      q:{
        get(){return this.$store.state.q},
        set(v){ this.$store.commit('set_q',v)}
      }
    },
    methods:{
      search(q){
        this.$store.dispatch('search',q);
      }
    }
  }

</script>

<style>
  input{
    line-height:40px;
    width:100%;
  }
  .status{
    padding:10px;
  }
  .result{
    margin-top:10px;
  }
  .item{
    line-height:50px;
    height:50px;
    margin-bottom:5px;
    padding:0 10px;
    background:#e1efe4;
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
