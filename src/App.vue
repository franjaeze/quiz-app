<script setup>
import Track from './components/Track.vue';
import Score from './components/Score.vue';
import { ref,reactive } from 'vue';
import {topics} from './services/topicsServices'

const index = ref(0);
const score = reactive({
      correctAnswer: 0,
      totalAnswer: 0,
    })
  
const game = ref(false);
const gameOver = ref(false);
   

 const startGame = () => {
  game.value = true;
  gameOver.value = false;
  restartScore();
  time.reset();
 }
 const restartScore = ()=> {
  score.correctAnswer = 0;
  score.totalAnswer = 0;
  index.value = 0;
 }
 const endGame = () => {
  game.value=false;
  gameOver.value = true;
  time.stop();

 
 
 }

 const choice = ref();

 const updateScore =  (winner) => {
  if(winner){
    score.correctAnswer++;
    score.totalAnswer++;
  } else {
    score.totalAnswer++;
  }
 }

const nextTrack = () => {
 
if(index.value==(choice.value.length - 1)){
endGame();

} else {
 
  index.value++;
}}
 
const startTimer = () => {
  const time = reactive({  minutes: 0 , sec: 0, running: true});
  let intervalId = null;
  
  const startInterval = () => {
    intervalId = setInterval(() => {
      time.sec++;
      if (time.sec === 60) {
        time.minutes++;
        time.sec = 0;
      }
    }, 1000);
  };

  startInterval();

  time.stop = () => {
    clearInterval(intervalId);
    time.running = false;

    
  };

  time.reset = () => {
    clearInterval(intervalId);
    time.minutes = 0;
    time.sec = 0;
    time.running = true;
    startInterval();
  };

  return time;
};


const time = startTimer();

</script>

<template>
  <div class="vibrate-1">   <span class="clock" v-if="game"> Timer     {{ time.minutes }}  : {{ time.sec }}</span>
    <h1 ><span class="flip" v-if="!game && !gameOver">Welcome to quiz App</span> <span v-if="game && !gameOver" id="score"> Score {{ score.correctAnswer}} / {{ score.totalAnswer }} </span></h1> 
    <Score v-if="gameOver"
            :score="score"
            :endTime="time"/>
   
    <form v-show="!game"> Select your subject
      <select v-model="choice"  >
       
        
        <option v-for="topic in topics" :key="topic.name" :value="topic.questions" > {{ topic.name }} </option>
      </select>  

    </form><i class="far fa-grin-squint"></i>
    <div class="animated-flip"><button @click="startGame()"   v-show="!game">Start!</button></div>
    
    
    <Track v-if="game"
           :model="choice[index]"
           @next-game="nextTrack"
           @update-score="updateScore"
           
           />

  </div>

</template>

<style>
h1 span{
 background: rgb(242,234,30);
background: linear-gradient(183deg, rgba(242,234,30,1) 0%, rgba(182,111,22,1) 39%, rgba(187,59,29,1) 73%, rgba(29,113,187,1) 97%);
Background-clip:text; 
-webkit-background-clip:text; 
Color:transparent; 
letter-spacing: .5rem;
 
} 
#score{
  font-size: 2rem;
  border-color: rgb(54, 54, 54);
  border-style: groove;
  border-radius: .5rem;
  padding: .5rem;
}

button{
  border-style: solid;
  border-color: black;
  background-color: rgba(77, 74, 70, 0.4);
  box-shadow: rgba(50, 50, 93, 0.25) 0px 50px 100px -20px, rgba(0, 0, 0, 0.3) 0px 30px 60px -30px, rgba(10, 37, 64, 0.35) 0px -2px 6px 0px inset;
  transition: all .6s;
}

button:hover{
  box-shadow: rgba(170, 161, 80, 0.25) 0px 50px 100px -20px, rgba(218, 160, 160, 0.3) 0px 30px 60px, rgba(120, 172, 224, 0.35)  0px -2px 6px 0px inset;
  transform: translateY(-5px);
 
}
button:active{
  box-shadow: rgba(170, 161, 80, 0.25) 25px 50px 100px -20px, rgba(218, 160, 160, 0.3) 0px 30px 60px, rgba(120, 172, 224, 0.35)  0px -2px 6px 0px inset;
  transform: translateY(3px);
  transform: translatex(1px);
  background-color: rgba(77, 74, 70, 0.6);


 
}
.clock{
  color: #7d5304;
  font-size: 1.5rem;
  font-family: Candara, Calibri, Segoe, Segoe UI, Optima, Arial, sans-serif;
  
}

form{
  font-family:  Candara, Calibri, Segoe, Segoe UI, Optima, Arial, sans-serif;
  font-size: 1.5rem;
  margin-bottom: 1rem;
}
</style>
