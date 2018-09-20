<template>
  <div class="card" style="width: 18rem;">
    <div class="row card-body">
      <form v-on:submit.prevent="onSubmit">
        <div class="form-group">
          <input v-model="username" id="username-input" placeholder="Username" type="text" class="form-control" aria-label="Username">
        </div>
        <div class="form-group">
          <textarea v-model="tweetContent" @keyup="setCharactersLeft" class="form-control" rows="3" placeholder="What's happening?"></textarea>
        </div>
        <label class="characters-left float-left">{{ charactersLeft }}</label>
        <button :disabled="!isValid" type="submit" class="btn btn-primary float-right">Tweet</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { EventBus } from '../event-bus'
import config from '../config'
const API_ROOT = config.API_ROOT

export default {
  name: 'TweetForm',
  state: {
  },
  data: function () {
    return {
      charactersLeft: 50,
      tweetContent: '',
      username: ''
    }
  },
  methods: {
    postTweet: function () {
      const tweet = {
        name: this.username,
        content: this.tweetContent
      }
      return axios.post(API_ROOT + '/tweets/', tweet)
    },
    onSubmit: function (e) {
      e.preventDefault()
      this.postTweet()
        .then(res => {
          EventBus.$emit('newTweet', res.data)
          this.tweetContent = ''
          this.username = ''
          this.charactersLeft = 50
        },
        err => { console.log(err) })
    },
    setCharactersLeft: function () {
      const charLimit = 50
      this.charactersLeft = charLimit - this.tweetContent.length
    }
  },
  computed: {
    isValid: function () {
      return (this.username.length > 0 &&
                                  this.tweetContent.length > 0 &&
                                  this.charactersLeft >= 0)
    }
  }
}
</script>

<style scoped>
  textarea{
    resize: none;
    width: 100%;
  }

  form{
    width: 100%;
  }

  .card{
    width: 24rem !important;
    margin: 50px auto;
    padding: 0 15px;
  }
</style>
