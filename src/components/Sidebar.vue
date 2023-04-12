<script setup>
import CardBack from "./CardBack.vue"
import CardFront from "./CardFront.vue"

</script>
<template>
<div>
    <div class="hidden h-screen lg:w-80 xl:w-96 lg:flex flex-col justify-center space-y-6  " style="background-image: url('/bg-main-desktop.png')">
        <div class="relative left-28">
            <CardFront />
        </div>
        <div class="relative left-1/2">
            <CardBack />
        </div>  
    </div>
    <div class="h-[240px] sm:space-x-6 md:space-x-8 lg:hidden w-screen flex flex-col sm:flex-row items-center sm:justify-center bg-cover" style="background-image: url('/bg-main-mobile.png')">
      <div class="relative top-2 sm:top-20 ">
        <CardFront />
      </div>
      <div class="relative top-5 sm:top-32 ">
        <CardBack />
      </div>
    </div>
</div>
</template>