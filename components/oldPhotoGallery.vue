<template>
    <div>
      <ul class="gallery">
        <li v-for="n in (Object.keys(assets).length)" :key="n">
          <img class="is-rounded" 
          @click="highlight"
          :src="useAsset('dostoievski/' + n + '.jpg')" />
        </li>
      </ul>

<div id="frame">
    bop
  <img
    :src="theatrical"
  />
</div>

<div>
  <button @click="imageNumber++"> previous</button>
  <p>next</p>
  {{showGallery()}}
  <img
    :src="showGallery()"
  />
</div>

    </div>
  </template>

<script setup>

let theatrical = ref('')

const imageNumber = ref(0)




onMounted(() => {
    useAsset()
    console.log('assets length:',Object.keys(assets).length)
    console.log('this is assets', assets)
    console.log('this is showGallery', showGallery)
}) 

const assets = import.meta.glob('~/assets/images/**/*', {
    eager: true,
    import: 'default',
  })
// const imagesAssets = () => {
//     const assets = import.meta.glob('~/assets/images/**/*', {
//     eager: true,
//     import: 'default',
//   })
//   const assList = []
//   for x in assets
//   return assets['/assets/images/' + path]
// }


function useAsset(path) {
  const assets = import.meta.glob('~/assets/images/**/*', {
    eager: true,
    import: 'default',
  })
  return assets['/assets/images/' + path]
}

const showGallery = () => { 
  const assets = import.meta.glob('~/assets/images/**/*', {
    eager: true,
    import: 'default',
  })
  const imagePath = Object.keys(assets)[imageNumber.value]
  return assets[imagePath]
}
//Usage: <img :src="useAsset(dynamic_image_name + '.svg')" alt="Discover Nuxt 3" />

// function   highlight() {
//     event.target.id = "theater";
//     theatrical.value = event.target.src;
// }

function   highlight() {
    event.target.id = "theater";
    let eventIterator = event.target.parentNode;
    while (eventIterator.previousElementSibling != null) {
      eventIterator.previousElementSibling.getElementsByTagName('img')[0].id = "";
      eventIterator = eventIterator.previousElementSibling;
    }
    eventIterator = event.target.parentNode;
    while (eventIterator.nextElementSibling != null) {
      eventIterator.nextElementSibling.getElementsByTagName('img')[0].id = "";
      eventIterator = eventIterator.nextElementSibling;
    }
    event.target.id = "theater";
    theatrical.value = event.target.src;
    console.log(theatrical.value)


  }

</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
.gallery {
  display: flex;
  justify-content: space-around;
}
img {
  width: 20%;
}

#theater {
  width: 40%;
}

#frame img {
  width: 70%;
}

</style>