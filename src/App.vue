<template>
  <div id="app">
    <HeaderPreview msg="Template Generator"/>
    <ConfigForm/>
  </div>
</template>

<script>
import ConfigForm from './components/Form/Form.vue'
import HeaderPreview from './components/Header/Header.vue'

export default {
  name: 'App',
  components: {
    ConfigForm,
    HeaderPreview
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
