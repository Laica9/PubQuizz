<template>
  <main class="flex h-screen items-center justify-center bg-gray-500">
    <!-- Quiz overlay -->
      <quizzOverlay v-if="endOfQuizz" :percent="percentageScore"
      ></quizzOverlay>

    <!-- Quiz container -->
    <div
       class="overflow-hidden bg-gray-300 flex-none container relative rounded-lg round px-12 py-6">

      <!-- Contents -->
      <div class="relative z-20">
        <!-- Score -->
        <div class="text-center text-gray-800">
          <p class="text-sm leading-3">Score</p>
          <p class="font-bold">{{score}}</p>
        </div>

          <!-- Questions container -->
          <div class="rounded-lg bg-gray-300 p-2 neumorph-1 text-center font-bold text-gray-650">
            <div class="bg-white p-5">
            {{curr.question}}
            </div>
          </div>

          <!-- options -->
          <div class="mt-8">

            <div v-for="(choice, item) in curr.choices" :key="item">
              <div class="neumorph-1 option-default bg-gray-100 p-2 rounded-lg mb-3 relative"
                :ref="chosenOption"
                @click="clickOption(choice, item)">    
                  <div>
                  </div>
                    <div class=" rounded-lg font-bold flex p-2 "> <!-- second div child -->

                      <!-- option ID -->
                      <div class="p-3 rounded-lg">{{item}}</div>

                      <!-- option name --> 
                      <div class="flex items-center pl-6">{{choice}}</div>
                    </div>
                </div>
            </div>

          </div>
        
        <!-- Progress container -->
      <div class="mt-8 text-center">
        <div class="h-1 w-12 bg-gray-800 rounded-full mx-auto"></div>
          <p class="font-bold text-gray-800">{{questionCounter}}/{{questions.length}}</p>
       </div>
      </div>
    </div>
  </main>
  
</template>

<style scoped>
.neumorph-1{
  box-shadow: 6px 6px 18px rgba(0, 0, 0, 0.9), -6px -6px 18px #ffffff;
}
.container{
  max-width: 400px;
  border-radius: 25px;
}
</style>

<script>
import {onMounted, ref} from 'vue'
import quizzOverlay from './components/quizzOverlay.vue'
export default {
  setup() {

    //data
    let canClick = true;
    let endOfQuizz = ref(false);
    let questionCounter = ref(0)
    let score = ref(0)
    const curr = ref ({
      question: "",
      answer: 3,
      choices: []
    });
    let percentageScore = ref(0);
    const questions=[
      {
        question:
          "The first ever commercial bungee jump took place in which country?",
          answer: 3,
          choices: ["South Africa", "Australia", "New Zealand"],
      },
      {
        question:
          "When did the British children's television programme Blue Peter first air? ",
          answer: 1,
          choices: ["1958", "1968", "1978"],
      },
      {
        question:
          "Where did Disney open a resort on June 16, 2016?",
          answer: 2,
          choices: ["Singapore", "Shanghai", "Tokyo"],
      },
    ];

  
    const onStart = () => {
      canClick = true;
      // array length check. If there are more questions
      if(questions.length > questionCounter.value) {
        // start next question
        curr.value = questions[questionCounter.value];
        console.log("current question", curr.value);
        questionCounter.value++;
      }else {
        // end of questions
        onQuizzEnd();
        console.log("Finish");
      }
    };

    // functions
    let itemsRef=[];
    const chosenOption = (Element) => {
      if(Element){
        itemsRef.push(Element);
      }
    };


  // function to reset style
    const clearOption = (dCont) => {
      
      setTimeout(() => {
        dCont.classList.remove("option-correct");
        dCont.classList.remove("option-wrong");
        dCont.classList.add("option.default");
        onStart();
      }, 1000)
      
    }

    
    const clickOption = (choice, item)=> {

      if(canClick) {
        //select option
        const dCont = itemsRef[item];
        const optionID = item+1;
          if(curr.value.answer == optionID) {
            console.log('Correct answer');
            score.value = score.value += 1;
            dCont.classList.add('option-correct');
            dCont.classList.remove('option-default');
          }else {
            console.log('Wrong answer');
            dCont.classList.add('option-wrong');
            dCont.classList.remove('option-default');
          }
        canClick = false;
        //go to next question
        clearOption(dCont);
        console.log(choice, item)
      }else{
        // cannot select
        console.log("cant select question");
      }
    };

    const onQuizzEnd = function(){
      // score percentage
      percentageScore.value = score.value;
      //show overlay
      endOfQuizz.value =true;
    };

  
    // lifecycle hooks
    onMounted(()=> {
      onStart();
    });

    // export
    return {curr, questions, questionCounter, onStart,
            clickOption, chosenOption, score, endOfQuizz, onQuizzEnd,
            percentageScore };
  },
  components: {
      quizzOverlay,
    },
};
</script>


