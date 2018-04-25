<template>
  <div class="hello">
    <h1>Simon Ipsum</h1>
    <div class="input">
      <select name="quantity" id="quantity" v-model="quantity">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
      </select> {{ quantity > 1 ? 'Paragraphs' : 'Paragraph' }}
    </div>
    <button @click="generate">Let's redefine the industry</button>
    <div class="body" v-if="ipsum" v-html="ipsum"></div>
  </div>
</template>

<script>
import _words from '../data/words'

export default {
  name: 'SimonIpsum',
  data () {
    return {
      quantity: 1,
      ipsum: ''
    }
  },
  methods: {
    randomize (x, y) {
      let rnd = (Math.random() * 2 - 1) + (Math.random() * 2 - 1) + (Math.random() * 2 - 1)
      return Math.round(Math.abs(rnd) * x + y)
    },
    count (min, max) {
      let result
      if (min && max) result = Math.floor(Math.random() * (max - min + 1) + min)
      else if (min) result = min
      else if (max) result = max
      else result = this.randomize(8, 2)

      return result
    },
    words (min, max) {
      let result = []
      let count = this.count(min, max)

      // get random words
      while (result.length < count) {
        let pos = Math.floor(Math.random() * _words.length)
        let rnd = _words[pos]

        // do not allow same word twice in a row
        if (result.length && result[result.length - 1] === rnd) {
          continue
        }

        result.push(rnd)
      }

      return result
    },
    sentence (min, max) {
      let words = this.words(min, max)

      // add comma(s) to sentence
      let index = this.randomize(6, 2)
      while (index < words.length - 2) {
        words[index] += ','
        index += this.randomize(6, 2)
      }

      // append puctation on end
      let punct = '...!?'
      words[words.length - 1] += punct.charAt(Math.floor(Math.random() * punct.length))

      // uppercase first letter
      words[0] = words[0].charAt(0).toUpperCase() + words[0].slice(1)

      return words.join(' ')
    },
    paragraph (min, max) {
      if (!min && !max) {
        min = 20
        max = 60
      }

      let result = ''
      let count = this.count(min, max)

      // append sentences until limit is reached
      while (result.slice(0, -1).split(' ').length < count) {
        result += this.sentence() + ' '
      }
      result = result.slice(0, -1)

      // remove words
      if (result.split(' ').length > count) {
        let punct = result.slice(-1)
        result = result.split(' ').slice(0, count).join(' ')
        result = result.replace(/,$/, '')
        result += punct
      }

      return result
    },
    generate () {
      this.ipsum = ''
      let quantity = this.quantity
      for (let i = 0; i < quantity; i++) {
        this.ipsum += '<p>' + this.paragraph() + '</p>'
      }
    }
  },
  mounted () {
    // console.log(this.paragraph())
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
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
  color: #EB1B75;
}
.input {
  padding: 20px;
}
.body {
  background-color: #F1F0F0;
  margin: 20px auto;
  max-width: 40.000em;
  padding: 20px;
  text-align: left;
}
</style>
