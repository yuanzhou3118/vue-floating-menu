<template>
  <div id="app">
    <floatBtn />
  </div>
</template>

<script>
import floatBtn from './components/floatBtn.vue';

export default {
  name: 'App',
  components: {
    floatBtn,
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  li {
    list-style: none;
  }
}
a {
  text-decoration: none;
}
html,
body {
  height: 100%;
  .select-option {
    display: none !important;
  }
}
#app {
  font-family: 'SourceHanSansCN-Medium', 'SourceHanSansCN', 'Avenir', Helvetica,
    Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  color: #2c3e50;
  font-size: 14px;
  text-align: left;
  overflow: hidden;
  height: 100vh;
  background: #1a237e;
}
</style>
