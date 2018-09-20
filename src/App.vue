<template>
  <div id="app">
    <TweetForm/>
    <TweetsTable/>
  </div>
</template>

<script>
import TweetForm from './components/TweetForm'
import TweetsTable from './components/TweetsTable'

export default {
  name: 'App',
  components: {
    TweetsTable,
    TweetForm
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
