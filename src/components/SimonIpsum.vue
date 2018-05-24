<template>
  <section class="container">
    <header class="headline">
      <h1>Simon Ipsum</h1>
    </header>
    <div class="controls">
      <label for="quantity" class="label box">
        Are you ready to redefine the industry?
      </label>
      <div class="input box">
        <input type="number" id="quantity" class="quantity" min="1" max="20" v-model="quantity">
      </div>
      <div class="action box">
        <button class="button" @click="generate">{{ quantity > 1 ? 'Paragraphs' : 'Paragraph' }}</button>
      </div>
    </div>
    <div class="output" v-if="ipsum" v-html="ipsum"></div>
  </section>
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
.headline {
  margin-bottom: 4em;

  h1 {
    font-family: 'Montserrat', sans-serif;
    font-size: 4.000em;
    font-weight: 900;
    line-height: 1;
    letter-spacing: 0.5em;
    margin: 0;
    text-align: center;
    text-transform: uppercase;

    @media (max-width: 50em) {
      font-size: 3.000em;
    }
  }
}

.controls {
  align-items: center;
  justify-content: center;
  display: flex;
  flex: 0;
  margin: 0 auto;
  max-width: 50em;

  @media (max-width: 50em) {
    flex-direction: column;
  }

  .box {
    padding: 0.625em;
  }

  .label {
    flex: 0 0 27.273em;
    font-size: 0.688em;
    font-weight: 800;
    letter-spacing: 0.500em;
    text-transform: uppercase;

      @media (max-width: 50em) {
        flex-basis: auto;
        text-align: center;
      }
  }

  .input {
    .quantity {
      background-color: transparent;
      border: none;
      color: transparent;
      flex: 1;
      font-family: 'Open Sans', sans-serif;
      font-size: 8.438em;
      height: 1em;
      line-height: 0.75;
      text-align: center;
      text-shadow: 0 0 0 #FFF;
      width: 100%;

      &:focus {
        outline: none;
      }
    }
  }

  .action {
    flex: 0 0 18.750em;

    @media (max-width: 50em) {
      flex-basis: auto;
      width: 18.750em;
    }

    .button {
      appearance: none;
      background-color: darken(#C5206A, 3%);
      border-radius: 3px;
      border: 0;
      color: #FFF;
      cursor: pointer;
      display: inline-block;
      font-family: 'Open Sans', sans-serif;
      font-size: 0.688em;
      font-weight: 800;
      letter-spacing: 0.500em;
      line-height: 1;
      margin: 1em 0;
      padding: 1.5em 1em;
      text-align: center;
      text-decoration: none;
      text-transform: uppercase;
      transition: background-color 0.2s ease-in;
      user-select: none;
      vertical-align: middle;
      white-space: nowrap;
      width: 100%;

      &:hover {
          background-color: darken(#C5206A, 5%);
          text-decoration: none;
      }

      &:active {
          background-color: darken(#FFF, 5%);
      }
    }
  }
}

.output {
  color: #FFF;
  margin: 0 auto;
  max-width: 40.000em;
}
</style>
