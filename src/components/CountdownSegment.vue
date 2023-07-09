<script setup>
import { ref, computed, watch, onMounted, reactive } from 'vue';
//TODO: Make this a generic counter that can take a date and a title
//Get time to New Years of this year
const newYearsDay = new Date("2024-01-01").getTime()
const now = ref(new Date().getTime())
const timer = ref(1)

//Calculate the time left
const timeLeft = computed(() => {
  return newYearsDay - now.value
})

//Watch timer, when changes update now
watch(timer, () => {
  now.value = new Date().getTime()
  updateTime()
})

//Setup something that happens regularly
onMounted(() => {
  setInterval(() => timer.value++, 1000)
  updateTime()
})

//Get Days, Hours, Minutes, Seconds
const timeObject = reactive({
  days: 0,
  hours: 0,
  minutes: 0,
  seconds: 0
})

function updateTime() {
  timeObject.days = Math.floor(timeLeft.value / (1000 * 60 * 60 * 24))
  timeObject.hours = Math.floor((timeLeft.value % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  timeObject.minutes = Math.floor((timeLeft.value % (1000 * 60 * 60)) / (1000 * 60))
  timeObject.seconds = Math.floor((timeLeft.value % (1000 * 60)) / 1000)
}

</script>

<template>
  <div class="countdown-container">
    <div class="title-container">
      <slot>Generic Countdown</slot>
    </div>
  <div class="time-container">
    <div>
      <p>{{ timeObject.days }}</p>
      <span>Days</span>
    </div>
    <div>
      <p>{{ timeObject.hours }}</p>
      <span>Hours</span>
    </div>
    <div>
      <p>{{ timeObject.minutes }}</p>
      <span>Minutes</span>
    </div>
    <div>
      <p>{{ timeObject.seconds }}</p>
      <span>Seconds</span>
    </div>
  </div>
</div>
</template>

<style scoped>


.countdown-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: antiquewhite;
  border-radius: 15px;
  color: black;
}

.title-container {
  font-size: x-large;
}

.time-container{
  display:inline-flex;
  flex-direction: row;
  padding: 0;
  margin: 0;
}

.time-container div {
  margin: 5%;
  justify-content: center;
}

.time-container p {
  font-size:x-large;
  justify-content: center;
}

.time-container span {
  justify-content: center;
}

li{
  display: inline;
  flex-direction: row;
  padding: 0;
  margin: 0;
}
</style>
