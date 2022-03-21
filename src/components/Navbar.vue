<script setup>
import { ref } from 'vue'
import draggable from 'vuedraggable'
import Navbar from './Navbar.vue'
defineProps({
  msg: String
})

const count = ref(0)



</script>

<template >

<nav class="h-20 mb-20 w-full">
    The Todo App
</nav>

</template>
<script>


</script>
<style scoped>
nav {
    background-color: #FEC260;
    display:grid;
    place-items: center start;
    color: white;
    padding:15px;
}

</style>
