<template>
  <div id="app">
    <div id="cover"></div>
     <myHeader></myHeader>
    <!--<router-view></router-view>-->
    <todo><</todo>
  </div>
</template>


<script>
import myHeader from './components/header.vue'
import todo from './components/todo.vue'
export default {
  components:{
    myHeader,
    todo
  }
};
</script>

<style lang="stylus">
  #app{
    position  absolute
    left 0
    right 0
    top 0
    bottom 0
  }
  #cover{
    min-height 1000px
    position absolute
    left 0
    right 0
    top 0
    bottom 0
    background-color #c7cdcc
    opacity 0.9
    z-index -1
  }
</style>
