<script setup>




</script>

<template >

<span class="filterbar bg-opacity-20 p-1">
<span class="text-white">{{remaining}} Tasks left</span>

<span class="filterbuttons">
  <button @click="filterAll" class="border-none text-white filterbutton">All </button>
 <button @click="filterActive" class="border-none text-white filterbutton">Active Tasks</button>
  <button @click="filterDone" class="border-none text-white filterbutton" >Done</button>
</span>
</span>
</template>
<script>


</script>
<style scoped>
.filterbar {
  background-color: #fec16020;
}

.filterbar {
  display: flex;
  justify-content: space-between;
}

.filterbuttons {
  display: grid;
  grid-template-columns: auto auto auto;
  grid-column-gap: 10px;
  font-size:15px
}

.filterbuttons button{
  background-color:#ffffff1e;
  text-align: center;
  padding:0px 5px 0px 5px;
}
.filterbuttons button:hover{
  background-color:#ff00007a;
  text-align: center;
  padding:0px 5px 0px 5px;
}

</style>
