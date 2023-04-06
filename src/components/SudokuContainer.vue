<template>
  <div class="containerApp">
    <!--     <h1>Sudoku</h1> -->
    <!--     <ButtonsGame :fillBoard="fillBoard" /> -->
    <div class="containerApp__game">
      <ButtonsSingleGame :findEmptyFieldsInMatrix="findEmptyFieldsInMatrix" :findCorsToFillDigit="findCorsToFillDigit"
        :findMissingDigit="findMissingDigit" :fillMissingDigit="fillMissingDigit" :fillBoard="fillBoard"
        :secondStepFindOneMissingDigit="secondStepFindOneMissingDigit"
        :secondStepFindOneMissingID="secondStepFindOneMissingID" :secondStepFindCoor="secondStepFindCoor"
        :thirdStepFindOneMissingID="thirdStepFindOneMissingID" :thirdStepFindMissingDigits="thirdStepFindMissingDigits"
        :thirdStepFindCorsToFillDigit="thirdStepFindCorsToFillDigit"
        :thirdStepFillDigitsInRightPlace="thirdStepFillDigitsInRightPlace" :finishGame="finishGame" :firstStep="firstStep"
        :secondStep="secondStep" :thirdStep="thirdStep" />
      <div class="containerApp__game__board">
        <BoardFields />
      </div>
      <!--       <div class="containerApp__game__screen">
        <ScreenForSteps />
      </div> -->
    </div>
  </div>
</template>

<script>

/* import { computed } from "@vue/reactivity";
import { ref } from "vue";
import { games } from "../data/Games" */
/* import ButtonsGame from "../components/buttons/ButtonsGame.vue" */
import ButtonsSingleGame from "./buttons/ButtonsSingleGame.vue";
import BoardFields from "./BoardFields.vue";
/* import ScreenForSteps from "./ScreenForSteps.vue"; */

export default {
  name: 'SudokuContainer',
  components: {
    /*     ButtonsGame, */
    ButtonsSingleGame,
    BoardFields,
    /*  ScreenForSteps, */
  },

  setup() {

    //counters
    let i = 0;
    let j = 0;
    let counterRowForStep = 0;
    // counters for third step
    let tempCounter = 0;
    let tempCounterFinish = 0;
    // arrays for the help
    let rows = [];
    let columns = [];
    let yCorrdinate = 0;
    let xCorrdinate = 0;
    let xCor = 0;
    let yCor = 0;
    let missingDigit = 99;
    let countOfZero = 0;
    let endCondition = true;
    let miniMatrixs = [];
    let testArr = [1, 2, 3, 4, 5, 6]
    let copyArr = [];
    let idForSwitch = 99;
    let oldXCorrdinate = 99;

    // for the second help
    let missingID = 99;

    // for the third help

    let ySecondCor = 9;
    let secondMissingDigit = 99;

    //conditions

    let conFirstStep = true;
    let conSecondStep = true;
    let conFinishGame = false;

    function drawGame() {
      console.log("drawGame");
    }

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
    }

    // function, which sholud finish game for one click
    function finishGame() {
      console.log("finishGame");

      // dopóki warunek końca gry "conFinishGame" jeśli fałszywy to wykonujemy kroki
      // Step 1. Sprawdzamy czy isZero zwraca -1, jeśli tak to koniec gry, nie ma pola o wartości zero na planszy.
      // Step 2.
      // Step 3. Jeśli warunek dla firstStep jest fałszywy ( nie ma minimacierzy z jednym wolnym polem) oraz warunek dla secondStep jest fałszywy ( nie ma cyfry występującej na planszy n-1 raza)
      // Wtedy wykonujemy Step3. Step 3  - szuka minimacierzy, w której są dwa puste pola, znajduje jakich cyfr brakuje, znajduje dla nich współrzędne oraz wybiera odpowiednią cyfrę do wpisania.
      // Po wykonaniu kroku Step 3, ustawiamy znowu warunki "true" dla Step1 i Step 2 aby mogły się wykonać ( są prostsze do zrealizowania ).

      while (!conFinishGame) {
        while (conFirstStep) {
          firstStep();
          if (!isZero) {
            alert("Sudoku ROZWIĄZANE")
            break;
          }
        }
        while (conSecondStep) {
          secondStep();
          conFirstStep = true;
          if (!isZero) {
            alert("Sudoku ROZWIĄZANE")
            break;
          }

        }
        if (conFirstStep == false && conSecondStep == false) {
          thirdStep();
          if (!isZero) {
            alert("Sudoku ROZWIĄZANE")
            break;
          }
          conFirstStep = true;
          conSecondStep = true;
        }
      }






    }

    // function, which check is there any "O" in fields
    function isZero() {

      // przeszukujemy każde pole, jeśli znajdziemy 0 to ustawiamy warunek końca Sudoku na true.
      // Warunkiem kończącym sudoku jest conFinishGame ustawiony na false lub -1.

      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          if (miniMatrixs[i][j] == 0) {
            conFinishGame = true;
            return 1;
          } else {
            return -1;
          }
        }
      }
    }

    // functions for the main buttons


    // function, which fill interial minimatrix
    function fillBoard() {
      console.log("fillBoard")

      let tempArray = [];


      for (i = 1; i < 7; i++) {
        tempArray[i - 1] = document.getElementById('square' + i).getElementsByClassName('inputField');
      }

      // tworzenie 2 wyamirowej tablicy
      for (i = 0; i < 18; i++) {
        miniMatrixs[i] = [];
      }

      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          miniMatrixs[i][j] = tempArray[i][j].value;
        }
      }


      // wypełnienie tablicy rows oraz jej wypełnienie
      for (i = 0; i < 6; i++) {
        rows[i] = [];
      }

      for (i = 0; i < 5; i += 2) {
        for (j = 0; j < 3; j++) {
          rows[i][j] = tempArray[i][j].value;
        }
      }
      for (i = 1; i < 6; i += 2) {
        for (j = 3; j < 6; j++) {
          rows[i][j - 3] = tempArray[i - 1][j].value;
        }
      }
      for (i = 0; i < 5; i += 2) {
        for (j = 3; j < 6; j++) {
          rows[i][j] = tempArray[i + 1][j - 3].value;
        }
      }
      for (i = 1; i < 6; i += 2) {
        for (j = 3; j < 6; j++) {
          rows[i][j] = tempArray[i][j].value;
        }
      }


      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          miniMatrixs[i + 6][j] = rows[i][j];
        }
      }


      // tworzenie tablicy columns oraz jej wypełnienie
      for (i = 0; i < 6; i++) {
        columns[i] = [];
      }
      for (i = 0; i < 3; i++) {
        for (j = 0; j < 5; j += 2) {
          columns[i][j] = tempArray[j][i].value;
        }
      }
      for (i = 3; i < 6; i++) {
        for (j = 1; j < 6; j += 2) {
          columns[i][j] = tempArray[j][i].value;
        }
      }
      for (i = 0; i < 3; i++) {
        for (j = 1; j < 6; j += 2) {
          columns[i][j] = tempArray[j - 1][i + 3].value;
        }
      }
      for (i = 3; i < 6; i++) {
        for (j = 0; j < 5; j += 2) {
          columns[i][j] = tempArray[j + 1][i - 3].value;
        }
      }

      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          miniMatrixs[i + 12][j] = columns[i][j];
        }
      }

      /*       console.log("KOMPLETNA MINIMATRIX")
            for (i = 0; i < 18; i++) {
              console.log(miniMatrixs[i])
            }
       */



    }

    // function, which fill right number for right place in BoardGame
    function fillMissingDigit() {
      console.log("fillMissingDigit")


      document.getElementById('square' + xCor).getElementsByClassName('inputField')[yCor].value = missingDigit;

      /*       setTimeout(() => {
              document.getElementById('square' + xCor).getElementsByClassName('inputField')[yCor].classList.add("inputConfirmed");
            }, 5000) */

      document.getElementById('square' + xCor).getElementsByClassName('inputField')[yCor].classList.add("inputConfirmed");

      /*       document.getElementById('rowOfStep' + counterRowForStep).innerHTML = "Krok " + (counterRowForStep + 1) + " . Cyfra [" + missingDigit + "] w pole o współrzędnych [" + xCorrdinate + "] [" + yCorrdinate + "]";
            counterRowForStep++; */


    }

    // FUNCTIONS FOR THE FIRST STEP

    // function, which find miniMatrix, which has one empty field and return ID.
    function findEmptyFieldsInMatrix() {
      console.log("findEmptyFieldsInMatrix");

      // znajdywanie 0 w małych macierzach i dopisywanie ich do tablicy.

      endCondition = true;
      while (endCondition) {
        for (i = 0; miniMatrixs.length; i++) {
          for (j = 0; j < 6; j++) {
            if (miniMatrixs[i][j] == 0) {
              countOfZero++;
            }
          }
          if (countOfZero == 1) {
            xCorrdinate = i;
            endCondition = false;
            break;
          } else if (i == 17 && countOfZero != 1) {
            endCondition = false;
            alert(" Nie ma macierzy z 1 wolnym miejscem!")
            return -1;
            /* break; */
          } else {
            countOfZero = 0;
          }
        }

      }

      console.log("Współrzędna X: " + xCorrdinate)
      endCondition = true;
      countOfZero = 0;



    }

    // function, which find corrdinates in miniMatrix for the empty field
    function findCorsToFillDigit() {

      // sprawdzenie jakiej cyfry brakuje w macierzy
      console.log("findCorsToFillDigit");

      console.log(miniMatrixs[xCorrdinate]);

      yCorrdinate = miniMatrixs[xCorrdinate].indexOf('0');
      console.log(yCorrdinate);

      console.log("współrzędne punktu: " + xCorrdinate + " i " + yCorrdinate);


      // ustalamy poprawną współrzędną X do wpisania w square o ID

      if (xCorrdinate < 6) {
        xCor = xCorrdinate + 1;
        yCor = yCorrdinate;
      } else if (xCorrdinate == 6 || xCorrdinate == 7) {
        if (yCorrdinate < 3) {
          xCor = 1;
        } else {
          xCor = 2;
        }
      } else if (xCorrdinate == 8 || xCorrdinate == 9) {
        if (yCorrdinate < 3) {
          xCor = 3;
        } else {
          xCor = 4;
        }
      } else if (xCorrdinate == 10 || xCorrdinate == 11) {
        if (yCorrdinate < 3) {
          xCor = 5;
        } else {
          xCor = 6;
        }
      } else if (xCorrdinate == 12 || xCorrdinate == 13 || xCorrdinate == 14) {
        if (yCorrdinate < 2) {
          xCor = 1;
        } else if (yCorrdinate >= 2 && yCorrdinate < 4) {
          xCor = 3;
        } else {
          xCor = 5;
        }
      } else if (xCorrdinate == 15 || xCorrdinate == 16 || xCorrdinate == 17) {
        if (yCorrdinate < 2) {
          xCor = 2;
        } else if (yCorrdinate >= 2 && yCorrdinate < 4) {
          xCor = 4;
        } else {
          xCor = 6;
        }
      }

      // ustalamy poprawną współrzędną Y do wpisania w square o ID

      if (xCorrdinate == 12 || xCorrdinate == 15) {
        switch (yCorrdinate) {
          case 0:
            yCor = yCorrdinate;
            break;
          case 1:
            yCor = yCorrdinate + 2;
            break;
          case 2:
            yCor = yCorrdinate - 2;
            break;
          case 3:
            yCor = yCorrdinate;
            break;
          case 4:
            yCor = yCorrdinate - 4;
            break;
          case 5:
            yCor = yCorrdinate - 2;
            break;
        }
      } else if (xCorrdinate == 13 || xCorrdinate == 16) {
        switch (yCorrdinate) {
          case 0:
            yCor = yCorrdinate + 1;
            break;
          case 1:
            yCor = yCorrdinate + 3;
            break;
          case 2:
            yCor = yCorrdinate - 1;
            break;
          case 3:
            //            yCor = yCorrdinate + 2;
            yCor = yCorrdinate + 1;
            break;
          case 4:
            yCor = yCorrdinate - 3;
            break;
          case 5:
            yCor = yCorrdinate - 1;
            break;
        }
      } else if (xCorrdinate == 14 || xCorrdinate == 17) {
        switch (yCorrdinate) {
          case 0:
            yCor = yCorrdinate + 2;
            break;
          case 1:
            yCor = yCorrdinate + 4;
            break;
          case 2:
            yCor = yCorrdinate;
            break;
          case 3:
            yCor = yCorrdinate + 2;
            break;
          case 4:
            yCor = yCorrdinate - 2;
            break;
          case 5:
            yCor = yCorrdinate;
            break;
        }
      } else if (xCorrdinate == 6 || xCorrdinate == 8 || xCorrdinate == 10) {
        if (yCorrdinate < 3) {
          yCor = yCorrdinate;
        } else {
          yCor = yCorrdinate - 3;
        }
      } else if (xCorrdinate == 7 || xCorrdinate == 9 || xCorrdinate == 11) {
        if (yCorrdinate < 3) {
          yCor = yCorrdinate + 3;
        } else {
          yCor = yCorrdinate;
        }
      }
      console.log(" Nowe współrzędne punktu: " + xCor + " i " + yCor);

    }

    // function, which find one missing number in miniMatrix, which has one empty field.
    function findMissingDigit() {
      console.log("FindMissingDigit");

      for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
        copyArr[i] = miniMatrixs[xCorrdinate][i];
      }

      copyArr.sort();
      console.log("macierz szukana: " + copyArr)
      console.log("macierz wzorcowa: " + testArr)

      for (i = 0; i < 6; i++) {
        if (copyArr[i + 1] != testArr[i]) {
          console.log("Brakuje cyfry: " + testArr[i]);
          miniMatrixs[xCorrdinate][yCorrdinate] = testArr[i];
          missingDigit = testArr[i];
          break;
        }
      }
      console.log(miniMatrixs[xCorrdinate]);
    }



    // FUNCTIONS FOR THE SECOND STEP

    // function for the buttons on the left, help buttons second 


    // function, which find one number , which occurs in Sudoku n-1 times
    function secondStepFindOneMissingDigit() {
      console.log("secondStepFindOneMissingDigit");

      let arrayOfDigits = [0, 0, 0, 0, 0, 0]

      let numberCondition = false;

      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          switch (miniMatrixs[i][j]) {
            case '0':
              break;
            case '1':
              arrayOfDigits[0] += 1;
              break;
            case '2':
              arrayOfDigits[1] += 1;
              break;
            case '3':
              arrayOfDigits[2] += 1;
              break;
            case '4':
              arrayOfDigits[3] += 1;
              break;
            case '5':
              arrayOfDigits[4] += 1;
              break;
            case '6':
              arrayOfDigits[5] += 1;
              break;
            default:
              console.log("DEFAULT")
          }
        }
      }
      console.log(" Tablica numerków " + arrayOfDigits)

      for (i = 0; i < arrayOfDigits.length; i++) {
        if (arrayOfDigits[i] == 5) {
          missingDigit = i + 1;
          console.log(" Element Tablica numerków " + arrayOfDigits[i])
          numberCondition = false;
          break;
        }
        numberCondition = true;
      }


      if (numberCondition) {
        alert("Nie ma cyfry występujące n-1 razy")
        return -1;
      } else {
        console.log(" Szukany numerek: " + missingDigit)
      }



    }

    // function, which find miniMatrix ID for one number , which occurs in Sudoku n-1 times. This function return matrix ID, which wanted number isn't occur. 
    function secondStepFindOneMissingID() {
      console.log("secondStepFindOneMissingID")


      console.log("missing DIGIT: " + missingDigit)


      for (i = 0; i < 17; i++) {
        if (miniMatrixs[i].some(item => item == missingDigit) == false) {
          missingID = i + 1;
          break;
        }
      }
      console.log("missing ID: " + missingID)

    }

    // function, which find corrdinates in miniMatrix for the empty field and wanted number.
    function secondStepFindCoor() {
      console.log("secondStepFindCoor")
      console.log("MISSING ID: " + missingID)

      xCor = missingID;


      // ZMIENIĆ NA SWITHC DLA KAŻDEGO MISSING ID BO MUSIMY DODAĆ KOLEJNY WARUNEK ABY SPRAWDZAŁ W PIONIE ORAZ POZIOMIE

      /*       for (i = 0; i < 6; i++) {
              if (missingID == 1 || missingID == 3 || missingID == 5) {
                if (miniMatrixs[missingID][i] == 0) {
                  switch (i) {
                    case 0:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[12].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 0);
                          yCor = 0;
                          break;
                        }
                      }
                      break;
                    case 1:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[13].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 1);
                          yCor = 1;
                          break;
                        }
                      }
                      break;
                    case 2:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[14].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 2);
                          yCor = 2;
                          break;
                        }
                      }
                      break;
                    case 3:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[12].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 3)
                          yCor = 3;
                          break;
                        }
                      }
                      break;
                    case 4:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[13].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 4);
                          yCor = 4;
                          break;
                        }
                      }
                      break;
                    case 5:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[14].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 5);
                          yCor = 5;
                          break;
                        }
                      }
                      break;
                  }
                }
              } else if (missingID == 2 || missingID == 4 || missingID == 6) {
                console.log("Druga czesc drabinki")
                if (miniMatrixs[missingID-1][i] == 0) {
                  switch (i) {
                    case 0:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[15].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 0);
                          yCor = 0;
                          break;
                        }
                      }
                      break;
                    case 1:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[16].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 1);
                          yCor = 1;
                          break;
                        }
                      }
                      break;
                    case 2:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[17].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 2);
                          yCor = 2;
                          break;
                        }
                      }
                      break;
                    case 3:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[15].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 3)
                          yCor = 3;
                          break;
                        }
                      }
                      break;
                    case 4:
                      for (j = 0; j < 6; j++) {
                        if ((miniMatrixs[16].some(item => item == missingDigit)) || (miniMatrixs[7].some(item => item == missingDigit))) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 4);
                          yCor = 4;
                          break;
                        }
                      }
                      break;
                    case 5:
                      for (j = 0; j < 6; j++) {
                        if (miniMatrixs[17].some(item => item == missingDigit)) {
                          break;
                        } else {
                          console.log("ZAWIERA SZUKANA CYFRE" + 5);
                          yCor = 5;
                          break;
                        }
                      }
                      break;
                  }
                }
              } else {
                console.log("ERROR - WRONG ID")
              }
            } */
      idForSwitch = missingID;
      missingID = missingID - 1;


      for (i = 0; i < 6; i++) {
        switch (idForSwitch) {
          case 1:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 2:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 3:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 4:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 5:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 6:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          default: {
            console.log("Błąd w pętli  z ID")
            break;
          }
        }
      }

      console.log("Szukane współrzędne: " + xCor + " - " + yCor)

    }

    // FUNCTIONS FOR THE THIRD STEP

    function thirdStepFindOneMissingID() {
      console.log("thirdStepFindOneMissingID")



      endCondition = true;
      while (endCondition) {
        for (i = 0; i < miniMatrixs.length; i++) {
          for (j = 0; j < 6; j++) {
            if (miniMatrixs[i][j] == 0) {
              countOfZero++;
            }
          }
          if (countOfZero == 2 && oldXCorrdinate != i) {
            xCorrdinate = i;
            endCondition = false;
            break;
          } else if (i == 17 && countOfZero != 1) {
            endCondition = false;
            alert(" Nie ma macierzy z 2 wolnym miejscem!")
            break;
          } else {
            countOfZero = 0;
          }
        }

      }

      console.log("Współrzędna macierzy z dwoma zerami: " + xCorrdinate)
      oldXCorrdinate = xCorrdinate;
      console.log("oldXCorrdinate: " + oldXCorrdinate)
      endCondition = true;
      countOfZero = 0;

    }

    function thirdStepFindMissingDigits() {
      console.log("thirdStepFindMissingDigits")

      missingDigit = 99;
      secondMissingDigit = 99;

      console.log("macierz szukana: " + miniMatrixs[xCorrdinate])

      for (i = 1; i < 7; i++) {
        if (miniMatrixs[xCorrdinate].some(item => item == i) == false) {
          if (missingDigit == 99) {
            missingDigit = i;
          } else {
            secondMissingDigit = i;
          }
        }
      }

      console.log("Brakuje cyfr: " + missingDigit + " oraz " + secondMissingDigit);
    }

    function thirdStepFindCorsToFillDigit() {

      // sprawdzenie jakiej cyfry brakuje w macierzy
      console.log("thirdStepFindCorsToFillDigit");

      console.log("Macierz z dwoma pustymi polami" + miniMatrixs[xCorrdinate]);

      let copyArr = [];

      yCorrdinate = miniMatrixs[xCorrdinate].indexOf('0');
      console.log(yCorrdinate);
      console.log("współrzędne 1 punktu: " + xCorrdinate + " i " + yCorrdinate);

      // We copied our matrix to temporary matrix (copyArr) and when I found index for "0", I fill to it expample number(9).
      // Next, I repeat function to found second cor for '0'
      for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
        copyArr[i] = miniMatrixs[xCorrdinate][i];
      }
      copyArr[yCorrdinate] = 9;
      ySecondCor = copyArr.indexOf('0');
      console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);
    }

    function thirdStepFillDigitsInRightPlace() {
      console.log("thirdStepFillDigitsInRightPlace")

      console.log("Brakuje cyfr: " + missingDigit + " oraz " + secondMissingDigit);
      console.log("współrzędne 1 punktu: " + xCorrdinate + " i " + yCorrdinate);
      console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);
      console.log("--------------------------------------------");
      missingID = xCorrdinate;
      idForSwitch = xCorrdinate + 1;

      console.log("MissingID: " + missingID);
      console.log("idForSwitch: " + idForSwitch);
      console.log("--------------------------------------------");

      tempCounter = 0;


      for (i = 0; i < 6; i++) {
        switch (idForSwitch) {
          case 1:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 2:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE tutaj" + 3)
                      tempCounter++;
                      yCor = 3;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 3:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 4:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 5:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 6:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          default: {
            console.log("Błąd w pętli  z ID")
            break;
          }
        }
      }

      if (tempCounter == 2) {
        console.log("tempCounter = 2. Musimy wykonać dla drugiej cyfry ")
        tempCounterFinish++;
        missingDigit = secondMissingDigit;
      } else {
        console.log("Musimy wpisać : " + missingDigit)
        xCor = xCorrdinate + 1;
        yCor = yCorrdinate;
        console.log("Prawidłowe współrzędne do wpisania: " + xCor + " oraz " + yCor)
      }

      for (i = 0; i < 6; i++) {
        switch (idForSwitch) {
          case 1:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 2:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE tutaj" + 3)
                      tempCounter++;
                      yCor = 3;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[7].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 3:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 4:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[8].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[9].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 5:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      tempCounter++;
                      yCor = 0;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      tempCounter++;
                      yCor = 1;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      tempCounter++;
                      yCor = 2;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[12].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      tempCounter++;
                      yCor = 3;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[14].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          case 6:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 0);
                      yCor = 0;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 1:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 1);
                      yCor = 1;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 2:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[10].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 2);
                      yCor = 2;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 3:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[15].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 3)
                      yCor = 3;
                      tempCounter++;
                      break;
                    }
                  }
                  break;
                case 4:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[16].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 4);
                      tempCounter++;
                      yCor = 4;
                      break;
                    }
                  }
                  break;
                case 5:
                  for (j = 0; j < 6; j++) {
                    if ((miniMatrixs[11].some(item => item == missingDigit)) || (miniMatrixs[17].some(item => item == missingDigit))) {
                      break;
                    } else {
                      console.log("ZAWIERA SZUKANA CYFRE" + 5);
                      tempCounter++;
                      yCor = 5;
                      break;
                    }
                  }

              }
            } break;
          default: {
            console.log("Błąd w pętli  z ID")
            break;
          }
        }
      }

      if (tempCounter == 4) {
        console.log("tempCounter = 2. Musimy wykonać dla innej Macierzy ")
      
        tempCounterFinish++;
        /* thirdStepFindOneMissingID(); */
      } else {
        console.log("Musimy wpisać : " + missingDigit)
        xCor = xCorrdinate + 1;
        yCor = yCorrdinate;
        console.log("Prawidłowe współrzędne do wpisania: " + xCor + " oraz " + yCor)
      }
      console.log("tempCounterFinish: " + tempCounterFinish)
    }


    function firstStep() {
      fillBoard();
      if (findEmptyFieldsInMatrix() != -1) {
        console.log("KROK NR 1")
        findCorsToFillDigit();
        findMissingDigit();
        fillMissingDigit();
      } else {
        alert(" Wykonaj krok nr 2")
        console.log(" Nie wykonano kroku nr 1")
        conFirstStep = false;
      }
    }

    function secondStep() {

      fillBoard();

      if (secondStepFindOneMissingDigit() != -1) {
        console.log("KROK NR 2")
        secondStepFindOneMissingID();
        secondStepFindCoor();
        fillMissingDigit();
        conFirstStep = true;
      } else {
        fillBoard();
        alert(" Wykonaj krok nr 3")
        conSecondStep = false;
        conFirstStep = false;
        console.log(" Nie wykonano kroku nr 2")
      }
    }

    function thirdStep() {
      console.log("KROK NR 3")
      fillBoard();
      thirdStepFindOneMissingID();
      thirdStepFindMissingDigits();
      thirdStepFindCorsToFillDigit();
      thirdStepFillDigitsInRightPlace();
      if(tempCounter == 4){
              thirdStepFindOneMissingID();
                    thirdStepFindMissingDigits();
        thirdStepFindCorsToFillDigit();
        thirdStepFillDigitsInRightPlace();
           fillMissingDigit();
      } else {
        if (tempCounterFinish == 2) {
          thirdStepFillDigitsInRightPlace();
          fillMissingDigit();
        } else {
          fillMissingDigit();
        }
      }


    }




    return { drawGame, sprawdz, confirmBoardGame, nextMove, checkGame, finishGame, fillBoard, findEmptyFieldsInMatrix, findCorsToFillDigit, findMissingDigit, fillMissingDigit, secondStepFindOneMissingDigit, secondStepFindOneMissingID, secondStepFindCoor, thirdStepFindOneMissingID, thirdStepFindMissingDigits, thirdStepFindCorsToFillDigit, thirdStepFillDigitsInRightPlace, counterRowForStep, firstStep, secondStep, thirdStep, isZero }
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

  .containerApp__game {

    align-items: center;
    border: solid 3px whitesmoke;
    border-radius: 8px;
    color: aqua;
    display: flex;
    flex-direction: row;
    height: 800px;
    justify-content: space-around;
    margin: 20px auto;
    width: 90vw;

    .containerApp__game__board {
      align-content: space-around;
      border: solid 3px whitesmoke;
      border-radius: 8px;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;
      height: 400px;
      margin: 20px;
      width: 400px;
    }

  }
}
</style>
