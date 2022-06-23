<template>
  <div class="flex mb-10">
    <div
      class="hidden lg:flex h-screen lg:w-40 ring-1 ring-gray-500 p-5 space-y-10 select-none sticky left-0 top-0"
    >
      <div>
        <h1 class="text-lg font-bold text-gray-900 font-sans"> <img src="/achilles.jpg" alt="" class="h-1 w-1 object-contain" /></h1>
      </div>
      <div>
        <ul
          v-for="sidebarItem in sidebarItems"
          :key="sidebarItem.text"
          class="text-gray-700 -ml-24 mt-5 font-medium font-sans hover:bg-green-600 rounded-lg hover:text-white"
        >
          <li :class="[sidebarItem.text === 'Dashboard' ? 'bg-green-500 rounded-lg' : '']" class="mb-3 px-5 py-2 text-xs duration-300 cursor-pointer">
            {{ sidebarItem.text }}
          </li>
        </ul>
      </div>
    </div>
    <div class="flex-1 container mx-auto select-none">
      <div
        class="flex justify-between items-center lg:justify-end lg:items-end lg:border-b border-gray-500 px-5 py-2 lg:p-3 sticky top-0 w-full bg-gray-300 left-0 z-20"
      >
        <div class="flex items-center space-x-2 order-1 lg:order-0">
          <div class="space-y-1">
            <p class="lg:text-lg text-sm font-sans font-bold text-gray-900">
              Marquis Abah
            </p>
            <p class="text-xs text-gray-700 font-sans">marquisabah@gmail.com</p>
          </div>
          <div>
            <img class="lg:w-10 lg:h-10 h-8 w-8" src="/logo.png" alt="" />
          </div>
        </div>
        <div class="lg:hidden space-y-1">
           <div class="bg-white w-6 h-0.5"></div>
           <div class="bg-white w-6 h-0.5"></div>
           <div class="bg-white w-6 h-0.5"></div>
        </div>
      </div>

      <div
        class="mx-auto space-y-10 flex justify-center items-center flex-col lg:w-11/12 mt-5 px-3 py-3 lg:p-5 lg:rounded-md lg:shadow-md relative"
      >
        <div class="space-y-2 flex justify-center items-center flex-col ">
          <p class="text-green-500 font-medium font-sans text-sm text-center">
            QUESTION {{ currentQuestionIndex + 1 }}/{{ dbQuestions.length }}
          </p>
          <div class="text-center text-sm font-serif lg:text-base ring-1 ring-gray-100 lg:font-bold p-3 rounded-md shadow-sm lg:w-3/4">{{dbQuestions[currentQuestionIndex].question }}</div>
          <img
            :src="dbQuestions[currentQuestionIndex].image"
            alt=""
            class="h-44 w-full object-cover"
          />
          <div class="flex flex-col items-center absolute top-64 lg:top-0 lg:right-0 bg-gray-100 lg:ring-1 ring-gray-500 rounded-sm shadow-sm px-3 lg:py-1 lg:px-3">
             <p class="text-xs font-medium">Time Remaining</p>
              <div v-if="showTimer">
                <span class="inline-block font-bold font-mono text-red-600 text-sm lg:text-xl">{{minutes}}</span>
                <span class="font-bold text-black">:</span>
                <span class="inline-block font-bold font-mono text-red-600 text-sm lg:text-xl">{{seconds}}</span>
              </div>
              <div @click="toggleTimerVisibility" class="text-green-500 font-sans text-sm cursor-pointer hidden lg:flex">{{showTimer ? '[ Hide Timer ]' : '[ Show Timer ]'}}</div>
          </div>
        </div>
        <div class="space-y-5">
          <div class="lg:flex justify-between items-center lg:space-x-10 space-y-4 lg:space-y-0">
            <div class="flex items-center space-x-1">
              <div class="bg-white rounded-full shadow-sm text-gray-900 p-1 ring-1 text-xs lg:text-base font-medium">A</div>
              <div
                @click="handleOptionClick"
                class="option flex items-center justify-between hover:text-green-600 border hover:border-green-600 cursor-pointer rounded-md shadow-sm py-3 space-x-5 text-xs font-sans px-3"
              >
                <span>{{
                  dbQuestions[currentQuestionIndex].optionA.toLowerCase()
                }}</span>
            </div>
            </div>
             <div class="flex items-center space-x-1">
              <div class="bg-white rounded-full shadow-sm text-gray-900 p-1 ring-1 text-xs lg:text-base font-medium">B</div>
              <div
                @click="handleOptionClick"
                class="option flex items-center justify-between hover:text-green-600 border hover:border-green-600 cursor-pointer rounded-md shadow-sm py-3 space-x-5 text-xs font-sans px-3"
              >
                <span>{{
                  dbQuestions[currentQuestionIndex].optionB.toLowerCase()
                }}</span>
            </div>
            </div>
          </div>
          <div class="lg:flex justify-between items-center lg:space-x-10 space-y-4 lg:space-y-0">
             <div class="flex items-center space-x-1">
              <div class="bg-white rounded-full shadow-sm text-gray-900 p-1 ring-1 text-xs lg:text-base font-medium">C</div>
              <div
                @click="handleOptionClick"
                class="option flex items-center justify-between hover:text-green-600 border hover:border-green-600 cursor-pointer rounded-md shadow-sm py-3 space-x-5 text-xs font-sans px-3"
              >
                <span>{{
                  dbQuestions[currentQuestionIndex].optionC.toLowerCase()
                }}</span>
            </div>
            </div>
              <div class="flex items-center space-x-1">
              <div class="bg-white rounded-full shadow-sm text-gray-900 p-1 ring-1 text-xs lg:text-base font-medium">D</div>
              <div
                @click="handleOptionClick"
                class="option flex items-center justify-between hover:text-green-600 border hover:border-green-600 cursor-pointer rounded-md shadow-sm py-3 space-x-5 text-xs font-sans px-3"
              >
                <span>{{
                  dbQuestions[currentQuestionIndex].optionD.toLowerCase()
                }}</span>
            </div>
            </div>
          </div>
        </div>
        <div class="flex items-center space-x-5 md:space-x-20 lg:space-x-48">
          <button
            class="py-2 text-white bg-green-600 rounded-lg text-xs lg:text-sm px-5 duration-300 hover:bg-green-900"
            @click="handlePreviousQuestion"
          >
            Previous Question
          </button>
          <button
            class="py-2 text-white bg-green-600 rounded-lg text-xs lg:text-sm px-5 duration-300 hover:bg-green-900"
            @click="handleNextQuestion"
          >
            Next Question
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  name: "AchilisQuiz",
  data() {
    return {
      sidebarItems: [
        { text: "Dashboard", link: "/#" },
        { text: "Take a test", link: "/#" },
      ],
      showTimer : true,
      currentQuestionIndex: 0,
      selectedOption: "",
      correctAnswers: 0,
      wrongAnswers: 0,
      score: 0,
      minutes: "",
      seconds: "",
      answeredQuestions: [],
      hasQuizEnded: false,
      interval : null,
      dbQuestions: [
        {
          id: 1,
          image: "/questionImage.jpeg",
          question:
            "Do you work or are you cared for/accomodated in one of the following areas 1 Do you work or are you cared for/accomodated in one of the following areas?",
          optionA:
            "Working in a medical fiels (care, doctor's office, hospital, similar) Working in a medical fiels (care, doctor's office, hospital, similar) 1",
          optionB:
            "Working in a medical field (care, doctor's office, hospital, similar) Working in a medical fiels (care, doctor's office, hospital, similar) 1",
          optionC:
            "Working in a medical field (care, doctor's office, hospital, similar) Working in a medical fiels (care, doctor's office, hospital, similar) 1",
          optionD:
            "Working in a medical field (care, doctor's office, hospital, similar) Working in a medical fiels (care, doctor's office, hospital, similar) 1",
          answer:
            "Working in a medical fiels (care, doctor's office, hospital, similar) Working in a medical fiels (care, doctor's office, hospital, similar) 1",
        },
        {
          id: 2,
          image: "/questionImage.jpeg",
          question:
            "Do you work or are you cared for/accomodated in one of the following areas 2?",
          optionA:
            "Working NEXT a medical fiels (care, doctor's office, hospital, similar) 2",
          optionB:
            "Working NEXT a medical fiels (care, doctor's office, hospital, similar) 2",
          optionC:
            "Working NEXT a medical fiels (care, doctor's office, hospital, similar) 2",
          optionD:
            "Working NEXT a medical fiels (care, doctor's office, hospital, similar) 2",
          answer:
            "Working NEXT a medical fiels (care, doctor's office, hospital, similar) 2",
        },
        {
          id: 3,
          image: "/questionImage.jpeg",
          question:
            "Do you work or are you cared for/accomodated in one of the following areas 3?",
          optionA:
            "Working in a medical fiels (care, doctor's office, hospital, similar 3",
          optionB:
            "Working in a medical fiels (care, doctor's office, hospital, similar 3",
          optionC:
            "Working in a medical fiels (care, doctor's office, hospital, similar 3",
          optionD:
            "Working in a medical fiels (care, doctor's office, hospital, similar 3",
          answer:
            "Working in a medical fiels (care, doctor's office, hospital, similar) 3",
        },
      ],
    };
  },
  methods: {
    handleNextQuestion() {
      this.currentQuestionIndex = this.currentQuestionIndex + 1;
      this.removeOptionsColor();
      let nextQuestion = this.dbQuestions[this.currentQuestionIndex];
      if (_.isEmpty(nextQuestion)) {
        ale("Next question is not available");
        this.hasQuizEnded = true;
      }
    },
    handlePreviousQuestion() {
      this.currentQuestionIndex = this.currentQuestionIndex - 1;
      this.removeOptionsColor();
      let previousQuestion = this.dbQuestions[this.currentQuestionIndex];
      if (_.isEmpty(previousQuestion)) {
        alert("Previous question is not available");
      }
    },
    handleOptionClick(e) {
      let optionClicked = e.target.innerHTML.toLowerCase();
      let correctAnswer =
        this.dbQuestions[this.currentQuestionIndex].answer.toLowerCase();
      let options = Array.from(document.querySelectorAll(".option"));
      if (optionClicked === correctAnswer) {
        this.correctAnswers++;
        this.score++;
        this.selectedOption = optionClicked;
        options.forEach((option) => {
          if (option.innerText.toLowerCase() === optionClicked) {
            option.style.color = "white";
            option.style.backgroundColor = "#006A4E";
          }
        });
      }

      if (optionClicked !== correctAnswer) {
        this.wrongAnswers++;
        this.selectedOption = optionClicked;
        options.forEach((option) => {
          if (option.innerText.toLowerCase() === optionClicked) {
            option.style.color = "white";
            option.style.backgroundColor = "#E23D28";
          }
        });
      }

      let eachQuestionSnapshot = {
        snapshotQuestion:
          this.dbQuestions[this.currentQuestionIndex].question.toLowerCase(),
        snapshotRightAnswer: correctAnswer,
        snapshotSelectedAnswer: optionClicked,
      };

      this.answeredQuestions = [
        ...this.answeredQuestions,
        eachQuestionSnapshot,
      ];
    },
    removeOptionsColor() {
      let options = Array.from(document.querySelectorAll(".option"));
      options.forEach((option) => {
        option.style.color = "#000000";
        option.style.backgroundColor = "white";
      });
    },

    startTimer(){
    const countDownTime = Date.now() + 900000;
    this.interval = setInterval(() => {
      const now = new Date();
      const distance = countDownTime - now;

      const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((distance % (1000 * 60)) / 1000);
      
      if(this.dbQuestions.length === 0) {
        clearInterval(this.interval);
      }

      if (distance < 0) {
        clearInterval(this.interval);
        this.minutes = 0
        this.seconds = 0
        // endGame();
      } else {
        this.minutes = minutes
        this.seconds = seconds
      }
    }, 1000);
  },
  toggleTimerVisibility(){
    this.showTimer = !this.showTimer
  }
  },

  mounted(){
    this.startTimer()
  }
};
</script>

<style></style>
