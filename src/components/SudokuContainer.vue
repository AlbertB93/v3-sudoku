<template>
  <div class="containerApp">
    <!--     <h1>Sudoku</h1> -->
    <ButtonsSingleGame :findEmptyFieldsInMatrix="findEmptyFieldsInMatrix" :findCorsToFillDigit="findCorsToFillDigit"
      :findMissingDigit="findMissingDigit" :fillMissingDigit="fillMissingDigit" :fillBoard="fillBoard"
      :secondStepFindOneMissingDigit="secondStepFindOneMissingDigit"
      :secondStepFindOneMissingID="secondStepFindOneMissingID" :secondStepFindCoor="secondStepFindCoor"
      :thirdStepFindOneMissingID="thirdStepFindOneMissingID" :thirdStepFindMissingDigits="thirdStepFindMissingDigits"
      :thirdStepFindCorsToFillDigit="thirdStepFindCorsToFillDigit"
      :thirdStepFillDigitsInRightPlace="thirdStepFillDigitsInRightPlace" :finishGame="finishGame" :firstStep="firstStep"
      :secondStep="secondStep" :thirdStep="thirdStep" :fourthStepFindOneMissingID="fourthStepFindOneMissingID"
      :fourthStepFindMissingDigits="fourthStepFindMissingDigits"
      :fourthStepFindCorsToFillDigit="fourthStepFindCorsToFillDigit"
      :fourthStepFillDigitsInRightPlace="fourthStepFillDigitsInRightPlace"
      :fourthStep="fourthStep"
      />
    <div class="containerApp__game">

      <ButtonsGame :fillBoard="fillBoard" :drawGame="drawGame" />
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

/* import { computed } from "@vue/reactivity";*/
/*  import { ref } from "vue";  */
import { games6x6LevelEasy, games6x6LevelHard } from "../data/Games"
import ButtonsGame from "../components/buttons/ButtonsGame.vue";
import ButtonsSingleGame from "./buttons/ButtonsSingleGame.vue";
import BoardFields from "./BoardFields.vue";
/* import ScreenForSteps from "./ScreenForSteps.vue"; */

export default {
  name: 'SudokuContainer',
  components: {
    ButtonsGame,
    ButtonsSingleGame,
    BoardFields,
    /*  ScreenForSteps, */
  },

  setup() {

    // to draw Game
    const gamesLevelEasy = games6x6LevelEasy;
    const gamesLevelHard = games6x6LevelHard;


    //counters
    let i = 0;
    let j = 0;
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
    let oldestXCorrdinate = 99;

    // for the second help
    let missingID = 99;

    // for the third help
    let ySecondCor = 9;
    let secondMissingDigit = 99;

    // for the fourth help
    let thirdMissingDigit = 99;
    let yThirdCor = 9;
    let warunekPom = true;

    let arrayOfCondition = [true, true, true]

    //conditions

    let conFirstStep = true;
    let conSecondStep = true;
    let conThirdStep = true;
    let conFinishGame = false;

    //FUNCTIONS

    //SPRAWDZIĆ
    function drawGame(exampleNumber) {
      console.log("drawGame");
      let tablica = document.getElementsByClassName('inputField');
      console.log(tablica);

      for (let i = 0; i < tablica.length; i++) {
        tablica[i].value = gamesLevelEasy[exampleNumber][i];
      }

    }
    //SPRAWDZIĆ
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
    //SPRAWDZIĆ
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

    // function, which sholud finish game for one click     // ALREADY CHECK
    function finishGame() {
      console.log("finishGame");
      while (!conFinishGame) {
        while (conFirstStep) {
          console.log("***************** KROK 1 ************************")
          firstStep();
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break;
          }
          conThirdStep = true;
        }
        while (conSecondStep) {
          console.log("***************** KROK 2 ************************")
          secondStep();
          conThirdStep = true;
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break;
          }
        }
        if (conFirstStep == false && conSecondStep == false && conThirdStep == true) {
          console.log("***************** KROK 3 ************************")
          isZero();
          conFirstStep = true;
          conSecondStep = true;
          thirdStep();
        }
      }
    }

    // function, which check is there any "O" in fields     // ALREADY CHECK
    function isZero() {
      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          if (miniMatrixs[i][j] == 0) {
            conFinishGame = false;
            return 1;
          }
        }
      }
      conFirstStep, conSecondStep, conThirdStep = false;
      conFinishGame = true;
      return -1;
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
    }

    // function, which fill right number for right place in BoardGame
    function fillMissingDigit() {
      console.log("fillMissingDigit")
      console.log("================ WPISANO [ " + missingDigit + " ] ====== [ " + xCor + " ] oraz [ " + yCor + " ] =======")

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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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

      xCorrdinate = 0;
      countOfZero = 0;

      /*       dodać 3 miejsce jako oldestCorrdinate */


      console.log("Poprzednia współrzędna macierzy z dwoma zerami: " + oldXCorrdinate)
      console.log("Najstarsza współrzędna macierzy z dwoma zerami: " + oldestXCorrdinate)

      endCondition = true;
      while (endCondition) {
        for (i = 0; i < miniMatrixs.length; i++) {
          for (j = 0; j < 6; j++) {
            if (miniMatrixs[i][j] == 0) {
              countOfZero++;
            }
          }
          if (countOfZero == 2 && oldXCorrdinate != i && oldestXCorrdinate != i) {
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
      console.log("Aktualna współrzędna macierzy z dwoma zerami: " + xCorrdinate)

      endCondition = true;
      countOfZero = 0;

    }

    function thirdStepFindMissingDigits() {
      console.log("thirdStepFindMissingDigits")

      missingDigit = 99;
      secondMissingDigit = 99;

      console.log("Macierz szukana: " + miniMatrixs[xCorrdinate])

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

      /* console.log("Macierz z dwoma pustymi polami" + miniMatrixs[xCorrdinate]); */

      let copyArr = [];

      yCorrdinate = miniMatrixs[xCorrdinate].indexOf('0');

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

      missingID = xCorrdinate;
      idForSwitch = xCorrdinate + 1;

      console.log("MissingID: " + missingID);
      console.log("idForSwitch: " + idForSwitch);


      tempCounter = 0;
      tempCounterFinish = 0;

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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli:  tutaj" + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                      console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
        /* yCor = yCorrdinate; */
        console.log("Prawidłowe współrzędne do wpisania: " + xCor + " oraz " + yCor)
      }

      if (tempCounter == 2) {
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                        console.log("Nie zawiera szukanej cyfry czyli:  tutaj" + 3)
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 0);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 1);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 2);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 3)
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 4);
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
                        console.log("Nie zawiera szukanej cyfry czyli: " + 5);
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
      }

      if (tempCounter == 4) {
        console.log("tempCounter = 4. Musimy wykonać dla innej Macierzy ")
        tempCounterFinish++;
        /* thirdStepFindOneMissingID(); */
      } else {
        console.log("Musimy wpisać : " + missingDigit)
        xCor = xCorrdinate + 1;
        /* yCor = yCorrdinate; */
        console.log("Prawidłowe współrzędne do wpisania: " + xCor + " oraz " + yCor)
      }
      console.log("tempCounterFinish: " + tempCounterFinish)
    }

    // FUNCTIONS FOR THE FOURTH STEP

    function fourthStepFindOneMissingID() {
      console.log("thirdStepFindOneMissingID")

      xCorrdinate = 0;
      countOfZero = 0;

      console.log("xCorrdinate przed wejściem do pętli: " + xCorrdinate)


      endCondition = true;
      while (endCondition) {
        for (i = 0; i < miniMatrixs.length; i++) {
          for (j = 0; j < 6; j++) {
            if (miniMatrixs[i][j] == 0) {
              countOfZero++;
            }
          }
          if (countOfZero == 3 && oldXCorrdinate != i) {
            xCorrdinate = i;
            endCondition = false;
            break;
          } else if (i == 17 && countOfZero != 1) {
            endCondition = false;
            alert(" Nie ma macierzy z 3 wolnym miejscem!")
            break;
          } else {
            countOfZero = 0;
          }
        }

      }

      console.log("Aktualna współrzędna macierzy z trzema zerami: " + xCorrdinate)
      oldXCorrdinate = xCorrdinate;
      console.log("Poprzednia współrzędna macierzy z trzema zerami: " + oldXCorrdinate)
      endCondition = true;
      countOfZero = 0;

    }

    function fourthStepFindMissingDigits() {
      console.log("thirdStepFindMissingDigits")

      missingDigit = 99;
      secondMissingDigit = 99;
      thirdMissingDigit = 99;

      console.log("macierz szukana: " + miniMatrixs[xCorrdinate])

      for (i = 1; i < 7; i++) {
        if (miniMatrixs[xCorrdinate].some(item => item == i) == false) {
          if (missingDigit == 99) {
            missingDigit = i;
          } else if (secondMissingDigit == 99) {
            secondMissingDigit = i;
          } else {
            thirdMissingDigit = i;
          }
        }
      }

      console.log("Brakuje cyfr: " + missingDigit + " oraz " + secondMissingDigit + " oraz " + thirdMissingDigit);
    }

    function fourthStepFindCorsToFillDigit() {

      // sprawdzenie jakiej cyfry brakuje w macierzy
      console.log("thirdStepFindCorsToFillDigit");

      /* console.log("Macierz z dwoma pustymi polami" + miniMatrixs[xCorrdinate]); */

      let copyArr = [];

      yCorrdinate = miniMatrixs[xCorrdinate].indexOf('0');

      console.log("współrzędne 1 punktu: " + xCorrdinate + " i " + yCorrdinate);

      // We copied our matrix to temporary matrix (copyArr) and when I found index for "0", I fill to it expample number(9).
      // Next, I repeat function to found second cor for '0'
      for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
        copyArr[i] = miniMatrixs[xCorrdinate][i];
      }
      copyArr[yCorrdinate] = 9;
      ySecondCor = copyArr.indexOf('0');
      console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);

      for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
        copyArr[i] = miniMatrixs[xCorrdinate][i];
      }
      copyArr[yCorrdinate] = 9;
      copyArr[ySecondCor] = 9;
      yThirdCor = copyArr.indexOf('0');
      console.log("współrzędne 3 punktu: " + xCorrdinate + " i " + yThirdCor);

    }

    function fourthStepFillDigitsInRightPlace() {
      console.log("***fourthStepFillDigitsInRightPlace-2-***")


      /*       zrobić pętle aby się wykonywało dopóki nie będzie counterCondition == 1. Jak będzie to ustawić warunekPetli na false. */


      /*  1. Pętla dopóki counterCondition będzie równy 1 wtedy ....................  
          2. Wykonać fourtStepHelp dla 3 brakujących liczb i dla 1 msc zerowego ( yCorrdinate )
          3. Jeśli arrayOfCondition[i] będzie prawdziwy to zwiększ counterCondition 
          4. Jeśli counterCondition będzie == 1 to break. Musimy jakoś wyrzucić tą brakującą liczbę i to miejsce zerowe, dla którego będzie to prawdziwe
          5. Jeśli nie powtórz pętla ale zmień miejsce zerowe , np.  z yCorrdinate  na ySecondCor.*/

      let counterCondition = 0;
      let warunekPetli = true;


      while (warunekPetli) {

        console.log(" JESTEM TU ?")
        arrayOfCondition[0] = fourthStepHelp(missingDigit, yCorrdinate);
        arrayOfCondition[1] = fourthStepHelp(secondMissingDigit, yCorrdinate);
        arrayOfCondition[2] = fourthStepHelp(thirdMissingDigit, yCorrdinate);

        for (i = 0; i < 3; i++) {
          if (arrayOfCondition[i]) {
            counterCondition++;
          }
        }

        console.log(" ILE WYNOSI COUNTER? " + counterCondition)
        console.log(" JAKI WARUNEK  conForFirstDigit ???" + arrayOfCondition[0])
        console.log(" JAKI WARUNEK  conForSecondDigit ???" + arrayOfCondition[1])
        console.log(" JAKI WARUNEK  conForThirdDigit ???" + arrayOfCondition[2])


        if (counterCondition == 1) {
          if (arrayOfCondition[0]) {
          /*   missingDigit = missingDigit; */
            warunekPetli = false;
            break;
          } else if (arrayOfCondition[1]) {
            missingDigit = secondMissingDigit;
            warunekPetli = false;
            break;
          } else {
            missingDigit = thirdMissingDigit
            warunekPetli = false;
            break;
          }

        } else {
          if (yCorrdinate != ySecondCor) {
            yCorrdinate = ySecondCor;
            counterCondition = 0;
            console.log(" JESTEM TU sec?")
            console.log(" yCoordinate  = " + yCorrdinate)
            continue;
          } else {
            yCorrdinate = yThirdCor;
            counterCondition = 0;
            console.log(" JESTEM TU thi?")
            console.log(" yCoordinate  = " + yCorrdinate)
            continue;
          }
        }


      }

     /*  xCor = xCorrdinate + 1; */
      yCor = yCorrdinate;
      xCor = xCorrdinate+1;

      console.log("Współrzędne do wpisania: " + xCor + " i " + yCor)
   

    }


    function fourthStepHelp(missingDigit, yCorrdinate) {
      warunekPom = true;
      switch (xCorrdinate) {
        case 0:
          if (yCorrdinate == 0) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[6][i] == missingDigit || miniMatrixs[12][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 1) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[6][i] == missingDigit || miniMatrixs[13][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 2) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[6][i] == missingDigit || miniMatrixs[14][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 3) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[7][i] == missingDigit || miniMatrixs[12][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 4) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[7][i] == missingDigit || miniMatrixs[13][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 5) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[7][i] == missingDigit || miniMatrixs[14][i] == missingDigit) {
                warunekPom = false;
              }
            }
          }
          break;
        case 1:
          if (yCorrdinate == 0) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[6][i] == missingDigit || miniMatrixs[15][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 1) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[6][i] == missingDigit || miniMatrixs[16][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 2) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[6][i] == missingDigit || miniMatrixs[17][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 3) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[7][i] == missingDigit || miniMatrixs[15][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 4) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[7][i] == missingDigit || miniMatrixs[16][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 5) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[7][i] == missingDigit || miniMatrixs[17][i] == missingDigit) {
                warunekPom = false;
              }
            }
          }
          break;
        case 2:
          if (yCorrdinate == 0) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[8][i] == missingDigit || miniMatrixs[12][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 1) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[8][i] == missingDigit || miniMatrixs[13][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 2) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[8][i] == missingDigit || miniMatrixs[14][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 3) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[9][i] == missingDigit || miniMatrixs[12][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 4) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[9][i] == missingDigit || miniMatrixs[13][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 5) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[9][i] == missingDigit || miniMatrixs[14][i] == missingDigit) {
                warunekPom = false;
              }
            }
          }
          break;
        case 3:
          if (yCorrdinate == 0) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[8][i] == missingDigit || miniMatrixs[15][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 1) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[8][i] == missingDigit || miniMatrixs[16][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 2) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[8][i] == missingDigit || miniMatrixs[17][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 3) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[9][i] == missingDigit || miniMatrixs[15][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 4) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[9][i] == missingDigit || miniMatrixs[16][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 5) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[9][i] == missingDigit || miniMatrixs[17][i] == missingDigit) {
                warunekPom = false;
              }
            }
          }
          break;
        case 4:
          if (yCorrdinate == 0) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[10][i] == missingDigit || miniMatrixs[12][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 1) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[10][i] == missingDigit || miniMatrixs[13][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 2) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[10][i] == missingDigit || miniMatrixs[14][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 3) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[11][i] == missingDigit || miniMatrixs[12][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 4) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[11][i] == missingDigit || miniMatrixs[13][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 5) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[11][i] == missingDigit || miniMatrixs[14][i] == missingDigit) {
                warunekPom = false;
              }
            }
          }
          break;
        case 5:
          if (yCorrdinate == 0) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[10][i] == missingDigit || miniMatrixs[15][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 1) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[10][i] == missingDigit || miniMatrixs[16][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 2) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[10][i] == missingDigit || miniMatrixs[17][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 3) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[11][i] == missingDigit || miniMatrixs[15][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 4) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[11][i] == missingDigit || miniMatrixs[16][i] == missingDigit) {
                warunekPom = false;
              }
            }
          } else if (yCorrdinate == 5) {
            for (i = 0; i < 6; i++) {
              if (miniMatrixs[11][i] == missingDigit || miniMatrixs[17][i] == missingDigit) {
                warunekPom = false;
              }
            }
          }
          break;

      }

      return warunekPom;
    }


    // FINALLY STEPS

    function firstStep() {
      fillBoard();
      if (findEmptyFieldsInMatrix() != -1) {
        console.log("********** KROK NR 1 **********")
        findCorsToFillDigit();
        findMissingDigit();
        fillMissingDigit();
        conSecondStep = true;
      } else {
        alert(" Wykonaj krok nr 2")
        console.log(" Nie wykonano kroku nr 1")
        conFirstStep = false;
        conSecondStep = true;
      }

    }

    function secondStep() {

      fillBoard();

      if (secondStepFindOneMissingDigit() != -1) {
        console.log("****** KROK NR 2 *******")
        secondStepFindOneMissingID();
        secondStepFindCoor();
        fillMissingDigit();
        conFirstStep = true;
        conSecondStep = false;
      } else {
        fillBoard();
        alert(" Wykonaj krok nr 3")
        conSecondStep = false;
        conFirstStep = false;
        console.log(" Nie wykonano kroku nr 2")
      }
    }

    function thirdStep() {
      console.log("******** KROK NR 3 ******")
      fillBoard();
      thirdStepFindOneMissingID();
      thirdStepFindMissingDigits();
      thirdStepFindCorsToFillDigit();
      thirdStepFillDigitsInRightPlace();
      if (tempCounter == 4) {
        console.log("----------TEST------- JAKIE OLDXCOORR?" + oldXCorrdinate)
        if (oldXCorrdinate == 99) {
          oldXCorrdinate = xCorrdinate;
        } else {
          oldestXCorrdinate = oldXCorrdinate;
          oldXCorrdinate = xCorrdinate;
        }
        thirdStepFindOneMissingID();
        thirdStepFindMissingDigits();
        thirdStepFindCorsToFillDigit();
        thirdStepFillDigitsInRightPlace();
        if (tempCounter == 4) {
          console.log("----------TEST---2------- JAKIE OLDXCOORR?" + oldXCorrdinate)
          if (oldXCorrdinate == 99) {
            oldXCorrdinate = xCorrdinate;
          } else {
            oldestXCorrdinate = oldXCorrdinate;
            oldXCorrdinate = xCorrdinate;
          }
          thirdStepFindOneMissingID();
          thirdStepFindMissingDigits();
          thirdStepFindCorsToFillDigit();
          thirdStepFillDigitsInRightPlace();
          conFirstStep = true;
          conSecondStep = true;
          fillMissingDigit();
        } else {
          thirdStepFindOneMissingID();
          thirdStepFindMissingDigits();
          thirdStepFindCorsToFillDigit();
          thirdStepFillDigitsInRightPlace();
          conFirstStep = true;
          conSecondStep = true;
          fillMissingDigit();
        }
      } else {
        if (tempCounterFinish == 2) {
          thirdStepFillDigitsInRightPlace();
          conFirstStep = true;
          conSecondStep = true;
          fillMissingDigit();

        } else {
          conFirstStep = true;
          conSecondStep = true;
          fillMissingDigit();

        }
      }


    }

    function fourthStep(){
            console.log("******** KROK NR 4 ******")
            fourthStepFindOneMissingID();
            fourthStepFindMissingDigits();
            fourthStepFindCorsToFillDigit();
            fourthStepFillDigitsInRightPlace();
            fillMissingDigit();
    }



    return {
      drawGame, confirmBoardGame, nextMove, finishGame, fillBoard, findEmptyFieldsInMatrix, findCorsToFillDigit, findMissingDigit, fillMissingDigit, secondStepFindOneMissingDigit,
      secondStepFindOneMissingID, secondStepFindCoor, thirdStepFindOneMissingID, thirdStepFindMissingDigits, thirdStepFindCorsToFillDigit, thirdStepFillDigitsInRightPlace, firstStep, secondStep, thirdStep, isZero,
      gamesLevelEasy, gamesLevelHard, fourthStepFindOneMissingID, fourthStepFindMissingDigits, fourthStepFindCorsToFillDigit, fourthStepFillDigitsInRightPlace,fourthStep
    }
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
  /*   border: solid 3px yellow; */
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
    /*    border: dotted 3px red; */
    border-radius: 8px;
    color: aqua;
    display: flex;
    flex-direction: row;
    height: 480px;
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
