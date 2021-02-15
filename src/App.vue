<template>
  <div id="app">
    <h1 class="text-3xl">SEARCH HERE...</h1>
    <p>{{data.length}} results</p>
    <div class="pt-2 relative mx-auto text-gray-600">
      <input class="border-2 border-gray-300 bg-white h-10 px-5 pr-16 rounded-lg text-sm focus:outline-none"
             type="search" name="search" placeholder="Search" @keyup="getData()" v-model="query">
      <button type="submit" class="absolute right-0 top-0 mt-5 mr-4">
      </button>
    </div>
    <div v-for="entry in data" v-bind:key="entry" class="mt-6 flex items-center justify-center">
      <div class="text-left w-96">
        <p class="text-gray-400">{{formatLink(entry.Link)}}</p>
        <a :href="entry.Link" style="color: #1a0dab">{{entry.API}}</a>
        <p class="text-gray-600">{{entry.Category}} - {{entry.Description}}</p>
      </div>
    </div>
  </div>
</template>

<script>
  const axios = require('axios');

export default {
  name: 'App',
  data: function () {
    return {
      data: [],
      query: '',
    }
  },
  methods: {
    getData: function () {
      const _self = this;
      // https://api.publicapis.org/entries?Category=
      axios.get('http://localhost:3000/items/' + this.query).then(res => {
        _self.data = res.data.entries;
      })
    },
    formatLink: function (link) {
      let retString = link.split('/')[2];
      for (let i =3; i < link.split('/').length - 1; i++) {
        retString += ' > ' + link.split('/')[i]
      }
      return retString;
    }
  },
  mounted() {
    this.getData();
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
