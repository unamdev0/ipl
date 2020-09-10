<template>
  <div class="container">
    <div class="main-container" style=" background-color: #10ffd9fb;">
      <div class="quiz-header">
        <h1>Take the IPL Quiz!!!</h1>
      </div>
      <div class="quiz-main" v-show="!show_marks">
        <div class="question-box">
          <h1>Q{{ counter }}. {{ questions[questionIndex].question }}</h1>
        </div>
        <div class="options">
          <ul>
            <li
              v-for="ans in questions[questionIndex].answers"
              :key="ans.option"
              @click="selectComponent(ans, $event)"
            >
              {{ ans.option }}
            </li>
          </ul>
        </div>
      </div>
      <div class="quiz-main" v-show="show_marks">
        <br />
        <br />
        <h1 v-show="marks >= 7">You did good....🤩</h1>
        <h1 v-show="marks >= 4 && marks < 7">Not Bad...😁</h1>
        <h1 v-show="marks < 4">Don't Worry,You Can Improve...🙂</h1>
        <br />
        <br />
        <h1>You Scored:{{ marks }}/10</h1>
      </div>

      <div class="quiz-footer">
        <button v-if="!show_marks" @click="finishQuiz()">Finish</button>
        <button v-if="!show_marks" @click="nextQuestion()">Next</button>
        <button v-if="show_marks" @click="startQuiz()">Restart</button>
      </div>
    </div>
  </div>
</template>

<script>
const json = () => import("../assets/data.json");
export default {
  name: "Quiz",
  data() {
    return {
      questions: null,
      questionIndex: null,
      component: null,
      selected: false,
      marks: 0,
      show_marks: false,
      counter: 1,
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
      (this.questionIndex = Math.floor(Math.random() * (this.questions.length))),
        (this.counter += 1);
      if (this.component != null && this.component.correct==true) {
        this.marks += 1;
      }
      this.component = null;
      if (this.selected) {
        this.selected.classList.remove("select");
      }
    },
    finishQuiz() {
      this.show_marks = true;
      if (this.component != null && this.component.correct==true) {
        this.marks += 1;
      }
      this.component = null;
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
  mounted() {
    if (localStorage.getItem("quiz")) {
      this.questions = JSON.parse(localStorage.getItem("quiz"));
      this.questionIndex=Math.floor(Math.random() * (this.questions.length))
    } else {
      json().then((data) => {
        this.questions = data.questions;
        localStorage.setItem("quiz", JSON.stringify(data.questions));
        this.questionIndex=Math.floor(Math.random() * (this.questions.length))

      });
    }
  },
};
</script>

<style scoped>
.quiz-header {
  display: flex;
  width: 100%;
  height: 20%;
  border-bottom: 1px solid #94b7b9fb;
  justify-content: center;
  align-items: center;
  border-radius: 10px 10px 0px 0px;
}

.quiz-main {
  display: flex;
  width: 100%;
  height: 70%;
  flex-flow: column;
  margin: auto;
  background-color: white;
}

.quiz-footer {
  display: flex;
  width: 100%;
  height: 10%;
  justify-content: center;
  border-top: 1px solid #aababb;
}

.question-box {
  margin-top: 20px;
}

.options {
  display: flex;
  width: 100%;
  justify-content: center;
  margin: auto;
}

ul {
  display: flex;
  width: 80%;
  margin: 0;
  padding: 0;
  flex-flow: column;
}

ul li {
  list-style: none;
  line-height: 2;
  border: 1px solid #4b5f61;
  margin-bottom: 20px;
  border-radius: 15px;
  cursor: pointer;
}

li.select {
  border: 1px solid rgb(74, 219, 74);
  background-color: rgb(74, 219, 74);
  color: white;
  font-weight: 600;
}

li:hover {
  background-color: #b68b59;
}

.quiz-footer button {
  display: flex;
  width: 150px;
  height: 35px;
  outline: none;
  border: 0;
  color: white;
  font-size: 18px;
  cursor: pointer;
  border-radius: 15px;
  margin: auto;
  background-color: #5da2db;
  justify-content: center;
}

@media only screen and (max-width: 710px) {
  /* For mobile phones: */
  .quiz-header h1 {
    font-size: 20px;
  }
  .quiz-main h1 {
    font-size: 10px;
  }

  ul li {
    font-size: 10px;
  }
  .quiz-footer button {
    font-size: 15px;
    width: 60px;
    height: 25px;
    border-radius: 3px;
  }
}
</style>
