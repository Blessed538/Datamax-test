<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App" :data="values" />
  </div>
</template>

<script>
import HelloWorld from './components/Table.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
  data() {
    return {
      values: [],
      errors: '',
      currrentPage: 1,
    };
  },

  mounted() {
    console.log(this.values);
    fetch(
      `https://www.anapioficeandfire.com/api/books/?page=${this.currrentPage}&pageSize=12`
    )
      .then((res) => res.json())
      .then((result) => (this.values = result))
      .catch((error) => {
        this.errors = error;
      });
  },
};
</script>

<style>
#app {
  padding: 10px 40px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
</style>
