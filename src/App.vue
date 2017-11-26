<template>
  <div id="app">
    <div class='get-word' v-if='(playing === false) && (gameOver === false) && (won === false)'>
      <h1>Hangman</h1>

      <p>Type a word</p>
      <input type='text' v-model='word' />
      <button v-on:click='save'>Save</button>
    </div>

    <div v-if='playing === true'>
      <div>
        <img v-if='incorrectGuesses === 0' src="./assets/image0.jpg">
        <img v-if='incorrectGuesses === 1' src="./assets/image1.jpg">
        <img v-if='incorrectGuesses === 2' src="./assets/image2.jpg">
        <img v-if='incorrectGuesses === 3' src="./assets/image3.jpg">
        <img v-if='incorrectGuesses === 4' src="./assets/image4.jpg">
        <img v-if='incorrectGuesses === 5' src="./assets/image5.jpg">
        <img v-if='incorrectGuesses === 6' src="./assets/image6.jpg">
        <img v-if='incorrectGuesses === 7' src="./assets/image7.jpg">
        <img v-if='incorrectGuesses === 8' src="./assets/image8.jpg">
        <img v-if='incorrectGuesses === 9' src="./assets/image9.jpg">
        <img v-if='incorrectGuesses === 10' src="./assets/image10.jpg">
        <img v-if='incorrectGuesses === 11' src="./assets/image11.jpg">
      </div>

      <ul style='margin-bottom: 20px;'>
        <li v-for='letter in wordArray' :key='letter.correctValue'>{{letter.value}}</li>
      </ul>

      <div>
        <input type='text' v-model='guess' />
        <button v-on:click='makeGuess'>Make Guess</button>

        <div>
          <h2>Guessed Letters</h2>
          <ul class='letters'>
            <li v-for='letter in letters' :key='letter.letter'>
              <div style='text-decoration: line-through' v-if='letter.guessed'>{{ letter.letter }}</div>
              <div v-else>{{ letter.letter }}</div>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div v-if='gameOver === true'>
      <h1>You lost!</h1>
      <p>The word was {{ word }}.</p>
      <button v-on:click='restart'>Restart</button>
    </div>

    <div v-if='won === true'>
      <h1>You won!</h1>
      <p>The word was {{ word }}.</p>
      <button v-on:click='restart'>Restart</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  computed: {
    imgSrc () {
      return `./assets/image${this.incorrectGuesses}.jpg`
    }
  },
  data () {
    return {
      word: '',
      wordArray: [],
      playing: false,
      guess: '',
      incorrectGuesses: 0,
      maxIncorrectGuesses: 11,
      gameOver: false,
      won: false,
      letters: [
        { letter: 'a', guessed: false },
        { letter: 'b', guessed: false },
        { letter: 'c', guessed: false },
        { letter: 'd', guessed: false },
        { letter: 'e', guessed: false },
        { letter: 'f', guessed: false },
        { letter: 'g', guessed: false },
        { letter: 'h', guessed: false },
        { letter: 'i', guessed: false },
        { letter: 'j', guessed: false },
        { letter: 'k', guessed: false },
        { letter: 'l', guessed: false },
        { letter: 'm', guessed: false },
        { letter: 'n', guessed: false },
        { letter: 'o', guessed: false },
        { letter: 'p', guessed: false },
        { letter: 'q', guessed: false },
        { letter: 'r', guessed: false },
        { letter: 's', guessed: false },
        { letter: 't', guessed: false },
        { letter: 'u', guessed: false },
        { letter: 'v', guessed: false },
        { letter: 'w', guessed: false },
        { letter: 'x', guessed: false },
        { letter: 'y', guessed: false },
        { letter: 'z', guessed: false }
      ]
    }
  },
  methods: {
    restart () {
      this.word = ''
      this.wordArray = []
      this.playing = false
      this.guess = ''
      this.incorrectGuesses = 0
      this.gameOver = false
      this.won = false
      this.letters = [
        { letter: 'a', guessed: false },
        { letter: 'b', guessed: false },
        { letter: 'c', guessed: false },
        { letter: 'd', guessed: false },
        { letter: 'e', guessed: false },
        { letter: 'f', guessed: false },
        { letter: 'g', guessed: false },
        { letter: 'h', guessed: false },
        { letter: 'i', guessed: false },
        { letter: 'j', guessed: false },
        { letter: 'k', guessed: false },
        { letter: 'l', guessed: false },
        { letter: 'm', guessed: false },
        { letter: 'n', guessed: false },
        { letter: 'o', guessed: false },
        { letter: 'p', guessed: false },
        { letter: 'q', guessed: false },
        { letter: 'r', guessed: false },
        { letter: 's', guessed: false },
        { letter: 't', guessed: false },
        { letter: 'u', guessed: false },
        { letter: 'v', guessed: false },
        { letter: 'w', guessed: false },
        { letter: 'x', guessed: false },
        { letter: 'y', guessed: false },
        { letter: 'z', guessed: false }
      ]
    },
    save () {
      const wordArray = this.word.split('')
      this.wordArray = wordArray.reduce((result, item) => {
        return [...result, {
          correctValue: item.toLowerCase(),
          value: '_'
        }]
      }, {})
      this.playing = true
    },
    checkIfAlreadyGuessed (guessedLetter) {
      const foundLetter = this.letters.find((item) => {
        return (item.letter.toLowerCase() === guessedLetter.toLowerCase())
      })

      return (foundLetter.guessed === true)
    },
    setToAlreadyGuessed (guessedLetter) {
      this.letters = this.letters.reduce((result, item) => {
        if (item.letter === guessedLetter) {
          item.guessed = true
        }

        return [...result, item]
      }, [])
    },
    checkIfGameOver() {
      if (this.incorrectGuesses === this.maxIncorrectGuesses) {
        this.gameOver = true
        this.playing = false
      }
    },
    checkIfWon() {
      const amountCorrect = this.wordArray.reduce((result, item) => {
        if (item.guessed === true) {
          return result + 1
        }

        return result
      }, 0)

      this.won = (amountCorrect === this.word.length)
      this.playing = !this.won
    },
    makeGuess () {
      for (var i = 0; i < this.guess.length; i++) {
        const guessedLetter = this.guess[i].toLowerCase()
        let alreadyGuessed = this.checkIfAlreadyGuessed(guessedLetter)

        if (alreadyGuessed === false) {
          let isCorrect = false

          for (var k = 0; k < this.wordArray.length; k++) {
            const correctLetter = this.wordArray[k]
            if (guessedLetter === correctLetter.correctValue) {
              isCorrect = true
              correctLetter.value = correctLetter.correctValue
              correctLetter.guessed = true
            }
          }

          if (isCorrect) {
            this.checkIfWon()
          } else {
            this.incorrectGuesses++
            this.checkIfGameOver()
          }

          this.setToAlreadyGuessed(guessedLetter)
        }
      }

      this.guess = ''
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Merriweather');

  body {
    background: #000;
    color: #fff;
    font-size: 20px;
    font-family: 'Merriweather', serif;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: block;
  }

  li {
    display: inline-block;
    padding-right: 5px;
  }

  #app {
    width: 400px;
    margin: 0 auto;
  }

  input {
    padding: 10px;
    border-radius: 4px;
    border: 0;
    color: #000;
    width: 380px;
    font-size: 20px;
    font-family: 'Merriweather', serif;
  }

  button {
    padding: 10px;
    width: 400px;
    background: pink;
    margin-top: 20px;
    border: 0;
    font-size: 20px;
    font-family: 'Merriweather', serif;
  }

  .letters {
    font-size: 18px;
  }
</style>