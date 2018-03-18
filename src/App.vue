<template>
  <div id="app" v-cloak>
    <select @change="selectTimezone($event.target.value)">
      <option v-for="city in cities" :value="city">{{city}}</option>
    </select>
    <ez-clock></ez-clock>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const modClock = {
    namespaced:true,
    state:{time:Date.now()},
    getters:{
      time_lts(state,getters,rootState,rootGetters){
        return moment(state.time).tz(rootState.timezone).format('LTS')
      }
    },
    mutations:{SET_TIME(state,val){state.time = val}},
    actions:{setTime(context,val){context.commit('SET_TIME',val)}}
  }

  const store = new Vuex.Store({
    state:{
      cities:['Asia/Shanghai','Australia/Melbourne','Africa/Cairo','America/Denver','Europe/London'],
      timezone:'Asia/Shanghai'
    },
    getters:{
      city: state => state.timezone.split('/')[1]
    },
    mutations:{
      SELECT_TIMEZONE: (state,city) => state.timezone = city
    },
    actions:{
      selectTimezone: (context,city) => context.commit('SELECT_TIMEZONE',city)
    },
    modules:{
      m2: modClock
    }
  });

  const EzClock = {
    template:`
  	<div class="clock">
      <div class="time">{{time}}</div>
      <div class="tool">
        <a href="#" @click.prevent>{{city}}</a>
      </div>
    </div>
  `,
    computed:{
      ...Vuex.mapGetters('m2',{time:'time_lts'}),
      ...Vuex.mapGetters(['city'])
    },
    methods:Vuex.mapActions('m2',['setTime']),
    created(){
      setInterval(()=>this.setTime(Date.now()),1000)
    }
  }

  export default {
    name: 'App',
    store:store,
    computed:Vuex.mapState(['cities','timezone']),
    methods:Vuex.mapActions(['selectTimezone']),
    components:{EzClock}
  }

</script>

<style>
  .counter,.clock{
    font-family:LED;
    font-size:80px;
    padding:10px;
  }
  .clock > * {
    float:left;
  }
  .clock .tool{
    margin-left:20px;
  }
  .clock .tool a{
    text-decoration:none;
    font-size:20px;
    background:#333;
    color:#fff;
    padding:2px 10px;
    border-radius:10px;
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
