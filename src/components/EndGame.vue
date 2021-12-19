<template>
  <div class="popup">
      <div v-if="result"> The Winner is <h1>{{ result }}</h1></div>
      <div v-else>It's a draw!</div>
      <button class="reset size" @click="$emit('reset')">RESET</button>
  </div>
</template>

<script>
export default {
  name: "EndGame",
  props: ["result"]
}
</script>

<style>
.popup {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
    background-color: rgba(54,54,54,0.85);
    font-size: 5rem;
}
.size {
  font-size: 2rem;
}
</style>