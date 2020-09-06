<template>
  <div class="container">
    <div class="container-quiz">
      <div class="quiz-header">
        <h1>Welcome</h1>
      </div>
      <div class="main-quiz" v-show="!show_marks" >
        <div class="question-box">
          <h1>question</h1>
          <p>{{questions[questionIndex].question}}</p>
        </div>
        <div class="options">
          <ul>
            <li
              v-for="ans in questions[questionIndex].answers"
              :key="ans"
              @click="selectComponent(ans,$event)"
            >{{ans}}</li>
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
import json from "./assets/data.json";
export default {
  name: "App",
  data() {
    return {
      questions: json.questions,
      questionIndex: Math.floor(Math.random() * 15),
      selected: false,
      marks: 0,
      show_marks:false,
      finished:false,
    };
  },
  methods: {
    selectComponent(component, event) {
      if (this.selected) {
        this.selected.classList.remove("select");
      }
      this.selected = event.target;
      this.selected.classList.add("select");
      if (component.correct) {
        this.marks += 1;
      }
    },
    nextQuestion() {
      (this.questionIndex = Math.floor(Math.random() * 15)),
      this.selected = null;
    },
     finishQuiz() {
      this.show_marks=true
      this.selected = null;
    },
  },
};
</script>

