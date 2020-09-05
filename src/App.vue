<template>
  <div class="container">
    <div class="container-quiz">
      <div class="quiz-header">
        <h1>Welcome</h1>
      </div>
      <div class="main-quiz" v-for="index in 10" :key="index">
        <div class="question-box">
            <h1>question</h1>
            <!-- <p>{{element.question}}</p> -->
        </div>
        <div class="options">
          <ul>
            <li>

            </li>
          </ul>
          
        </div>
      </div>

      <div class="footer-quiz"></div>
    </div>
  </div>
</template>

<script>
import json from './assets/data.json'
export default {
  name: "App",
  data(){
    return{
      questions:json.questions
    }
  }
};
</script>

