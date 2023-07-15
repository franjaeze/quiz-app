<script setup>
import {ref  } from 'vue';


const props = defineProps({
  model: Object
})

const emit = defineEmits (['next-game','update-score'])

const afterGame = ref (false);
const winner = ref (false);
const loser = ref (false);

 
const nextGame = () =>{
    emit('next-game');
 
    afterGame.value = false;
    winner.value= false;
    loser.value=false;
}
const checkAnswer = (answer) =>{
    afterGame.value = true;
  
  if (answer.correct){
   winner.value=true;
   emit('update-score', winner.value);
  } else {
    loser.value= true;
    emit('update-score', winner.value);
  }

}

 


</script>

<template>
  <div >
 
    <button v-show="afterGame" @click="nextGame()">Next question</button>
 
    <div class="question"> {{ model.question }}</div>
    <div :class="loser? 'rubberBand' : ''">
     <span v-show="loser"   :class="winner? '' : 'wrong '" >Wrong answer!</span>
    </div>
     <div :class="winner? 'animated-flip' : 'winner'"> <span v-show="winner" :class="winner? 'congrats animated-flip' : 'winner'">Congrats! Thats correct!</span></div>

    <div :class="winner? 'vibrate-1' : 'vibrate-2'">
    <div :class="afterGame ? (answer.correct ? 'after-true' : 'after-false') : 'answer'" @click="checkAnswer(answer)" v-for="answer in model.answers" :key="answer.answer"> {{ answer.answer }}  </div>
  </div>
  </div>

</template>

<style>
.answer{
  margin: 1rem;
  padding:0.25rem;
  border-radius: .6rem;
  border-color: rgb(5, 5, 5);
  border-style: groove;
  background-color: rgba(179, 152, 121, 0.527);
  transition: all .2s;
  cursor: pointer;
}

.answer:hover{
  transform: translateY(-3px);
  background-color: rgba(179, 152, 121, 0.342);
}
.question{
  font-family:   monospace;
  font-weight: 600;
  color: rgb(42, 49, 43);
  font-size: 3rem;

}

.after-true{
    margin: 1rem;
  padding:0.25rem;
  border-radius: .6rem;
  border-color: rgb(5, 5, 5);
  border-style: groove;
  background-color: rgba(55, 192, 37, 0.527);
  transition: all .2s;
 
}
.after-false{
    margin: 1rem;
  padding:0.25rem;
  border-radius: .6rem;
  border-color: rgb(5, 5, 5);
  border-style: groove;
  background-color: rgba(153, 47, 47, 0.527);
  transition: all .2s;
   
}
.congrats{
    color: rgba(16, 114, 3, 0.884);
    font-family:   monospace;
    font-size: 2rem;

}

.wrong{
    color: rgba(114, 16, 3, 0.884);
    font-family:   monospace;
    font-size: 2rem;

}
</style>