<template>
    <div class="flex justify-center overflow-hidden">
      <div class="relative flex justify-center overflow-hidden">
        <button
          @click="decreaseImageNumber()"
          class="absolute top-0 bottom-0 left-10 flex items-center justify-center p-0 text-center"
          aria-label="Previous"
        >
          <Icon
            class="hover:bg-white rounded-full p-2 text-5xl"
            name="fa6-solid:chevron-left"
          />
        </button>
  
        <button
          @click="increaseImageNumber()"
          class="absolute top-0 bottom-0 right-5"
          aria-label="Next"
          
        >
          <Icon
            class="hover:bg-white rounded-full p-2 text-5xl"
            name="fa6-solid:chevron-right"
          />
        </button>
        <ul class="absolute flex bottom-5 justify-center">
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
      <transition 
  
      :name="swipeDirection" class="" :class="swipeDirection" >
        <img :src="showGallery()" :key="imageNumber" class="rounded-lg img " />
      </transition>
      </div>
    </div>
  </template>
  
  <script setup>
  const props = defineProps({
    imageFolder: String,
  });
  
  const imageFolder = props.imageFolder;
  const imageNumber = ref(0);
  const swipeDirection = ref('')
  
  onMounted(() => {
    console.log("hello");
    console.log("this is props", imageFolder);
  
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
  
  const showGallery = () => {
    const imagePath = Object.values(assetsFiltered)[imageNumber.value];
    //console.log('this is image path',imagePath)
    return imagePath;
  };
  
  function increaseImageNumber() {
    const limitScroll = Object.keys(assetsFiltered).length - 1;
    // console.log(limitScroll)
    swipeDirection.value = "carousel-right"
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
  .slide-next-enter-active,
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
  .slide-prev-leave-active {
    transition: transform 0.5s ease-in-out;
  }
  .carousel-left-enter-from {
    transform: translate(-100%);
  }
  .carousel-left-leave-to {
    transform: translate(100%);
  }
  
  
  
  
  
  
  
  
  
  
  </style>
  