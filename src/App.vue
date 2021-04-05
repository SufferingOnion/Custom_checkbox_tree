<template>
  <div id="app">
    <vue-form  @senddata="acceptData"/>
    <p>
      {{ checkboxData }}
    </p>
  </div>
</template>

<script>
import VueForm from './components/VueForm.vue'

export default {
  name: 'App',
  data(){
    return {
      checkboxData: null,
    }
  },
  methods: {
    acceptData(payload){
      this.checkboxData = payload
    }
  },
  components: {
    'vue-form': VueForm
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
