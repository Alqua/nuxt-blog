<template>
    <div class="overflow-hidden">
        <transition-group tag="div" v-model:name="transitionName" class="slides-group">
            <div v-if="show" :key="current" class="slide" :class="slides[current].className">
        <p>I'm {{slides[current].className}}!</p>
      </div>
        </transition-group>
        <div class="btn btn-prev" aria-label="Previous slide" @click="slide(-1)">
      &#10094;
    </div>
    <div class="btn btn-next" aria-label="Next slide" @click="slide(1)">
      &#10095
    </div>
</div>
</template>

<script setup>
const current = ref(0)
const direction = ref(1)
const transitionName = ref('fade')
const show = ref('false')
const slides = reactive([
      { className: "blue" },
      { className: "red" },
      { className: "yellow" }
    ])
    
    
function slide(dir) {
      direction.value = dir;
      dir === 1
        ? (transitionName.value = "slide-next")
        : (transitionName.value = "slide-prev");
      let len = slides.length;
      current.value = (current.value + dir % len + len) % len;
      console.log(current.value)
      console.log(transitionName.value)
    }    

</script>

<style scoped>

@import url("https://fonts.googleapis.com/css?family=Crimson+Text");

/* FADE IN */
.fade-enter-active {
  transition: opacity 1s;
}
.fade-enter {
  opacity: 0;
}

/* GO TO NEXT SLIDE */
.slide-next-enter-active,
.slide-next-leave-active {
  transition: transform 0.5s ease-in-out ;
}
.slide-next-enter-from {
  transform: translate(100%) !important;
}
.slide-next-leave-to {
  transform: translate(-100%) !important;
}

/* GO TO PREVIOUS SLIDE */
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: transform 0.5s ease-in-out !important;
}
.slide-prev-enter-from {
  transform: translate(-100%) !important;
}
.slide-prev-leave-to {
  transform: translate(100%) !important;
}

/* SLIDES CLASSES */

.blue {
  background: #4a69bd;
}

.red {
  background: #e55039;
}

.yellow {
  background: #f6b93b;
}

/* SLIDER STYLES */
body {
  overflow: hidden ;
  margin: 0;
  font-size: 50px;
  font-family: "Crimson Text", sans-serif;
  color: #fff;
}

#slider {
  width: 100%;
  height: 100vh;
  position: relative ;
}

.slide {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn {
  z-index: 10;
  cursor: pointer;
  border: 3px solid #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 70px;
  height: 70px;
  position: absolute;
  top: calc(50% - 35px);
  left: 1%;
  transition: transform 0.3s ease-in-out;
  user-select: none;
}

.btn-next {
  left: auto;
  right: 1%;
}

.btn:hover {
  transform: scale(1.1);
}


/* GO TO NEXT SLIDE */
#lide-next-enter-active,
#slide-next-leave-active {
  transition: transform 0.5s ease-in-out !important;
}
#slide-next-enter {
  transform: translate(100%) !important;
}
.slide-next-leave-to {
  transform: translate(-100%) !important;
}

/* GO TO PREVIOUS SLIDE */
#slide-prev-enter-active,
#slide-prev-leave-active {
  transition: transform 0.5s ease-in-out !important;
}
#slide-prev-enter {
  transform: translate(-100%) !important;
}
#slide-prev-leave-to {
  transform: translate(100%) !important;
}

</style>