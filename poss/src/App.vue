<template>
  <div id="app">
    
      <leftNav></leftNav>
    <div class="main">
      <router-view/>
    </div>
    
    
  </div>
</template>

<script>
import leftNav from './components/leftNav'

export default {
  components:{
    leftNav
  }

}

</script>

<style>
*{
  padding: 0px;
  margin: 0px;
}
html,body,#app{
  height: 100%;
}
.main{
  float: left;
  width: 95%;
  background-color: #EFF2F7;
  height: 100%;
  overflow: hidden;
}
</style>
