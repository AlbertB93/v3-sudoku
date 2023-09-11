
<template>
  <div class="containerApp__game--board__field" id="square0">
    <SingleBoardField />
  </div>
  <div class="containerApp__game--board__field" id="square1">
      <SingleBoardField />
  </div>
  <div class="containerApp__game--board__field" id="square2">
      <SingleBoardField />
  </div>
  <div class="containerApp__game--board__field" id="square3">
      <SingleBoardField />
  </div>
  <div class="containerApp__game--board__field" id="square4">
      <SingleBoardField />
  </div>
  <div class="containerApp__game--board__field" id="square5">
      <SingleBoardField />
  </div>
</template>

<script>
import SingleBoardField from "./SingleBoardField.vue"
export default {
  name: 'BoardFields',
  components: { SingleBoardField },
}
</script>

<style lang="scss" scoped>
.containerApp__game--board {
  &__field {
    align-content: space-around;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    height: 35%;
    text-align: center;
    width: 48%;
  }
}
</style>