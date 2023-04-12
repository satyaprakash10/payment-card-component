<script setup>
const props = defineProps({
    title: {
        required: true,
        default: "Confirm"
    }
})
</script>

<template>
    <button 
        class="w-full mb-6 md:mb-0 bg-dark-orchid font-normal hover:bg-opacity-80 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline" 
        type="button"
    >
        {{ title }} 
    </button>
</template>