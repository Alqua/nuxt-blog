<template>
  <div class="flex justify-center">
    <div class="relative w-full h-[25rem] sm:h-[30rem] md:h-[35rem] lg:h-[40rem] flex justify-center overflow-hidden">
      <transition-group
        tag="div"
        :name="transitionName"
        class="w-full"
        mode="in-out"
      >
        <div
          v-if="show"
          :key="imageNumber"
          class="slide absolute left-0 right-0"
        >
          <img
            :src="showGall[imageNumber]"
            :key="imageNumber"
            :alt="imageFolder + imageNumber"
            class="rounded-lg mx-auto absolute right-0 left-0"
          />
        </div>
      </transition-group>

      <button
        @click="decreaseImageNumber()"
        class="absolute top-0 bottom-0 left-1 sm:left-10 flex items-center justify-center p-0 text-center"
        aria-label="Previous"
      >
        <Icon
          class="hover:bg-white rounded-full p-1 sm:p-2 text-3xl sm:text-4xl md:text-5xl"
          name="fa6-solid:chevron-left"
        />
      </button>

      <button
        @click="increaseImageNumber()"
        class="absolute top-0 bottom-0 right-3 sm:right-5"
        aria-label="Next"
      >
        <Icon
          class="hover:bg-white rounded-full p-1 sm:p-2 text-3xl sm:text-4xl md:text-5xl"
          name="fa6-solid:chevron-right"
        />
      </button>
      <ul class="absolute flex bottom-16 sm:bottom-20 justify-center">
        <li
          v-for="n in Object.keys(assetsFiltered).length"
          :key="n"
          @click="setImageNumber(n)"
        >
          <button class="" aria-label="Dot">
            <Icon
              class="hover:bg-white rounded-full p-2 text-3xl sm:text-4xl md:text-5xl"
              name="fa6-solid:circle"
              :class="{ 'text-gray-900/10': selectedImage(n) }"
            />
          </button>
        </li>
      </ul>
      <!-- <transition 

    :name="swipeDirection" class="" :class="swipeDirection" >
      <img :src="showGall[imageNumber]" :key="imageNumber" class="rounded-lg img " />
    </transition> -->
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  imageFolder: String,
});

const imageFolder = props.imageFolder;
const imageNumber = ref(0);
const swipeDirection = ref("");
const transitionName = ref("fade");
const show = ref(true);

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

const showGall = Object.values(assetsFiltered);

const showGallery = () => {
  const imagePath = Object.values(assetsFiltered)[imageNumber.value];
  //console.log('this is image path',imagePath)
  return imagePath;
};

function increaseImageNumber() {
  const limitScroll = Object.keys(assetsFiltered).length - 1;
  // console.log(limitScroll)
  swipeDirection.value = "carousel-right";
  transitionName.value = "slide-next";
  console.log(swipeDirection.value);
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
  swipeDirection.value = "carousel-left";
  transitionName.value = "slide-prev";
  console.log(swipeDirection.value);
  if (imageNumber.value > 0) {
    imageNumber.value = imageNumber.value - 1;
    console.log(imageNumber.value);
  } else if (imageNumber.value == 0) {
    imageNumber.value = limitScroll;
    console.log(imageNumber.value, "passed 0");
  }
}

function setImageNumber(index) {
  if (imageNumber.value > (index - 1)) {
    transitionName.value = "slide-prev"
  } else if (imageNumber.value < (index - 1)) {
    transitionName.value = "slide-next"
  }
  imageNumber.value = index - 1;
}

const selectedImage = (n) => {
  if (n == imageNumber.value + 1) {
    return true;
  } else {
    return false;
  }
};
</script>

<style scoped>
img {
  width: 50%;
}

/* FADE IN */
.fade-enter-active {
  transition: opacity 1s;
}
.fade-enter {
  opacity: 0;
}

/* GO TO NEXT SLIDE */
.carousel-right-enter-active,
.carousel-right-leave-active {
  transition: transform 0.5s ease-in-out;
}
.carousel-right-enter-from {
  transform: translate(100%);
}
.carousel-right-leave-to {
  transform: translate(-100%);
}

/* GO TO PREVIOUS SLIDE */
.carousel-left-enter-active,
.carousel-left-leave-active {
  transition: transform 0.5s ease-in-out;
}
.carousel-left-enter-from {
  transform: translate(-100%);
}
.carousel-left-leave-to {
  transform: translate(100%);
}

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

.slide {
  @apply min-w-[100px] min-h-[200px] md:min-w-[500px] md:min-h-[300px]
  /* min-width: 500px;
  min-height: 300px; */
}

/* .slide {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 500px;
  left: 500px;
  display: flex;
  align-items: center;
  justify-content: center;
} */
</style>
