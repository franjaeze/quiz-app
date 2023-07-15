    <script setup>
    import { onMounted,ref } from 'vue';


    const props = defineProps({
      score: Object,
      endTime: Object
    })

    const scoreFinal = ref(props.score);
    const percentageScore = ( scoreFinal.value.correctAnswer /scoreFinal.value.totalAnswer )*100
    const endTimefinal = ref(props.endTime);

    onMounted(() => {
 
      const correct = scoreFinal.value.correctAnswer
    const wrong = scoreFinal.value.totalAnswer - scoreFinal.value.correctAnswer

    console.log (correct)
  const xValues = ["Right Answers", "Wrong Answers"]
  const yValues = [correct,wrong];
  const barColors = ["rgb(0, 153, 51)", "	rgb(255, 51, 51)"];

  new Chart("myChart", {
    type: "pie",
    data: {
      labels: xValues,
      datasets: [{
        backgroundColor: barColors,
        data: yValues
      }]
    },
    options: {
      title: {
        display: true,
        text: "Chart"
      }
    }
  });
});

    </script>

    <template>
      <div >
         
      <h1><span>Game Over!</span></h1>
       <canvas id="myChart" style="width:80%;max-width:600px"></canvas>
      <p v-if="(percentageScore>60)" class="congrats">Well done!</p>     <p v-else class="wrong">Try harder next time!</p>
      <h3>Your score  <span :class="(percentageScore>60)? 'congrats' : 'wrong'">  {{ percentageScore   }} %</span></h3>
      <h3> Your time  <span class="time">  {{ endTime.minutes }}' : {{ endTime.sec }}"</span></h3>
      <h4> Try again or chose another topic!</h4>
      </div>

    </template>

    <style>

    .time{
      color:rgb(16, 91, 155)
    }
    
    </style>