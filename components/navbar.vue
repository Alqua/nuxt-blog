
<template>
    <nav class="flex items-center justify-between flex-wrap sm:mx-auto p-6">
        <div class="flex items-center flex-no-shrink text-indigo-900 mr-6">
          <NuxtLink to="/" class="font-semibold text-xl sm:text-4xl tracking-tight">My Site </NuxtLink>
        </div>
        <div class="block sm:hidden">
          <button @click="toggle" class="flex items-center text-indigo-900 hover:text-indigo">
            <!-- <svg class="fill-current h-3 w-3" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><title>Menu</title><path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"/></svg> -->
            <svg class="fill-current" viewBox="0 0 100 80" width="30" height="30">
              <title>Menu</title>
              <rect width="100" height="10"></rect>
              <rect y="30" width="100" height="10"></rect>
              <rect y="60" width="100" height="10"></rect>
          </svg>
          </button>
        </div>
        <div :class="showMobileMenu ? 'block': 'hidden'" class="w-full flex-grow sm:flex sm:mx-20 sm:items-center sm:w-auto">
          <div class="text-xl sm:flex-grow">
            <NuxtLink to="/" @click="toggleClose()" class="no-underline block mt-4 sm:inline-block sm:mt-0 mr-4">
              Les Projets
            </NuxtLink>
            <NuxtLink to="/" @click="toggleClose()" class="no-underline block mt-4 sm:inline-block sm:mt-0 mr-4">
              Le Blog
            </NuxtLink>
          </div>
          <!-- <div>
            <a href="#" class="no-underline inline-block text-sm px-4 py-2 leading-none  text-black  mt-4 sm:mt-0">
              Le Blog
            </a>
          </div>
    
          <div>
            <a href="#" class="no-underline inline-block text-sm px-4 py-2 leading-none border rounded text-black border-black hover:border-transparent hover:text-teal hover:bg-white mt-4 sm:mt-0">
              Download
            </a>
          </div> -->
    
        </div>
        
      </nav>
      </template>
    
    <script setup>
    const showMobileMenu = ref(false)
    const toggle = () =>{
        showMobileMenu.value = !showMobileMenu.value
    }
    const toggleClose = () =>{
        showMobileMenu.value = false
    }
    </script>