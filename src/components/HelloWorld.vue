<template>
  <div class="hello">
    <img alt="Vue logo" src="../assets/logo.png" width="80">
    <h3>A pull-down refresh and pull-up loaded Vue component based on IScroll5.</h3>
    <v-iscroll>
      <ul>
        <li><a href="https://router.vuejs.org" target="_blank" rel="noopener">vue-router</a></li>
        <li><a href="https://vuex.vuejs.org" target="_blank" rel="noopener">vuex</a></li>
        <li><a href="https://github.com/vuejs/vue-devtools#vue-devtools" target="_blank" rel="noopener">vue-devtools</a></li>
        <li><a href="https://vue-loader.vuejs.org" target="_blank" rel="noopener">vue-loader</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
        <li><a href="https://github.com/vuejs/awesome-vue" target="_blank" rel="noopener">awesome-vue</a></li>
      </ul>
    </v-iscroll>
  </div>
</template>

<script>
import VIscroll from './VIscroll.vue'
export default {
  name: 'HelloWorld',
  components: {
    VIscroll
  },
  props: {
    msg: String
  }
}
</script>

<style scoped>
img {
  margin-top: 60px;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 0 10px;
  height: 100px;
  background-color: coral;
  border-top: 1px solid #ccc;
}
a {
  color: #42b983;
}
</style>
