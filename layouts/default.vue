<template>
    <div class="flex flex-col w-screen overflow-y-auto overflow-x-hidden h-screen">
    <Navbar />
              
        <div class="bg-gray-100">
  
            <main class="font-sans flex-grow w-full h-full py-2 sm:p-4 sm:mx-auto sm:my-5 md:my-10 max-w-7xl">
                
                <slot />
            
            </main>

        </div>
    </div>
</template>