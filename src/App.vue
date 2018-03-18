<template>
  <div id="app" v-cloak>
    <button @click="reset">RESET</button>
    <div class="counter">{{counter}}</div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data:function () {
    return{
      counter: 0
    }
  },
  methods:{
    reset(){
      this.counter = 0;
    }
  },
  created(){
    setInterval(()=>this.counter++,100);
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
