<template>
  <div class="luisdel">
    <h1>{{ msg }}</h1>
    <h1>{{ reversedMessage }}</h1>
    <a :href="getLink"> Fake news link </a>
    <h1></h1>
    <input v-model="name">
    <h1>{{ result }}</h1>
  </div>
</template>

<script>
import * as _ from 'lodash'
import axios from 'axios'

export default {
  name: 'LuisDel',
  props: {
    msg: String
  },
  data: () => {
    return {
      name: null,
      result: null
    }
  },
  watch: {
    name: function () {
      this.result = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  },
  computed: {
    reversedMessage: function() {
      return this.msg.split('').reverse().join('')
    },
    getLink: function () {
      return 'https://www.google.ch'
    }
  },
  created: function() {
    this.debouncedGetAnswer = _.debounce(this.getResult, 500)
  },
  methods: {
    getResult: function () {
      this.result = 'Thinking...'
      var that = this
      axios.get('https://yesno.wtf/api')
        .then(response => {
          that.result = _.capitalize(response.data.answer)
        })
        .catch(error => {
          that.answer = 'Error! Could not reach the API ' + error
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
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
