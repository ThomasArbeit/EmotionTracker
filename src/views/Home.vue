<template>
  <main>
    <div class="day-container">
      <Day v-for="(day, index) in data.daysToDisplay" 
      :day="day"
      :selected="data.selectedDay === day"
      :key="index"
      :dayMood="moodOfTheDay(day)"
      @click="selectDay(day)"/>
    </div>

    <div class="home-board">
      <h1>Salut toi !</h1>
      <h2>{{ subtitle }} <strong v-if="!isToday">{{ readableDay }} {{ readableDate }}</strong> </h2>
      <div class="mood-container">
        <MoodEmoji v-for="mood in 5" 
        :mood="mood"
        :selected="data.selectedMood === mood"
        :key="mood"
        size="md"
        :isDisabled="!!dayMood && data.selectedMood !== mood"
        @click="data.selectedMood = mood"/>
      </div>

      <h2>{{ inputDescription }}</h2>
      <textarea v-if="!dayMood" class="input" v-model="data.selectedDayComment" placeholder="Tapez pour écrire"></textarea>
      <p class="mood-description" v-else>{{ dayMood.comment }}</p>
      <button v-if="!dayMood" class="button" @click="save()">Sauvegarder</button>
    </div>
  </main>
</template>

<script setup lang="ts">
import Day from '@/components/Day.vue'
import MoodEmoji from '@/components/MoodEmoji.vue'
import { computed } from '@vue/reactivity';
import { onMounted, reactive, watch, watchEffect } from 'vue';

type DayMood = {
  date: Date,
  comment?: string,
  mood: number,
}

const data = reactive({
  daysToDisplay : [] as Date[],
  selectedDay: new Date as Date,
  selectedDayComment: undefined as string | undefined,
  selectedMood: null as number | null,
  recap: [] as DayMood[],
})

const isToday = computed(() => {
  return new Date().getUTCDate() === data.selectedDay?.getUTCDate();
})


const dayMood = computed(() => {
  if (data.recap.length && data.selectedDay) {
    const dayMood = data.recap.filter(x =>
      new Date(x.date).getDate() === data.selectedDay?.getDate()
      && new Date(x.date).getMonth() === data.selectedDay?.getMonth()
      && new Date(x.date).getFullYear() === data.selectedDay?.getFullYear()
    )
    return dayMood[0]
  }
})

const readableDay = computed(() => {
  switch( data.selectedDay?.getDay()) {
  case 0:
   return `Dimanche`;
  case 1:
   return 'Lundi';
  case 2:
   return 'Mardi';
  case 3:
   return 'Mercredi';
  case 4:
   return 'Jeudi';
  case 5:
   return 'Vendredi';
  case 6:
   return 'Samedi';
 }
})

const readableDate = computed(() => {
  return `${data.selectedDay?.getUTCDate()} / ${data.selectedDay?.getUTCMonth()}`
})

const subtitle = computed(() => {
  return isToday.value
  ? `Comment tu te sens aujourd'hui ?`
  : `Voici comment tu sentais le `;
})

const inputDescription = computed(() => {
  return isToday.value
  ? `Un mot sur cette journée ?`
  : `Ton commentaire sur cette journée`
})


onMounted (() => {
  assignDaysToData();
  getRecapMood();
})

function assignDaysToData () {
  const today = new Date();
  let yesterday = new Date();
  let todayMinus2 = new Date();
  let todayMinus3 = new Date();
  let todayMinus4 = new Date();
  yesterday.setDate(today.getDate() - 1);
  todayMinus2.setDate(today.getDate() - 2);
  todayMinus3.setDate(today.getDate() - 3);
  todayMinus4.setDate(today.getDate() - 4);
  data.selectedDay = today;

  data.daysToDisplay = [ todayMinus4, todayMinus3, todayMinus2, yesterday, today]
}

function selectDay (day: Date) {
  if (day.getUTCDate() > new Date().getUTCDate()) return;
  data.selectedDay = day;
  setTimeout(() => {
    if (dayMood.value && dayMood.value.mood) {
      data.selectedMood = dayMood.value.mood
    } else {
      data.selectedMood = null;
    }
  })
}

async function getRecapMood () {
  const recap = await localStorage.getItem('recap');
  if (recap?.length) {
    data.recap = JSON.parse(recap);
  }
  if (dayMood.value && dayMood.value.mood) {
    data.selectedMood = dayMood.value.mood;
  }
}
 
function save() {
  const dayMood = {
    date: data.selectedDay,
    comment: data.selectedDayComment,
    mood: data.selectedMood,
  } as DayMood;
  data.recap.push(dayMood);
  localStorage.setItem('recap', JSON.stringify(data.recap));
  // data.selectedDayComment = undefined;
}

function moodOfTheDay(date: Date) {
    if (data.recap.length && date) {
    const dayMood = data.recap.filter(x =>
      new Date(x.date).getDate() === new Date(date)?.getDate()
      && new Date(x.date).getMonth() === new Date(date)?.getMonth()
      && new Date(x.date).getFullYear() === new Date(date)?.getFullYear()
    )
    if (dayMood[0] && dayMood[0].mood) return dayMood[0].mood
    else return undefined;
  }
}
</script>


<style lang="less">
.day-container, .mood-container {
  display: flex;
  gap: 8px;
  margin-bottom: 48px;
}

.mood-container {
  margin-top: 32px;
}

.input {
  width: 100%;
  min-height: 240px;
  outline: unset;
  border: unset;
  background: #3c76ff15;
  border-radius: 4px;
  padding: 16px;
  font-size: 13px;
  margin-top: 16px;
  resize: none;
}

.mood-description {
  margin-top: 16px;
  font-size: 13px;
  line-height: 24px;
}

.button {
  width: 100%;
  border-radius: 4px;
  background: #3c76ff;
  color: white;
  outline: unset;
  border: unset;
  padding: 16px;
  font-size: 16px;
  margin: 16px 0;
}
</style>