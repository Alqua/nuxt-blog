<template>

    <div id="slider" class="overflow-hidden">
      <transition-group tag="div" :name="transitionName" class="slides-group" mode="in-out">
        <div v-if="show" :key="imageNumber" class="slide" :class="showGall[imageNumber]">
            <img :src="showGall[imageNumber]" :key="imageNumber" class="rounded-lg img " />
        </div>
      </transition-group>
      <div class="btn btn-prev" aria-label="Previous slide" @click="decreaseImageNumber()">
        &#10094;
      </div>
      <div class="btn btn-next" aria-label="Next slide" @click="increaseImageNumber()">
        &#10095
      </div>
      <div class="slider">
      <ul class="absolute flex bottom-5 justify-center roundSelect">
        <li v-for="n in (Object.keys(assetsFiltered).length)" :key="n" @click="setImageNumber(n)">
      <button class="" aria-label="Dot">
        <Icon
          class="hover:bg-white rounded-full p-2 text-5xl"
          name="fa6-solid:circle"
          :class="{ 'text-gray-900/10' : selectedImage(n) }"
        />
      </button>
    </li>
    </ul>
</div>
    </div>
    </template>

<script setup>

const imageFolder = "dostoievski"
const imageNumber = ref(0);
const swipeDirection = ref('')
const transitionName = ref("fade")
const show = ref(true)

onMounted(() => {
  console.log("hello");
  console.log("this is props", imageFolder);
  console.log("this is showGall", showGall);

  // adding listener to go through the gallery with left and right keys, issues may arise with ssr
  window.addEventListener("keyup", (event) => {
    if (event.key === "ArrowLeft") {
      decreaseImageNumber();
    }
    if (event.key === "ArrowRight") {
      increaseImageNumber();
    }
  });
});

const assets = import.meta.glob("~/assets/images/**/*", {
  eager: true,
  import: "default",
});
const path = "/assets/images/" + imageFolder;
const assetsFiltered = Object.keys(assets)
  .filter((key) => key.includes(path))
  .reduce((obj, key) => {
    return Object.assign(obj, {
      [key]: assets[key],
    });
  }, {});
  
const showGall =  Object.values(assetsFiltered);

const showGallery = () => {
  const imagePath = Object.values(assetsFiltered)[imageNumber.value];
  //console.log('this is image path',imagePath)
  return imagePath;
};





function increaseImageNumber() {
  const limitScroll = Object.keys(assetsFiltered).length - 1;
  // console.log(limitScroll)
  swipeDirection.value = "carousel-right"
  transitionName.value = "slide-next"
  console.log(swipeDirection.value)
  if (imageNumber.value < limitScroll) {
    imageNumber.value = imageNumber.value + 1;
    console.log(imageNumber.value);
  } else {
    imageNumber.value = 0;
    console.log(imageNumber.value, "limit passed");
  }
}

function decreaseImageNumber() {
  const limitScroll = Object.keys(assetsFiltered).length - 1;
  // console.log(limitScroll)
  swipeDirection.value = "carousel-left"
  transitionName.value = "slide-prev"
  console.log(swipeDirection.value)
  if (imageNumber.value > 0) {
    imageNumber.value = imageNumber.value - 1;
    console.log(imageNumber.value);
  } else if (imageNumber.value == 0) {
    imageNumber.value = limitScroll;
    console.log(imageNumber.value, "passed 0");
  }
}

function setImageNumber(index){
  imageNumber.value = (index-1)
}

const selectedImage = (n) => {
    if (n == (imageNumber.value +1)){
      return true
    }
    else {
      return false
    }

}


</script>


<style scoped>

@import url("https://fonts.googleapis.com/css?family=Crimson+Text");

img {
  width: 50%;

}

/* FADE IN */
.fade-enter-active {
  transition: opacity 2s;
}
.fade-enter {
  opacity: 0;
}

/* GO TO NEXT SLIDE */
.slide-next-enter-active,
.slide-next-leave-active {
  transition: transform 0.5s ease-in-out;
}
.slide-next-enter-from {
  transform: translate(100%);
}
.slide-next-leave-to {
  transform: translate(-100%);
}

/* GO TO PREVIOUS SLIDE */
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: transform 0.5s ease-in-out;
}
.slide-prev-enter-from {
  transform: translate(-100%);
}
.slide-prev-leave-to {
  transform: translate(100%);
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
  overflow: hidden;
  margin: 0;
  font-size: 50px;
  font-family: "Crimson Text", sans-serif;
  color: #fff;
}

#slider {
  width: 100%;
  height: 100vh;
  position: relative;
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

.roundSelect {
  right: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>