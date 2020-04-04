<template>
  <div class="SightWords">

    <header>
      <LevelNavigation :items="wordLevels"
                      :data="wordData"
                      :level="level"
                      @levelChange="changeLevel"
      ></LevelNavigation>
    </header>


    <main class="Stage">
      <button type="button"
              :style="{fontSize: activeWordFontSize+ 'vw'}"
              @click="announce(activeWord)"
              class="ActiveWord Stage-word"
      >
        {{ activeWord }}
      </button>
    </main>

    <ul class="WordList">
      <li v-for="word in currentWordsLevel.words">
        <button type="button"
                @click="announce(word)"
                :class="{'is-active': word === activeWord }"
        >
          {{ word }}
        </button>

      </li>
    </ul>
  </div>
</template>

<script>
import wordData from "../settings/words.json";
import LevelNavigation from "./LevelNavigation";

export default {
  name: "HelloWords",
  components: {
    LevelNavigation
  },
  props: {
    level: {
      default: 1,
      type: Number
    }
  },
  data() {
    return {
      wordData: wordData,
      activeWord: null,
      activeWordFontSize: 5,
      activeLevel: this.level
    };
  },
  computed: {
    // a computed getter
    currentWordsLevel: function(level) {
      // `this` points to the vm instance
      return this.wordData["level" + this.level];
    },
    wordLevels: function() {
      let keyArray = [];
      for (var key in wordData) {
        keyArray.push(key);
      }

      return keyArray;
    }
  },
  mounted: function() {
    let vm = this;
    vm.randomWord();

    window.utterance = new SpeechSynthesisUtterance();
    window.utterance.rate = 0.85;

    window.utterance.addEventListener("end", function() {
      //console.log("hello");
      // vm.randomWord();
    });
  },
  methods: {
    announce: function(word) {
      window.utterance.text = word.toLowerCase();
      speechSynthesis.speak(utterance);

      this.randomWord(word);
      console.log("just said: " + word);
    },
    randomWord: function(previousWord) {
      this.activeWord = this.currentWordsLevel.words[
        Math.floor(Math.random() * this.currentWordsLevel.words.length)
			];

			// re-random word
			if (this.activeWord === previousWord){
				this.randomWord(previousWord);
				return true;
			}

      this.activeWordFontSize =
        this.activeWord.length > 2 ? 100 / this.activeWord.length : 30;
    },
    changeLevel: function(value) {
      this.activeLeveL = value;
      this.level = value;
      this.randomWord();
    }
  }
};
</script>

<style lang="scss" scoped>
.SightWords {
  /* background-color: red; */
  height: 100%;
  display: flex;
  flex-direction: column;
}

.Level {
  display: flex;
  width: 100%;
  justify-content: space-around;

  &-button {
    padding: 1em;
    border-radius: 0.8em;
  }
}
.Stage {
  flex: 1;
  display: flex;
  text-align: center;
  /* background-color: orange; */
  &-word {
    margin: auto;
  }
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

button {
  background-color: transparent;
  border: none;
  transition: 200ms transform ease-out;

  &:active {
    transform: scale(0.95);
  }

  &:focus {
    outline: none;
  }
}

.is-active {
  background-color: #fff;
  box-shadow: 0 2px 2px rgba(0, 0, 0, 0.25) inset;
}
</style>
