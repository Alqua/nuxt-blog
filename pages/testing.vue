<template>
    <main>
        <transition-group tag="div" :name="transitionName" class="slides-group">
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
    </main>
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
      this.direction = dir;
      dir === 1
        ? (transitionName.value = "slide-next")
        : (transitionName.value = "slide-prev");
      let len = slides.length;
      this.current = (this.current + dir % len + len) % len;
    }    

</script>