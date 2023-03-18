<template>
  <div class="containerApp">
    <h1>Sudoku</h1>
    <div class="containerApp__game__buttons">
      <div class="containerApp__game__buttons--title">
        buttons game
        <p>Gra o id [ indexGame]: {{ indexGame }}</p>
      </div>
      <div class="containerApp__game__buttons--box">
        <!--         <button class="buttons--game" @click="drawGame">Wylosuj numer gry</button>
        <button class="buttons--game" @click="drawGame">Zacznij rozgrywkę od nowa</button>
        <button class="buttons--game" @click="drawGame">Nowa gra</button>
        <button class="buttons--game" @click="drawGame">Sprawdź planszę</button>
        <button class="buttons--game" @click="drawGame">Włącz podpowiedź</button> -->
      </div>
      <div class="containerApp__game__buttons--box">
        <button class="buttons--game" @click="confirmBoardGame">Zatwierdź planszę</button>
        <button class="buttons--game" @click="nextMove">Wykonaj kolejny ruch</button>
        <button class="buttons--game" @click="checkGame">Sprawdź planszę - mały kwadracik</button>
        <button class="buttons--game" @click="finishGame">Rozwiąż planszę</button>
        <button class="buttons--game" @click="fillBoard">Wypełnij mini macierze</button>
      </div>
    </div>
    <div class="containerApp__game" v-for="game in gamesArr" :key="game.id">
      Numer tablicy: {{ game.id }}
      <div class="containerApp__game__helpButtons">
        <p> Guziki do pomocy:</p>
        <button class="buttons--help" @click="findEmptyFieldsInMatrix"> Podaj macierze z ilością pustych miejsc</button>
        <button class="buttons--help" @click="findCorsToFillDigit"> Znajdź współrzędne na wpisanie brakującej
          cyfry</button>
        <button class="buttons--help" @click="findMissingDigit"> Sprawdź jakiej cyfry brakuje</button>
        <button class="buttons--help" @click="fillMissingDigit"> Wpisz brakującą cyfrę</button>
      </div>
      <div class="containerApp__game__board">
        <div class="containerApp__game__board__field" id="square1">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="4">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="5">
          <input type="text" class="inputField" value="3">
        </div>
        <div class="containerApp__game__board__field" id="square2">
          <input type="text" class="inputField" value="3">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="0">
        </div>
        <div class="containerApp__game__board__field" id="square3">
          <input type="text" class="inputField" value="3">
          <input type="text" class="inputField" value="1">
          <input type="text" class="inputField" value="4">
          <input type="text" class="inputField" value="5">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="0">
        </div>
        <div class="containerApp__game__board__field" id="square4">
          <input type="text" class="inputField" value="6">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="5">
          <input type="text" class="inputField" value="4">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="1">
        </div>
        <div class="containerApp__game__board__field" id="square5">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="6">
          <input type="text" class="inputField" value="5">
          <input type="text" class="inputField" value="4">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="0">
        </div>
        <div class="containerApp__game__board__field" id="square6">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="4">
          <input type="text" class="inputField" value="3">
          <input type="text" class="inputField" value="5">
          <input type="text" class="inputField" value="0">
          <input type="text" class="inputField" value="2">
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import { computed } from "@vue/reactivity";
import { ref } from "vue";
import { games } from "../data/Games"

export default {
  name: 'SudokuContainer',

  setup() {

    const gamesArr = ref([...games]);
    let indexGame = ref(99);

    let arrayOfInputElements = document.getElementsByClassName('inputField');
    let arrayOfValue = [];
    let arrayOfZero = [6];
    let i = 0;
    let j = 0;
    let miniSquares = [];
    let rows = [];
    let columns = [];
    let square = [];


    function drawGame() {
      indexGame.value = Math.floor(Math.random() * 10 + 1);
      console.log(indexGame);
    }

    const testValue = computed(() =>
      gamesArr.value.filter((gamesArr.value > 0)))

    function sprawdz() {
      let t1 = document.querySelector("#s91");
      let t2 = document.querySelector("#s92");
      let t3 = document.querySelector("#s93");

      console.log("TESTING" + "t1: " + t1.value + t2.value + t3.value)
    }

    function confirmBoardGame() {
      console.log("confirmBoardGame");

      let tablica = document.getElementsByClassName('inputField');
      console.log(tablica);

      for (let i = 0; i < tablica.length; i++) {
        if (tablica[i].value != 0) {
          tablica[i].classList.add("inputConfirmed");
          tablica[i].setAttribute('readonly', 'true');
        }

      }
    }

    function nextMove() {
      console.log("nextMove");

      console.log("Jeśli 1,2,3 to biały kolor");
      let t1 = document.querySelector("#s91");
      let t2 = document.querySelector("#s92");
      let t3 = document.querySelector("#s93");
      console.log("wartość t3" + t3.value);
      if (t1.value == 1 && t2.value == 2 && t3.value == 3) {
        console.log("warunek spełniony");
        t1.classList.add("inputConfirmed");
        t2.classList.add("inputConfirmed");
        t3.classList.add("inputConfirmed");
        console.log("wartość t3" + t3.value);
      } else {
        console.log("Błąd")
      }
    }

    function checkGame() {
      console.log("checkGame");

      for (let i = 0; i < arrayOfInputElements.length; i++) {
        arrayOfValue[i] = arrayOfInputElements[i].value;
      }
      arrayOfValue.sort();
      console.log(arrayOfValue);


      if (arrayOfValue.includes('0')) {
        console.log("Macierz nie wypełniona")
      } else if (arrayOfValue[0] == '1') {
        console.log("Pierwszy element to 1")
      } else {
        console.log("KONIEC")
      }
      console.log(arrayOfValue);



    }

    function finishGame() {
      console.log("finishGame");


    }

    function fillBoard() {
      console.log("fillBoard");

      // zapisanie do tablicy małych macierzy square[i]
      for (i = 1; i < 7; i++) {
        miniSquares[i - 1] = document.getElementById('square' + i).getElementsByClassName('inputField');
      }

      // wypełnienie tablicy rows oraz jej wypełnienie
      for (i = 0; i < 6; i++) {
        square[i] = [];
      }

      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          square[i][j] = miniSquares[i][j].value;
        }
      }

      /*       for (i = 0; i < miniSquares.length; i++) {
              console.log("MiniSquares " + square[i]);
            } */

      // wypełnienie tablicy rows oraz jej wypełnienie
      for (i = 0; i < 6; i++) {
        rows[i] = [];
      }

      for (i = 0; i < 5; i += 2) {
        for (j = 0; j < 3; j++) {
          rows[i][j] = miniSquares[i][j].value;
        }
      }
      for (i = 1; i < 6; i += 2) {
        for (j = 3; j < 6; j++) {
          rows[i][j - 3] = miniSquares[i - 1][j].value;
        }
      }
      for (i = 0; i < 5; i += 2) {
        for (j = 3; j < 6; j++) {
          rows[i][j] = miniSquares[i + 1][j - 3].value;
        }
      }
      for (i = 1; i < 6; i += 2) {
        for (j = 3; j < 6; j++) {
          rows[i][j] = miniSquares[i][j].value;
        }
      }
      /*       for (i = 0; i < rows.length; i++) {
              console.log("Rows " + rows[i]);
            } */

      // tworzenie tablicy columns oraz jej wypełnienie
      for (i = 0; i < 6; i++) {
        columns[i] = [];
      }
      for (i = 0; i < 3; i++) {
        for (j = 0; j < 5; j += 2) {
          columns[i][j] = miniSquares[j][i].value;
        }
      }
      for (i = 3; i < 6; i++) {
        for (j = 1; j < 6; j += 2) {
          columns[i][j] = miniSquares[j][i].value;
        }
      }
      for (i = 0; i < 3; i++) {
        for (j = 1; j < 6; j += 2) {
          columns[i][j] = miniSquares[j - 1][i + 3].value;
        }
      }
      for (i = 3; i < 6; i++) {
        for (j = 0; j < 5; j += 2) {
          columns[i][j] = miniSquares[j + 1][i - 3].value;
        }
      }
      /*       for (i = 0; i < columns.length; i++) {
              console.log("Columns " + columns[i]);
            } */

    }

    function findEmptyFieldsInMatrix() {
      console.log("findEmptyFieldsInMatrix");

      let countOfZero = 0;

      // znajdywanie 0 w małych macierzach i dopisywanie ich do tablicy.
      for (i = 0; i < square.length; i++) {
        for (j = 0; j < 6; j++) {
          if (square[i][j] == 0) {
            countOfZero++;
          }
        }
        arrayOfZero[i] = countOfZero;
        countOfZero = 0;
      }
      console.log("Jest jedno zero w macierzy square, indeks: " + arrayOfZero.indexOf(1))


      for (i = 0; i < rows.length; i++) {
        for (j = 0; j < 6; j++) {
          if (rows[i][j] == 0) {
            countOfZero++;
          }
        }
        arrayOfZero[i] = countOfZero;
        countOfZero = 0;
      }
      console.log("Jest jedno zero w macierzy rows, indeks: " + arrayOfZero.indexOf(1))


      for (i = 0; i < columns.length; i++) {
        for (j = 0; j < 6; j++) {
          if (columns[i][j] == 0) {
            countOfZero++;
          }
        }
        arrayOfZero[i] = countOfZero;
        countOfZero = 0;
      }
      console.log("Jest jedno zero w macierzy columns, indeks: " + arrayOfZero.indexOf(1))

    }

    function findCorsToFillDigit() {

            // sprawdzenie jakiej cyfry brakuje w macierzy
      let corX = arrayOfZero.indexOf(1);
      console.log(corX);

      console.log(rows[corX]);

      let corY = rows[corX].indexOf('0');
      console.log(corY);

      console.log("współrzędne punktu: " + corX + " i " + corY);

            console.log("tablica z pustym miejscem" + rows[corX]);


      /*       for (i = 0; i < rows[corX].length; i++) {
              rows[corX][i]++;
            }
            console.log("Nowa tablica" + rows[corX]); */

      let testArr = [0, 1, 2, 3, 4, 5]

      let copyArr = rows.slice();
      console.log("Skopiowana tablica" + copyArr[corX]);
      copyArr[corX].sort();
      console.log("Skopiowana posortowana tablica" + copyArr[corX]);

      for (i = 0; i < 6; i++) {
        if (copyArr[corX][i] != testArr[i]) {
          console.log("spełniony   " + testArr[i])
          rows[corX][corY] = testArr[i];
          break;
        }
      }

      console.log(rows[corX]);
    }



    return { gamesArr, drawGame, indexGame, testValue, sprawdz, confirmBoardGame, nextMove, checkGame, finishGame, fillBoard, findEmptyFieldsInMatrix, findCorsToFillDigit }
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}



.containerApp {
  align-items: center;
  background-color: rgb(24, 23, 23);
  display: flex;
  flex-direction: column;
  height: 100vh;
  justify-content: center;
  text-align: center;
  width: 100vw;

  h1 {
    color: rgb(84, 210, 210);
    height: 50px;
    padding-top: 10px;
    text-align: center;
    width: 90vw;
  }


  .containerApp__game__buttons {
    display: flex;
    border: solid 3px yellowgreen;
    flex-direction: column;
    height: 200px;
    margin: 20px;
    width: 90vw;

    &--title {
      color: yellowgreen;
      text-align: center;
    }

    &--box {
      align-items: center;
      display: flex;
      flex-direction: row;
      justify-content: space-around;

      .buttons--game {
        background-color: aqua;
        border: solid 2px white;
        border-radius: 12px;
        cursor: pointer;
        font-size: 16px;
        font-weight: 600;
        height: auto;
        margin: 10px 0px;
        padding: 12px;
        width: auto;
      }
    }

    .inputField {
      background-color: red;
      font-size: 24px;
      height: 40px;
      width: 40px;
    }

  }

  .containerApp__game {

    align-items: center;
    border: solid 3px aqua;
    color: aqua;
    display: flex;
    flex-direction: row;
    height: 600px;
    /*     justify-content: center; */
    justify-content: space-around;
    margin: 20px auto;
    width: 90vw;


    .containerApp__game__board {
      align-content: space-around;
      border: solid 3px yellowgreen;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
      height: 500px;
      margin: 20px;
      width: 500px;

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
        background-color: yellowgreen;
        font-size: 24px;
        height: 40px;
        width: 25%;
        text-align: center;
      }

      .inputConfirmed {
        color: white;
      }
    }

    .containerApp__game__helpButtons {
      border: solid 1px red;
      height: 500px;
      width: 300px;

      .buttons--help {
        margin: 20px;
      }
    }

  }



}
</style>
