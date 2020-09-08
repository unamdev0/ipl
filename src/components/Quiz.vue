<template>
  <div class="container">
    <div class="container-quiz">
      <div class="quiz-header">
        <h1>Welcome</h1>
      </div>
      <div class="main-quiz" v-show="!show_marks">
        <div class="question-box">
          <h1>{{questions[questionIndex].question}}</h1>
        </div>
        <div class="options">
          <ul>
            <li
              v-for="ans in questions[questionIndex].answers"
              :key="ans.option"
              @click="selectComponent(ans,$event)"
            >{{ans.option}}</li>
          </ul>
        </div>
      </div>
      <div class="main-quiz" v-show="show_marks">
        <h1>You Scored:{{marks}}/10</h1>
      </div>

      <div class="footer-quiz">
        <button v-if="!show_marks" @click="finishQuiz()">Finish</button>
        <button v-if="!show_marks" @click="nextQuestion()">Next</button>
        <button v-if="show_marks" @click="startQuiz()">Restart</button>
      </div>
    </div>
  </div>
</template>

<script>
import json from "../assets/data.json";
export default {
  name: "Quiz",
  data() {
    return {
      questions: json.questions,
      questionIndex: Math.floor(Math.random() * 15),
      component: null,
      selected: false,
      marks: 0,
      show_marks: false,
      counter: 0,
    };
  },
  methods: {
    selectComponent(component, event) {
      if (this.selected) {
        this.selected.classList.remove("select");
      }
      this.selected = event.target;
      this.selected.classList.add("select");
      this.component = component;
    },
    nextQuestion() {
      if (this.counter >= 10) {
        this.finishQuiz();
      }
      (this.questionIndex = Math.floor(Math.random() * 19)),
        (this.counter += 1);
      if (this.component != null && this.component.correct) {
        this.marks += 1;
      }
    },
    finishQuiz() {
      this.show_marks = true;
      if (this.component != null && this.component.correct) {
        this.marks += 1;
      }
      this.selected = null;
    },
    startQuiz() {
      this.show_marks = false;
      this.marks = 0;
      this.counter = 0;
      this.selected = null;
      this.component = null;
      this.nextQuestion();
    },
  },
};
</script>

