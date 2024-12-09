<template v-if="question">
  <h1 v-html="question"></h1>

  <template v-for="(answer, index) in answers" :key="index">
    <input type="radio" name="options" :id="answer" value="answer" />
    <label :for="answer" v-html="answer"></label>
    <br />
  </template>

  <button class="send" type="button">Send</button>
</template>

<script lang="ts">
import { defineComponent } from "vue";

const apiUrl = "https://opentdb.com/api.php?amount=1&category=18";

export default defineComponent({
  name: "App",
  data() {
    return {
      question: "",
      incorrectAnswers: [],
      correctAnswers: "",
    };
  },
  computed: {
    answers() {
      let answers: any = [];
      if (this.incorrectAnswers && this.correctAnswers) {
        answers = [...this.incorrectAnswers];
        answers.splice(
          Math.round(Math.random() * answers.length),
          0,
          this.correctAnswers
        );
      }
      return answers;
    },
  },
  created() {
    this.axios.get(apiUrl).then((res) => {
      console.log(res.data.results[0]);
      this.question = res.data.results[0].question;
      this.incorrectAnswers = res.data.results[0].incorrect_answers;
      this.correctAnswers = res.data.results[0].correct_answer;
    });
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;
}

h1 {
  margin-top: 40px;
}

input[type="radio"] {
  margin: 12px 4px;
}

button.send {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  cursor: pointer;
}

section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;

  span {
    padding: 8px;
    font-weight: bold;
    border: 1px solid black;
  }
}
</style>
