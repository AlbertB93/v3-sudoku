

<template>
  <div class="containerApp__game__board__field" id="square1">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="4">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="3">
  </div>
  <div class="containerApp__game__board__field" id="square2">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="2">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="4">
  </div>
  <div class="containerApp__game__board__field" id="square3">
    <input type="text" class="inputField" value="2">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="6">
    <input type="text" class="inputField" value="4">
  </div>
  <div class="containerApp__game__board__field" id="square4">
    <input type="text" class="inputField" value="4">
    <input type="text" class="inputField" value="6">
    <input type="text" class="inputField" value="3">
    <input type="text" class="inputField" value="5">
    <input type="text" class="inputField" value="2">
    <input type="text" class="inputField" value="1">
  </div>
  <div class="containerApp__game__board__field" id="square5">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="6">
    <input type="text" class="inputField" value="5">
    <input type="text" class="inputField" value="2">
    <input type="text" class="inputField" value="0">
  </div>
  <div class="containerApp__game__board__field" id="square6">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="5">
    <input type="text" class="inputField" value="2">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="4">
    <input type="text" class="inputField" value="0">
  </div>
</template>

<script>



export default {
  name: 'BoardFields',
}


</script>

<style lang="scss" scoped>
.containerApp__game__board {

  &__field {
    align-content: space-around;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    font-size: 1rem;
    height: 30%;
    text-align: center;
    width: 48%;

    &__single {
      border: solid 1px white;
      font-size: 2rem;
      height: 30%;
      text-align: center;
      width: 30%;
    }
  }

  .inputField {
    background-color: rgb(55, 54, 54);
    color: whitesmoke;
    font-size: 24px;
    height: 40px;
    width: 25%;
    text-align: center;
  }

  .inputConfirmed {
    color: orange;
  }
}
</style>