<script setup>
import ResultsTable from "./components/ResultsTable.vue";
import NavBox from "./components/NavBox.vue";
</script>

<template>
  <h1>WHO IS IT?</h1>
  <div class="streak-container">🔥 {{ streak }}</div>
  <NavBox />

  <div class="game-container" v-if="gameIsActive">
    <span class="question-label">Click on:</span>
    <span class="name-label">{{ currentPerson.name }}</span>
    <span class="correct" :class="{ hidden: !showResponse }">
      {{ correctLabel }}
    </span>
    <span class="tries-left" :class="{ hidden: !showTriesLeft }">{{
      triesLeftLabel
    }}</span>
    <div class="image-container">
      <img
        v-for="(person, index) in people"
        class="face-image"
        :class="['index-' + index]"
        width="150"
        v-bind:src="person.path"
        alt="Picture of face"
        @click="submitAnswer"
      />
    </div>
  </div>

  <ResultsTable
    v-else
    :correct-guesses="correctGuessesTotal"
    :wrong-guesses="wrongGuessesTotal"
    @reset="resetGame"
    @retry="retryGame"
  />
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
      nameLabel: "",

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
      this.showResponse = false;
      this.showTriesLeft = false;
      this.gameIsActive = true;

      this.setupRound();
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

      this.setupRound();
    },

    setupRound() {
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
      let index = e.target.className[e.target.className.length - 1];

      let answer = this.people[index];

      this.showResponse = true;
      if (answer.name === this.currentPerson.name) {
        this.correctLabel = "✅ Correct";
        this.showTriesLeft = false;

        this.correctGuessesTotal++;
        this.availablePeople.splice(
          this.availablePeople.indexOf(this.currentPerson),
          1
        );
        this.streak++;
        this.setupRound();
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
          this.setupRound();
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

.game-container {
  display: grid;
  grid-template-rows: auto auto auto auto 1fr;
  height: 100%;
  width: 100%;
  /* place-items: center; */
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

.streak-container {
  position: absolute;
  right: 5%;
  top: 10%;
  font-size: 3rem;
  font-weight: 500;
}

.question-label,
.name-label {
  margin: 0;
  font-size: 2rem;
  text-align: center;
  font-weight: bold;
}

.question-label {
  margin-top: 10vh;
}

.name-label {
  margin-bottom: 20vh;
}

.correct,
.tries-left {
  text-align: center;
  font-size: 1.6rem;
}

.image-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
}

.face-image {
  margin: 0.4rem;
}

.face-image:hover {
  cursor: pointer;
  transform: scale(1.1);
}

a:hover {
  transform: scale(1.1);
}

.hidden {
  visibility: hidden;
}
</style>
