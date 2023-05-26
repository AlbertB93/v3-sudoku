
<template>
  <div class="containerApp__game--board__field" id="square0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="5">
    <input type="text" class="inputField" value="2">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="1">
  </div>
  <div class="containerApp__game--board__field" id="square1">
    <input type="text" class="inputField" value="3">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="4">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="6">
  </div>
  <div class="containerApp__game--board__field" id="square2">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
  </div>
  <div class="containerApp__game--board__field" id="square3">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="1">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
  </div>
  <div class="containerApp__game--board__field" id="square4">
    <input type="text" class="inputField" value="5">
    <input type="text" class="inputField" value="1">
    <input type="text" class="inputField" value="4">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="0">
    <input type="text" class="inputField" value="2">
  </div>
  <div class="containerApp__game--board__field" id="square5">
    <input type="text" class="inputField" value="2">
    <input type="text" class="inputField" value="6">
    <input type="text" class="inputField" value="3">
    <input type="text" class="inputField" value="1">
    <input type="text" class="inputField" value="4">
    <input type="text" class="inputField" value="5">
  </div>
</template>

<script>



export default {
  name: 'BoardFields',
}


</script>

<style lang="scss" scoped>
.containerApp__game--board {

  &__field {
    align-content: space-around;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    height: 20%;
    text-align: center;
    width: 48%;
    /*     border: solid 1px red; */

    &__single {
      border: solid 1px white;
      height: 30%;
      text-align: center;
      width: 30%;
    }
  }

  .inputField {

    background-color: transparent;
    color: whitesmoke;
    font-size: 1rem;
    font-weight: 600;
    height: auto;
    padding: 4px 2px;
    width: 25%;
    text-align: center;
  }

  .inputConfirmed {
    color: yellow;
  }
}
</style>