<template>
  <v-iscroll class="v-iscroll" v-model="refresh" @refresh="onRefresh" up>
    <img src="../assets/logo.png" width="80">
    <h3>A pull-down refresh and pull-up loaded Vue component based on IScroll5.</h3>
    <ul>
      <li><a>vue-router</a></li>
      <li><a>vuex</a></li>
      <li><a>vue-devtools</a></li>
      <li><a>vue-loader</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
      <li><a>awesome-vue</a></li>
    </ul>
  </v-iscroll>
</template>

<script>
import VIscroll from './VIscroll.vue'
export default {
  name: 'HelloWorld',
  components: {
    VIscroll
  },
  data() {
    return { 
      refresh: true
    }
  },
  methods: {
    onRefresh() {
      setTimeout(() => {
        this.refresh = false
      }, 2000)
    }
  }
}
</script>

<style lang="scss" scoped>
.v-iscroll {
  text-align: center;
  height: 100vh !important;
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
