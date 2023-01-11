<script setup lang="ts">
// const toggleColorScheme = (input) => {
//     if
// }

</script>

<template>
  <fieldset>
    <div>
      <label for="toggle-dark"><slot name="enable-dark-mode"></slot></label>
      <!-- <input type="radio" name="color-scheme" id="toggle-dark" :onchange=toggleColorScheme({e}) /> -->
    </div>
    <div>
      <label for="toggle-light"><slot name="disable-dark-mode"></slot></label>
      <input type="radio" name="color-scheme" id="toggle-light" onchange="toggleColorScheme" />
    </div>
  </fieldset>
</template>

<style scoped>
fieldset {
  border: none;
  padding: 0.25em;
  position: absolute;
  display: flex;
  gap: 0.25em;
  top: 3em;
  left: 85%;
  z-index: 5;
}

input {
  appearance: none;
}

label {
  cursor: pointer;
  z-index: 3;
}

label:has(~ input[checked]) {
  display: none;
}

</style>
