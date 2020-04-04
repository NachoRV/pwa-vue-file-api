<template>
  <div id="app">
    <router-view />
    <div>
      <p v-html="html"></p>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      msg: 'hola',
      html: ''
    }
  },
  created() {
    this.msg = this.fetchData()
  },
  methods: {
    fetchData() {
      fetch('http://localhost:8080/?rest_route=/wp/v2/posts/1')
        .then(response => {
          console.log(response)
          return response.json()
        })
        .then(Myjson => {
          console.log(Myjson.content.rendered)
          this.html = Myjson.content.rendered
        })
    }
  }
}
</script>
<style lang="scss">
$theme-color: '#4DBA87';
body {
  margin: 0px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
