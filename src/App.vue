<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/lion-200.jpg">
    <MyUserTable msg="Welcome to My App"/>
  </div>
</template>

<script>
  import MyUserTable from './components/MyUserTable.vue'

  export default {
    name: 'app',
    components: {
      // HelloWorld
      MyUserTable
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
