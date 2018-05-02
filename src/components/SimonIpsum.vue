<template>
  <div class="container">
    <h1 class="headline">Simon Ipsum</h1>
    <label class="input">
      <select name="quantity" id="quantity" v-model="quantity" class="input--select">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
        <option value="5">5</option>
      </select> {{ quantity > 1 ? 'Paragraphs' : 'Paragraph' }}
    </label>
    <button class="button" @click="generate">Let's redefine the industry</button>
    <div class="output" v-if="ipsum" v-html="ipsum"></div>
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
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.container {
  padding: 1.875em;
}

.output {
  color: #FFF;
  max-width: 40.000em;
}

.headline {
  font-size: 5em;
  line-height: 1;
  margin-bottom: 0.25em;
}

.input {
  display: block;
  font-weight: 700;
  margin-bottom: 0.5em;
}

.input--select {
  margin-right: 0.625em;
  appearance: none;
  background-color: #FFF;
  background-image: url('/static/img/caret.png');
  background-position: right;
  background-repeat: no-repeat;
  border-color: transparent;
  border-radius: 4px;
  border-style: solid;
  border-width: 1px;
  font-size: 0.750em;
  font-weight: 400;
  color: black;
  padding: 0.625em 0.625em;
  width: 3.750em;

  &:hover {
    cursor: pointer;
  }

  &::-ms-expand {
    display: none;
  }
}

.button {
  appearance: none;
  background-color: #FFF;
  border-radius: 4px;
  border: 0;
  color: #ea1a74;
  cursor: pointer;
  display: inline-block;
  font-family: 'Open Sans', Helvetica, Arial, sans-serif;
  font-size: 0.875em;
  font-weight: 700;
  line-height: 1;
  margin: 1em 0;
  max-height: 3.500em;
  padding: 0.625em 3.333em;
  text-align: center;
  text-decoration: none;
  text-transform: uppercase;
  transition: background-color 0.2s ease-in;
  user-select: none;
  vertical-align: middle;
  white-space: nowrap;

  &:hover {
      background-color: darken(#FFF, 3%);
      text-decoration: none;
  }

  &:active {
      background-color: darken(#FFF, 5%);
  }
}
</style>
