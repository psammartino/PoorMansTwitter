<template>
  <table class="table">
    <thead >
    <tr>
      <th v-for="fieldname in fieldnames" :key="fieldname" v-on:click="sortBy(fieldname)">
        {{ fieldname | capitalize }}
      </th>
    </tr>
    </thead>
    <tr v-for="tweet in orderedTweets" :key="tweet.id">
      <td>{{ tweet.name }}</td>
      <td>{{ tweet.content }}</td>
      <td>{{ new Date(tweet.created).toLocaleString() }}</td>
    </tr>
  </table>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'
import { EventBus } from '../event-bus'
import config from '../config'
const API_ROOT = config.API_ROOT

export default {
  name: 'TweetsTable',
  data: function () {
    return {
      tweets: [],
      sortKey: '',
      reverse: true,
      fieldnames: ['name', 'content', 'time']
    }
  },
  methods: {
    sortBy: function (sortKey) {
      if (sortKey !== 'content') {
        this.reverse = (this.sortKey === sortKey) ? !this.reverse : false
        this.sortKey = sortKey
      }
    },
    getTweets: function () {
      axios
        .get(API_ROOT + '/tweets')
        .then(res => {
          this.tweets = res.data
          this.sortKey = 'time'
        },
        err => {
          console.log(err)
          alert('Something went wrong, cannot reach the server')
        })
    }
  },
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  },
  computed: {
    orderedTweets: function () {
      if (this.reverse) {
        return _.orderBy(this.tweets, this.sortKey, this.reverse).reverse()
      }
      return _.orderBy(this.tweets, this.sortKey, this.reverse)
    }
  },
  created () {
    EventBus.$on('newTweet', tweet => {
      this.tweets.push(tweet)
    })
  },
  beforeMount () {
    this.getTweets()
  }
}
</script>

<style scoped>

</style>
