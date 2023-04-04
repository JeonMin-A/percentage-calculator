<template>
    <font-awesome-icon :icon="isDark ? 'cloud-sun' : 'moon'" class="text-2xl absolute top-5 right-5 darkMode dark:text-white dark:transition-all dark:duration-500 cursor-pointer" @click="$emit('dark')" />
    <!-- <font-awesome-icon icon="cloud-sun" class="text-2xl" /> -->
</template>
<script>
export default {
    name: "NavPage",
    data() {
        return {
            
        }
    },
    props:{
    isDark: Boolean
    }
}
</script>
<style>
    
</style>