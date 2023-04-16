<template>
 <div class="mood"
 :class="[
   {'mood--selected': props.selected},
   {'mood--disabled': props.isDisabled},
   {'mood--xs': props.size === 'xs'}
  ]
 ">
  <div class="mood-emoji"
  :class="[
   {'mood-emoji--angry': props.mood === 1},
   {'mood-emoji--sad': props.mood === 2},
   {'mood-emoji--neutral': props.mood === 3},
   {'mood-emoji--great': props.mood === 4},
   {'mood-emoji--happy': props.mood === 5},
  ]">
   <div class="mood-emoji__eyes">
    <div class="mood-emoji__eye mood-emoji__eye--left"></div>
    <div class="mood-emoji__eye mood-emoji__eye--right" ></div>
   </div>
   <div class="mood-emoji__mouth"></div>
  </div>
 </div>
</template>

<script setup lang="ts">
const props = defineProps<{
 selected: boolean,
 mood: number,
 isDisabled: boolean,
 size: string
}>()

</script>

<style lang="less">
.mood {
 width: 62px;
 height: 64px;
 border-radius: 4px;
 display: flex;
 align-items: center;
 justify-content: center;

 &--selected {
  background:#3c76ff15;
 }

 &--disabled {
  pointer-events: none;
  opacity: 0.3;
 }

 &--xs {
  transform: scale(0.5);
 }
}

.mood-emoji {
 height: 34px;
 width: 34px;
 border-radius: 50%;
 position: relative;

 &__eyes {
  position: absolute;
  height: 4px;
  right: 4px;
  display: flex;
  align-items: center;
  // justify-content: flex-end;
  min-height: 4px;
  gap: 14px;
 }

 &__eye {
  height: 4px;
  width: 4px;
  border-radius: 2px;
  background: #494949;
  animation: eyeClose 2s infinite alternate;
  animation-delay: 1s;
  // transform-origin: center;
 }

 &__mouth {
  position: absolute;
 }

 &--angry {
  background: linear-gradient(45deg, #FF2929, #FFEACA);
  .mood-emoji__eyes {
   top: 22px;
  }

  .mood-emoji__eye {
   position: relative;
   

   &::before {
    content: "";
    position: absolute;
    height: 2px;
    width: 7px;
    background:#494949;
    top: -4px;
   }

   &--left {
    &::before {
     left: -2px;
     transform: rotate(18deg);
    }
   }
   &--right {
    &::before {
     left: -1px;
     transform: rotate(-18deg);
    }
   }
  }

  .mood-emoji__mouth {
   top: 24px;
   right: 10px;
   width: 10px;
   height: 5px;
   border: 2px solid #494949;
   border-bottom: transparent;
   border-radius: 5px 5px 0 0;
  }
 }

 &--sad {
  background: linear-gradient(45deg, #FF7629, #FFEACA);
  .mood-emoji__eyes {
   top: 19px;
  }
  .mood-emoji__mouth {
   top: 22px;
   right: 10px;
   width: 10px;
   height: 5px;
   border: 2px solid #494949;
   border-bottom: transparent;
   border-radius: 5px 5px 0 0;
  }
 }

 &--neutral {
  background: linear-gradient(45deg, #FFAA29, #FFF2DE);
  .mood-emoji__eyes {
   right: 2px;
   top: 15px;
  }

  .mood-emoji__mouth {
   height: 2px;
   width: 10px;
   background:#494949;
   top: 20px;
   right: 8px;
  }
 }
 &--great {
  background: linear-gradient(45deg, #FFB800, #FFEE53);
  .mood-emoji__eyes {
   top: 11px;
  }
  .mood-emoji__mouth {
   top: 15px;
   right: 10px;
   width: 10px;
   height: 5px;
   border: 2px solid #494949;
   border-top: transparent;
   border-radius: 0 0 5px 5px;
  }

 }
 &--happy {
  background: linear-gradient(45deg, #E3B100, #FFF59B);
  .mood-emoji__eyes {
   top: 7px;
  }

  .mood-emoji__mouth {
   top: 11px;
   right: 10px;
   width: 10px;
   height: 5px;
   background:#494949;
   border-radius: 0 0 5px 5px;
  }
 }
}

@keyframes eyeClose {
 0% {
  height: 4px;
 }

 10% {
  height: 1px;
 }

 20% {
  height: 4px;
 }

 100% {
  height: 4px;
 }
}
</style>