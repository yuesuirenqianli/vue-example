<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <br>
    <ul>
      <li v-for="componentName in componentList" :key="componentName">
        <a href="javascript:;" @click="activateComponents = componentName">{{componentName}}</a>
      </li>
    </ul>
    <components :is="activateComponents" msg="Welcome to Your Vue.js App"></components>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import VirtualList from "./components/VirtualList.vue";

export default {
  name: 'App',
  components: {
    HelloWorld,
    VirtualList
  },
  data() {
    return {
      activateComponents: VirtualList.name,
    }
  },
  computed: {
    componentList() {
      return Object.values(JSON.parse(JSON.stringify(this.$options.components))).map(i => i.name).filter(Boolean)
    }
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
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
