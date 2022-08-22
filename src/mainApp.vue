<template>
  <h1>WHO IS IT?</h1>
  <div class="streak">🔥 {{ streak }}</div>
  <div class="nav">
    <a href="./index.html">📠</a>
    <a href="./multipleChoice.html">📷</a>
  </div>

  <div class="game-container" v-if="gameIsActive">
    <img width="300" v-bind:src="currentPersonPath" alt="Picture of face" />
    <form @submit="submitAnswer">
      <input id="name" type="text" autocomplete="off" v-model="inputValue" />
    </form>
    <span class="correct" :class="{ hidden: !showResponse }">
      {{ correctLabel }}
    </span>
    <span class="tries-left" :class="{ hidden: !showTriesLeft }">{{
      triesLeftLabel
    }}</span>
  </div>
  <div class="result-container" v-else>
    <p>✅ Correct guesses: {{ correctGuessesTotal }}</p>
    <p>❌ Wrong guesses: {{ wrongGuessesTotal }}</p>
    <p>🎯 Accuracy: {{ accuracyLabel }}%</p>
    <button type="button" class="reset-button" @click="resetGame">Reset</button>
    <button type="button" class="retry-button" @click="retryGame">Retry</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      people: [
        {
          name: "Amir Ismail",
          path: "./img/IMG_1329.jpeg",
        },
        {
          name: "Markus Piotrowski",
          path: "./img/IMG_1320.jpeg",
        },
        {
          name: "Robin Barwari",
          path: "./img/IMG_1321.jpeg",
        },
        {
          name: "Samuel Broman",
          path: "./img/IMG_1322.jpeg",
        },
        {
          name: "Charlie Nylund",
          path: "./img/IMG_1323.jpeg",
        },
        {
          name: "Mathias Åhlander",
          path: "./img/IMG_1324.jpeg",
        },
        {
          name: "Albin Norback",
          path: "./img/IMG_1325.jpeg",
        },
        {
          name: "Hugo Lindfors",
          path: "./img/IMG_1326.jpeg",
        },
      ],

      availablePeople: [],
      failedPeople: [],
      currentPerson: {},
      correctGuessesTotal: 0,
      wrongGuessesTotal: 0,
      streak: 0,

      triesLeft: 3,

      showResponse: false,
      showTriesLeft: false,
      correctLabel: "✅ Correct",

      inputValue: "",
      gameIsActive: true,
    };
  },
  methods: {
    resetGame() {
      this.availablePeople = [...this.people];
      this.failedPeople = [];
      this.correctGuessesTotal = 0;
      this.wrongGuessesTotal = 0;

      this.setupGame();
    },

    retryGame() {
      if (this.failedPeople.length === 0) {
        alert("no");
        return;
      }

      this.availablePeople = [...this.failedPeople];
      this.failedPeople = [];
      this.showResponse = false;
      this.showTriesLeft = false;

      this.gameIsActive = true;

      this.setupGame();
    },

    setupGame() {
      this.inputValue = "";
      this.triesLeft = 3;
      if (this.availablePeople.length === 0) {
        this.gameIsActive = false;
        return;
      }

      this.currentPerson = this.randomizePerson();
    },
    randomizePerson() {
      let randomIndex = Math.round(
        Math.random() * (this.availablePeople.length - 1)
      );
      return this.availablePeople[randomIndex];
    },

    submitAnswer(e) {
      e.preventDefault();

      this.showResponse = true;
      if (
        this.inputValue.toLowerCase() === this.currentPerson.name.toLowerCase()
      ) {
        this.correctLabel = "✅ Correct";
        this.showTriesLeft = false;

        this.correctGuessesTotal++;
        this.availablePeople.splice(
          this.availablePeople.indexOf(this.currentPerson),
          1
        );
        this.streak++;
        this.setupGame();
      } else {
        this.triesLeft--;
        this.wrongGuessesTotal++;
        this.streak = 0;

        this.correctLabel = "❌ Incorrect";
        this.showTriesLeft = true;

        if (this.triesLeft === 0) {
          this.failedPeople = this.failedPeople.concat(
            this.availablePeople.splice(
              this.availablePeople.indexOf(this.currentPerson),
              1
            )
          );
          this.setupGame();
        }
      }
    },
  },

  computed: {
    currentPersonPath() {
      return this.currentPerson.path;
    },
    triesLeftLabel() {
      if (this.triesLeft === 3) {
        return "Failed";
      } else {
        return "Tries left: " + this.triesLeft;
      }
    },
    accuracyLabel() {
      Math.round(
        100 *
          (this.correctGuessesTotal /
            (this.wrongGuessesTotal + this.correctGuessesTotal))
      );
    },
  },

  mounted() {
    this.resetGame();
  },
};
</script>

<style>
#app {
  display: grid;
  min-height: 100vh;
  width: 100vw;
  place-items: center;
  grid-template-rows: 10vh 1fr;
  margin: 0;
  padding: 0;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
}

h1 {
  font-size: 3rem;
  font-weight: bold;
}

input {
  font-size: 1.5rem;
  width: 300px;
  text-align: center;
}

.game-container {
  display: grid;
  place-items: center;
}

.result-container {
  display: inline;
  place-items: center;
}

input {
  margin: 3rem;
}

.disabled {
  display: none;
}

p {
  font-size: 2rem;
}

.streak {
  position: absolute;
  right: 5%;
  top: 10%;
  font-size: 3rem;
  font-weight: 500;
}

.correct,
.tries-left {
  text-align: center;
  font-size: 1.6rem;
}

a {
  display: block;
}

.nav {
  position: absolute;
  left: 5%;
  top: 10%;
  font-size: 3rem;
  font-weight: 300;
}

a:hover {
  transform: scale(1.1);
}

.hidden {
  visibility: hidden;
}
</style>
