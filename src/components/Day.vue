<template>
 <div class="day"
 :class="[
  {'day--today': isToday},
  {'day--selected': selected},
  {'day--disabled': !showCircle}
 
  ]">
  <p class="day__name">
   {{ weekDayShortName }}
  </p>
  <p class="day__number">
   {{ dayNumber }}
  </p>
  <div v-if="showCircle && !props.dayMood" class="day__circle">
  </div>
  <MoodEmoji 
  style="margin-top: 9px;"
  v-if="props.dayMood" 
  :selected="false" 
  :is-disabled="false" 
  :mood="props.dayMood"
  size="xs"></MoodEmoji>
 </div>
</template>





<script setup lang="ts">
import { computed } from '@vue/reactivity';
import MoodEmoji from '@/components/MoodEmoji.vue'
import { onMounted } from 'vue';

const props = defineProps<{
 day: Date,
 selected: Boolean,
 dayMood: number | undefined,
}>()

const weekDayShortName = computed(() => {
 switch(props.day.getDay()) {
  case 0:
   return 'Dim';
  case 1:
   return 'Lun';
  case 2:
   return 'Mar';
  case 3:
   return 'Mer';
  case 4:
   return 'Jeu';
  case 5:
   return 'Ven';
  case 6:
   return 'Sam';
 }
})

const showCircle= computed(() => {
 return props.day.getUTCDate() <= new Date().getUTCDate();
})

const isToday = computed(() => {
 return props.day.getUTCDate() === new Date().getUTCDate() 
})

const dayNumber = computed(() => {
 return props.day.getUTCDate();
})
</script>

<style lang="less">
.day {
 width: 62px;
 padding: 8px 0 16px;
 height: 106px;
 display: flex;
 flex-direction: column;
 align-items: center;
 border-radius: 4px;

 &--today {
  color:#3c76ff !important;
 }

 &--selected {
  background:#3c76ff15;
 }

 &--disabled {
  opacity: 0.5;
 }

 &__name {
  font-size: 12px;
  line-height: 24px;
  opacity: 0.7;
 }

 &__number {
  font-size: 16px;
  line-height: 24px;
  font-weight: 600;
 }
 
 &__circle {
  height: 16px;
  width: 16px;
  border-radius: 50%;
  border: 1px solid #3c76ff;
  margin-top: auto;
 }
}
</style>