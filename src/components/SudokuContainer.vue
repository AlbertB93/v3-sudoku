<template>
  <div class="containerApp">
    <div class="containerApp__title">
      <h1>Sudoku - how to resolve game...
      </h1>
    </div>
    <div class="containerApp__box">
      <div class="containerApp__box--container">
        <div class="containerApp__buttons">
          <ButtonsGame :drawGame="drawGame" :newGame="newGame" :finishGame="finishGame" :fillBoard="fillBoard" />
        </div>
        <div class="containerApp__game--steps">
          <ButtonsSteps :findEmptyFieldsInMatrix="findEmptyFieldsInMatrix"
            :firstStepFindEmptyField="firstStepFindEmptyField" :findMissingDigit="findMissingDigit"
            :fillMissingDigit="fillMissingDigit" :secondStepFindOneMissingDigit="secondStepFindOneMissingDigit"
            :secondStepFindOneMissingID="secondStepFindOneMissingID" :secondStepFindCoor="secondStepFindCoor"
            :findMiniMatrixWithZeros="findMiniMatrixWithZeros" :findMissingDigits="findMissingDigits"
            :findCorsToFillDigitExtend="findCorsToFillDigitExtend" :fillDigitInRightPlace="fillDigitInRightPlace"
            :firstStep="firstStep" :findCorsToFillDigit="findCorsToFillDigit" :secondStep="secondStep"
            :anotherStep="anotherStep" />
        </div>
        <div class="containerApp__game">
          <div class="containerApp__game--board">
            <BoardFields />
          </div>
        </div>
      </div>
      <div class="containerApp__screen">
        <ScreenForSteps />
      </div>
    </div>
  </div>
</template>

<script>
import { games6x6LevelEasy, games6x6LevelMedium, games6x6LevelHard } from "../data/Games"
import ButtonsGame from "../components/buttons/ButtonsGame.vue";
import ButtonsSteps from "./buttons/ButtonsSteps.vue";
import BoardFields from "./BoardFields.vue";
import ScreenForSteps from "./ScreenForSteps.vue";

export default {
  name: 'SudokuContainer',
  components: {
    ButtonsGame,
    ButtonsSteps,
    BoardFields,
    ScreenForSteps,
  },

  setup() {
    //counters
    let i = 0;
    let j = 0;
    let counterArrayOfZero = 0;
    let counterStepstoFinish = 0;
    let countOfZero = 0;
    let counterCondition = 0;
    let localCounterCondition = 0;
    let counterExecutedTimes = 0;

    // to draw Game
    const gamesLevelEasy = games6x6LevelEasy;
    const gamesLevelMedium = games6x6LevelMedium;
    const gamesLevelHard = games6x6LevelHard;
    let arrayOfInputs;
    let numberOfGame = 0;

    //conditions
    let conFinishGame = false;
    let endCondition = true;
    let conIsAvaiable = true;
    let conOfLoop = true;

    // other
    let missingDigit = 99;
    let missingID = 99;
    let xCor = 0;
    let yCor = 0;
    let ySecondCor = 9;
    let yThirdCor = 9;
    let yFourthCor = 9;
    let yCorrdinate = 0;
    let xCorrdinate = 0;
    let numberStepDecrease = 0;

    // arrays
    let miniMatrixs = [];
    let rows = [];
    let columns = [];
    let arrayOfZero = [];
    let arrayOfMissingDigits = [];
    let arrayDefault = [1, 2, 3, 4, 5, 6]
    let arrayForHelp = [];
    let arrayOfConditions = [true, true, true]
    let arrayOfStepsEnding = [true, true, true, true, true]


    // function, which removes style from board and fill "0" to all fields. // ALREADY CHECK
    function newGame() {
      for (i = 0; i < 6; i++) {
        for (j = 0; j < 6; j++) {
          document.getElementById('square' + i).getElementsByClassName('inputField')[j].classList.remove("inputConfirmed");
        }
      }
      for (let i = 0; i < arrayOfInputs.length; i++) {
        arrayOfInputs[i].value = 0;
      }
    }

    //function, which draw game. One of three levels and one of ten examples. // ALREADY CHECK
    /*     function drawGame(levelOfGame) {
          console.log("drawGame");
          arrayOfInputs = document.getElementsByClassName('inputField');
          numberOfGame = Math.floor(Math.random() * 10 + 1);
          switch (levelOfGame) {
            case 'easy':
              {
                for (let i = 0; i < arrayOfInputs.length; i++) {
                  arrayOfInputs[i].value = gamesLevelEasy[numberOfGame][i];
                }
              }
              break;
            case 'medium':
              {
                for (let i = 0; i < arrayOfInputs.length; i++) {
                  arrayOfInputs[i].value = gamesLevelMedium[numberOfGame][i];
                }
              }
    
              break;
            case 'hard':
              {
                for (let i = 0; i < arrayOfInputs.length; i++) {
                  arrayOfInputs[i].value = gamesLevelHard[numberOfGame][i];
                }
              }
              break;
            default:
              alert("Error during drawing Game")
              break;
          }
          fillBoard();
        }
     */
    function drawGame(levelOfGame, numberGame) {
      console.log("drawGame");
      arrayOfInputs = document.getElementsByClassName('inputField');
      numberOfGame = Math.floor(Math.random() * 10 + 1);
      switch (levelOfGame) {
        case 'easy':
          {
            for (let i = 0; i < arrayOfInputs.length; i++) {
              arrayOfInputs[i].value = gamesLevelEasy[numberOfGame][i];
            }
          }
          break;
        case 'medium':
          {
            for (let i = 0; i < arrayOfInputs.length; i++) {
              arrayOfInputs[i].value = gamesLevelMedium[numberGame][i];
            }
          }

          break;
        case 'hard':
          {
            for (let i = 0; i < arrayOfInputs.length; i++) {
              arrayOfInputs[i].value = gamesLevelHard[numberGame][i];
            }
          }
          break;
        default:
          alert("Error during drawing Game")
          break;
      }
      fillBoard();
    }

    // function, which sholud finish game for one click     // CHECK ONCE AGAIN
    function finishGame() {
      console.log("finishGame");
      fillBoard();
      ety: while (!conFinishGame) {
        while (arrayOfStepsEnding[0]) {
          console.log("***************** KROK 1 ************************")
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break ety;
          } else {
            firstStep();
          }
        }
        while (arrayOfStepsEnding[1]) {
          console.log("***************** KROK 2 ************************")
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break ety;
          } else {
            secondStep();
          }
        }
        while (arrayOfStepsEnding[2]) {
          console.log("**numberStepDecrease**" + numberStepDecrease)
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break ety;
          } else {
            anotherStep(2);
          }
        }
        while (arrayOfStepsEnding[3]) {
          console.log("**numberStepDecrease**" + numberStepDecrease)
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break ety;
          } else {
            anotherStep(3);
          }
        }
        while (arrayOfStepsEnding[4]) {
          console.log("**numberStepDecrease**" + numberStepDecrease)
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break ety;
          } else {
            anotherStep(4);
          }
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
      for (i = 0; i < arrayOfStepsEnding.length; i++) {
        arrayOfStepsEnding[i] = false;
      }
      conFinishGame = true;
      return -1;
    }

    // function, which fill interial minimatrix
    function fillBoard() {
      console.log("fillBoard")

      let tempArray = [];
      for (i = 0; i < 6; i++) {
        tempArray[i] = document.getElementById('square' + i).getElementsByClassName('inputField');
      }

      // create two dimensions array
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

      document.getElementById('square' + xCor).getElementsByClassName('inputField')[yCor].classList.add("inputConfirmed");

      document.getElementById('rowOfStep' + counterStepstoFinish).innerHTML = (counterStepstoFinish + 1) + " . Cyfra [" + missingDigit + "] w pole o współrzędnych [" + xCorrdinate + "] [" + yCorrdinate + "]";
      counterStepstoFinish++;
    }



    // FUNCTIONS FOR THE FIRST STEP

    // function, which find miniMatrix, which has one empty field and return ID.
    function findEmptyFieldsInMatrix() {
      console.log("findEmptyFieldsInMatrix");
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
          } else {
            countOfZero = 0;
          }
        }
      }
      console.log("Współrzędna X: " + xCorrdinate)
      endCondition = true;
      countOfZero = 0;
    }

    function firstStepFindEmptyField() {
      yCorrdinate = miniMatrixs[xCorrdinate].indexOf('0');
      console.log("Współrzędna: " + yCorrdinate)
    }

    // function, which find corrdinates in miniMatrix for the empty field
    function findCorsToFillDigit() {
      console.log("findCorsToFillDigit");
      console.log("Pierwotne współrzędne punktu: " + xCorrdinate + " i " + yCorrdinate);

      if (xCorrdinate < 6) {
        xCor = xCorrdinate;
        yCor = yCorrdinate;
      } else if (xCorrdinate == 6 || xCorrdinate == 7) {
        if (yCorrdinate < 3) {
          xCor = 0;
        } else {
          xCor = 1;
        }
      } else if (xCorrdinate == 8 || xCorrdinate == 9) {
        if (yCorrdinate < 3) {
          xCor = 2;
        } else {
          xCor = 3;
        }
      } else if (xCorrdinate == 10 || xCorrdinate == 11) {
        if (yCorrdinate < 3) {
          xCor = 4;
        } else {
          xCor = 5;
        }
      } else if (xCorrdinate == 12 || xCorrdinate == 13 || xCorrdinate == 14) {
        if (yCorrdinate < 2) {
          xCor = 0;
        } else if (yCorrdinate >= 2 && yCorrdinate < 4) {
          xCor = 2;
        } else {
          xCor = 4;
        }
      } else if (xCorrdinate == 15 || xCorrdinate == 16 || xCorrdinate == 17) {
        if (yCorrdinate < 2) {
          xCor = 1;
        } else if (yCorrdinate >= 2 && yCorrdinate < 4) {
          xCor = 3;
        } else {
          xCor = 5;
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
        arrayForHelp[i] = miniMatrixs[xCorrdinate][i];
      }
      arrayForHelp.sort();
      for (i = 0; i < 6; i++) {
        if (arrayForHelp[i + 1] != arrayDefault[i]) {
          console.log("Brakuje cyfry: " + arrayDefault[i]);
          miniMatrixs[xCorrdinate][yCorrdinate] = arrayDefault[i];
          missingDigit = arrayDefault[i];
          break;
        }
      }
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

      for (i = 0; i < arrayOfDigits.length; i++) {
        if (arrayOfDigits[i] == 5) {
          missingDigit = i + 1;
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

      for (i = 0; i < 17; i++) {
        if (miniMatrixs[i].some(item => item == missingDigit) == false) {
          missingID = i;
          break;
        }
      }
      console.log("missing ID: " + missingID)
    }

    // function, which find corrdinates in miniMatrix for the empty field and wanted number.
    function secondStepFindCoor() {
      console.log("secondStepFindCoor")
      xCor = missingID;
      helpForCorrectCorrdinates(missingID)
      console.log("Szukane współrzędne: " + xCor + " - " + yCor)

    }

    function helpForCorrectCorrdinates(missingID) {
      console.log("helpForCorrectCorrdinates")
      for (i = 0; i < 6; i++) {
        switch (missingID) {
          case 0:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(6, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(6, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(7, 12, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(7, 13, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(7, 14, i);
              }
            } break;
          case 1:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 15, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(6, 16, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(6, 17, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(7, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(7, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(7, 17, i);
              }
            } break;
          case 2:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(8, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(8, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 12, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(9, 13, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(9, 14, i);
              }
            } break;
          case 3:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(8, 15, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(8, 16, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 17, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(9, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(9, 17, i);
              }
            } break;
          case 4:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(10, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(10, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(10, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(11, 12, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(11, 13, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 14, i);
              }
            } break;
          case 5:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(10, 15, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(10, 16, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(10, 17, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(11, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(11, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 17, i);
              }
            } break;
          case 6:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(6, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(6, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(6, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(6, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(6, 17, i);
              }
            } break;
          case 7:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(7, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(7, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(7, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(7, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(7, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(7, 17, i);
              }
            } break;
          case 8:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(8, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(8, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 13, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(8, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(8, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(8, 17, i);
              }
            } break;
          case 9:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(9, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(9, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(9, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(9, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(9, 17, i);
              }
            } break;
          case 10:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(10, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(10, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(10, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(10, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(10, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(10, 17, i);
              }
            } break;
          case 11:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(11, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(11, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(11, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(11, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(11, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 17, i);
              }
            } break;
          case 12:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 12, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(7, 12, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 12, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 12, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(10, 12, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(10, 12, i);
              }
            } break;
          case 13:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 13, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(7, 13, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 13, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 13, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(10, 13, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 13, i);
              }
            } break;
          case 14:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 14, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(7, 14, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 14, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 14, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(10, 14, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 14, i);
              }
            } break;
          case 15:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 15, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(7, 15, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 15, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 15, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(10, 15, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 15, i);
              }
            } break;
          case 16:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 16, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(7, 16, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 16, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 16, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(10, 16, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 16, i);
              }
            } break;
          case 17:
            if (miniMatrixs[missingID][i] == 0) {
              switch (i) {
                case 0:
                  checkMiniMatrixForMissingDigit(6, 17, i);
                  break;
                case 1:
                  checkMiniMatrixForMissingDigit(7, 17, i);
                  break;
                case 2:
                  checkMiniMatrixForMissingDigit(8, 17, i);
                  break;
                case 3:
                  checkMiniMatrixForMissingDigit(9, 17, i);
                  break;
                case 4:
                  checkMiniMatrixForMissingDigit(10, 17, i);
                  break;
                case 5:
                  checkMiniMatrixForMissingDigit(11, 17, i);
              }
            } break;
          default: {
            console.log("Błąd w pętli  z ID")
            break;
          }
        }
      }
    }

    function checkMiniMatrixForMissingDigit(a, b, i) {
      console.log("checkMiniMatrixForMissingDigit");
      for (j = 0; j < 6; j++) {
        if ((miniMatrixs[a].some(item => item == missingDigit)) || (miniMatrixs[b].some(item => item == missingDigit))) {
          conIsAvaiable = false;
          break;
        } else {
          console.log("Nie zawiera szukanej cyfry - zwiększ counterCondition");
          yCor = i;
          counterCondition++;
          break;
        }
      }
    }

    function findMissingDigitInMiniMatrix(a, b, missingDigit) {
      console.log("checkMiniMatrixForMissingDigit");
      conIsAvaiable = true;
      for (j = 0; j < 6; j++) {
        if ((miniMatrixs[a].some(item => item == missingDigit)) || (miniMatrixs[b].some(item => item == missingDigit))) {
          console.log("WESZLIŚMY ???" + missingDigit)
          conIsAvaiable = false;
          break;
        }
      }
    }


    /*******************************************************************************************************************************/


    // FUNCTIONS FOR THE THREE & FOUR STEP & FIFTH STEP
    function findMiniMatrixWithZeros(numberOfZeros) {
      console.log("findMiniMatrixWithZeros");
      countOfZero = 0;
      arrayOfZero = [];
      counterArrayOfZero = 0;
      for (i = 0; i < miniMatrixs.length; i++) {
        for (j = 0; j < 6; j++) {
          if (miniMatrixs[i][j] == 0) {
            countOfZero++;
          }
        }
        if (countOfZero == numberOfZeros) {
          arrayOfZero[counterArrayOfZero] = i
          counterArrayOfZero++;
          countOfZero = 0;
        } else {
          countOfZero = 0;
        }
      }
      console.log("arrayOfInputs z ID macierzy, w których są " + numberOfZeros + " msc. zerowe: " + arrayOfZero)
      console.log("Length arrayOfZero: " + arrayOfZero.length)
      countOfZero = 0;
      xCorrdinate = arrayOfZero[0];
    }

    function findMissingDigits(numberOfMissingDigit) {
      console.log("findMissingDigits")
      arrayOfMissingDigits = [];
      console.log("arrayOfMissingDigits LENGTH po wyzerowaniu" + arrayOfMissingDigits.length)
      if (numberOfMissingDigit == 2) {
        arrayOfMissingDigits = [99, 99];
      } else if (numberOfMissingDigit == 3) {
        arrayOfMissingDigits = [99, 99, 99];
      } else if (numberOfMissingDigit == 4) {
        arrayOfMissingDigits = [99, 99, 99, 99];
      } else {
        alert("Błąd w findMissingDigit")
      }
      console.log("macierz szukana: " + miniMatrixs[xCorrdinate])
      console.log("arrayOfMissingDigits LENGTH po wyzerowaniu" + arrayOfMissingDigits.length)

      for (i = 1; i < 7; i++) {
        if (miniMatrixs[xCorrdinate].some(item => item == i) == false) {
          if (arrayOfMissingDigits[0] == 99) {
            arrayOfMissingDigits[0] = i;
          } else if (arrayOfMissingDigits[1] == 99) {
            arrayOfMissingDigits[1] = i;
          } else if (arrayOfMissingDigits[2] == 99) {
            arrayOfMissingDigits[2] = i;
          } else {
            arrayOfMissingDigits[3] = i;
          }
        }
      }
      if (arrayOfMissingDigits[0] != 99) {
        console.log("Brakuje cyfry: " + arrayOfMissingDigits[0]);
      }
      if (arrayOfMissingDigits[1] != 99) {
        console.log("Brakuje cyfry: " + arrayOfMissingDigits[1]);
      }
      if (arrayOfMissingDigits[2] != 99) {
        console.log("Brakuje cyfry: " + arrayOfMissingDigits[2]);
      }
      if (arrayOfMissingDigits[3] != 99) {
        console.log("Brakuje cyfry: " + arrayOfMissingDigits[3]);
      }

      console.log("arrayOfMissingDigits LENGTH" + arrayOfMissingDigits.length)

    }

    function findCorsToFillDigitExtend(a) {
      console.log("thirdStepFindCorsToFillDigitExtend");
      yCorrdinate = miniMatrixs[xCorrdinate].indexOf('0');
      console.log("współrzędne 1 punktu: " + xCorrdinate + " i " + yCorrdinate);

      if (a == 2) {
        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          arrayForHelp[i] = miniMatrixs[xCorrdinate][i];
        }
        arrayForHelp[yCorrdinate] = 9;
        ySecondCor = arrayForHelp.indexOf('0');
        console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);
      } else if (a == 3) {

        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          arrayForHelp[i] = miniMatrixs[xCorrdinate][i];
        }
        arrayForHelp[yCorrdinate] = 9;
        ySecondCor = arrayForHelp.indexOf('0');
        console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);

        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          arrayForHelp[i] = miniMatrixs[xCorrdinate][i];
        }
        arrayForHelp[yCorrdinate] = 9;
        arrayForHelp[ySecondCor] = 9;
        yThirdCor = arrayForHelp.indexOf('0');
        console.log("współrzędne 3 punktu: " + xCorrdinate + " i " + yThirdCor);
      } else if (a == 4) {

        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          arrayForHelp[i] = miniMatrixs[xCorrdinate][i];
        }
        arrayForHelp[yCorrdinate] = 9;
        ySecondCor = arrayForHelp.indexOf('0');
        console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);

        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          arrayForHelp[i] = miniMatrixs[xCorrdinate][i];
        }
        arrayForHelp[yCorrdinate] = 9;
        arrayForHelp[ySecondCor] = 9;
        yThirdCor = arrayForHelp.indexOf('0');
        console.log("współrzędne 3 punktu: " + xCorrdinate + " i " + yThirdCor);

        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          arrayForHelp[i] = miniMatrixs[xCorrdinate][i];
        }
        arrayForHelp[yCorrdinate] = 9;
        arrayForHelp[ySecondCor] = 9;
        arrayForHelp[yThirdCor] = 9;
        yFourthCor = arrayForHelp.indexOf('0');
        console.log("współrzędne 4 punktu: " + xCorrdinate + " i " + yFourthCor);
      }
    }


    function findRightPlaceToFill(missingDigit, numberOfZeros) {
      localCounterCondition = 0;
      console.log("TUTAJ")
      if (numberOfZeros == 2) {
        arrayOfConditions[0] = checkMissingDigitInNearbyMatrix(missingDigit, yCorrdinate);
        arrayOfConditions[1] = checkMissingDigitInNearbyMatrix(missingDigit, ySecondCor);
      } else if (numberOfZeros == 3) {
        arrayOfConditions[0] = checkMissingDigitInNearbyMatrix(missingDigit, yCorrdinate);
        arrayOfConditions[1] = checkMissingDigitInNearbyMatrix(missingDigit, ySecondCor);
        arrayOfConditions[2] = checkMissingDigitInNearbyMatrix(missingDigit, yThirdCor);
      } else if (numberOfZeros == 4) {
        arrayOfConditions[0] = checkMissingDigitInNearbyMatrix(missingDigit, yCorrdinate);
        arrayOfConditions[1] = checkMissingDigitInNearbyMatrix(missingDigit, ySecondCor);
        arrayOfConditions[2] = checkMissingDigitInNearbyMatrix(missingDigit, yThirdCor);
        arrayOfConditions[3] = checkMissingDigitInNearbyMatrix(missingDigit, yFourthCor);
      } else {
        console.log("Something wrong in findRightPlaceToFill")
      }

      for (i = 0; i < numberOfZeros; i++) {
        if (arrayOfConditions[i]) {
          counterCondition++;
          localCounterCondition++;
        }
      }
    }





    function checkMissingDigitInNearbyMatrix(missingDigit, yCorrdinate) {
      console.log("checkMissingDigitInNearbyMatrix")
      conIsAvaiable = true;
      switch (xCorrdinate) {
        case 0:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(6, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(6, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(7, 12, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(7, 13, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(7, 14, missingDigit);
          }
          break;
        case 1:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 15, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(6, 16, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(6, 17, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(7, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(7, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(7, 17, missingDigit);
          }
          break;
        case 2:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(8, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(8, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 12, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(9, 13, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(9, 14, missingDigit);
          }
          break;
        case 3:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(8, 15, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(8, 16, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 17, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(9, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(9, 17, missingDigit);
          }
          break;
        case 4:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(10, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(10, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(10, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(11, 12, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(11, 13, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 14, missingDigit);
          }
          break;
        case 5:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(10, 15, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(10, 16, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(10, 17, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(11, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(11, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 17, missingDigit);
          }
          break;
        case 6:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(6, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(6, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(6, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(6, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(6, 17, missingDigit);
          }
          break;
        case 7:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(7, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(7, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(7, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(7, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(7, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(7, 17, missingDigit);
          }
          break;
        case 8:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(8, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(8, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(8, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(8, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(8, 17, missingDigit);
          }
          break;
        case 9:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(9, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(9, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(9, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(9, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(9, 17, missingDigit);
          }
          break;
        case 10:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(10, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(10, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(10, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(10, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(10, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(10, 17, missingDigit);
          }
          break;
        case 11:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(11, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(11, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(11, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(11, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(11, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 17, missingDigit);
          }
          break;
        case 12:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 12, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(7, 12, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 12, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 12, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(10, 12, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 12, missingDigit);
          }
          break;
        case 13:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 13, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(7, 13, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 13, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 13, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(10, 13, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 13, missingDigit);
          }
          break;
        case 14:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 14, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(7, 14, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 14, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 14, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(10, 14, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 14, missingDigit);
          }
          break;
        case 15:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 15, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(7, 15, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 15, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 15, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(10, 15, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 15, missingDigit);
          }
          break;
        case 16:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 16, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(7, 16, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 16, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 16, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(10, 16, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 16, missingDigit);
          }
          break;
        case 17:
          if (yCorrdinate == 0) {
            findMissingDigitInMiniMatrix(6, 17, missingDigit);
          } else if (yCorrdinate == 1) {
            findMissingDigitInMiniMatrix(7, 17, missingDigit);
          } else if (yCorrdinate == 2) {
            findMissingDigitInMiniMatrix(8, 17, missingDigit);
          } else if (yCorrdinate == 3) {
            findMissingDigitInMiniMatrix(9, 17, missingDigit);
          } else if (yCorrdinate == 4) {
            findMissingDigitInMiniMatrix(10, 17, missingDigit);
          } else if (yCorrdinate == 5) {
            findMissingDigitInMiniMatrix(11, 17, missingDigit);
          }
          break;
      }
      return conIsAvaiable;
    }

    function findTrueCondition(arrayOfConditions) {
      if (arrayOfConditions[0]) {
        yCor = yCorrdinate;
      } else if (arrayOfConditions[1]) {
        yCor = ySecondCor
      } else if (arrayOfConditions[2]) {
        yCor = yThirdCor
      } else {
        yCor = yFourthCor;
      }
    }


    // FUNCTIONS FOR THE THIRD STEP


    function fillDigitInRightPlace(numberOfMissingDigits, numberOfZeroPlaces) {
      console.log("TESTUJEMY fillDigitInRightPlace")
      let helpCounter = 0;
      let digit = arrayOfMissingDigits[helpCounter];
      counterCondition = 0;
      while (numberOfMissingDigits > 0) {
        findRightPlaceToFill(digit, numberOfZeroPlaces);
        if (localCounterCondition == 1) {
          counterCondition = 1;
          findTrueCondition(arrayOfConditions)
          break;
        } else {
          if (helpCounter < arrayOfMissingDigits.length) {
            helpCounter++;
            digit = arrayOfMissingDigits[helpCounter];
          }
          numberOfMissingDigits--;
        }
      }
      xCor = xCorrdinate;
      console.log("Musimy wpisać cyfrę: " + arrayOfMissingDigits[helpCounter] + " , we współrzędne: " + xCor + " i " + yCor)
      missingDigit = arrayOfMissingDigits[helpCounter];
    }


    // FINALLY STEPS

    function firstStep() {
      fillBoard();
      console.log("********** KROK NR 1 **********")
      if (findEmptyFieldsInMatrix() != -1) {
        firstStepFindEmptyField();
        findCorsToFillDigit();
        findMissingDigit();
        fillMissingDigit();
      } else {
        alert(" Wykonaj krok nr 2")
        console.log(" Nie wykonano kroku nr 1")
        arrayOfStepsEnding[0] = false;
      }
      arrayOfStepsEnding[1] = true;
    }

    function secondStep() {
      fillBoard();
      console.log("****** KROK NR 2 *******")
      if (secondStepFindOneMissingDigit() != -1) {
        secondStepFindOneMissingID();
        secondStepFindCoor();
        fillMissingDigit();
        arrayOfStepsEnding[0] = true;
        arrayOfStepsEnding[1] = false;
      } else {
        alert(" Wykonaj krok nr 3")
        arrayOfStepsEnding[0] = false;
        arrayOfStepsEnding[1] = false;
        arrayOfStepsEnding[2] = true;
        console.log(" Nie wykonano kroku nr 2")
      }
    }

    function anotherStep(numberStepDecrease) {
      console.log("******** ANOTHER STEP ******" + (numberStepDecrease + 1))
      fillBoard();
      findMiniMatrixWithZeros(numberStepDecrease);
      counterExecutedTimes = 0;
      if (arrayOfZero.length == 0) {
        alert("Execute next Step. No matrix with " + numberStepDecrease + " zero places.")
        arrayOfStepsEnding[0] = true;
        arrayOfStepsEnding[1] = true;
        arrayOfStepsEnding[numberStepDecrease] = false;
      } else {
        while (conOfLoop) {
          if (counterExecutedTimes <= arrayOfZero.length) {
            findMissingDigits(numberStepDecrease);
            findCorsToFillDigitExtend(numberStepDecrease);
            fillDigitInRightPlace(numberStepDecrease, numberStepDecrease);
            while (counterCondition == 4 && counterExecutedTimes < arrayOfZero.length - 1) {
              console.log("*******JESTEŚMY TUTAJ counterCondition == 4 **********")
              counterExecutedTimes++;
              xCorrdinate = arrayOfZero[counterExecutedTimes];
              findMissingDigits(numberStepDecrease);
              findCorsToFillDigitExtend(numberStepDecrease);
              fillDigitInRightPlace(numberStepDecrease, numberStepDecrease);
            }
            if (counterCondition == 1) {
              arrayOfStepsEnding[0] = true;
              arrayOfStepsEnding[1] = true;
              if (xCor > 5) {
                findCorsToFillDigit();
              }
              fillMissingDigit();
              conOfLoop = false;
            }
            else {
              conOfLoop = false;
              if (numberStepDecrease == 4) {
                alert(" You have to start again from step: 1")
                arrayOfStepsEnding[0] = true;
                arrayOfStepsEnding[1] = true;
                arrayOfStepsEnding[2] = true;
                arrayOfStepsEnding[3] = true;
                arrayOfStepsEnding[numberStepDecrease] = false;
              } else {
                alert(" Execute next step: " + (numberStepDecrease + 2))
                arrayOfStepsEnding[0] = true;
                arrayOfStepsEnding[1] = true;
                arrayOfStepsEnding[2] = true;
                arrayOfStepsEnding[3] = true;
                arrayOfStepsEnding[4] = true;
                arrayOfStepsEnding[numberStepDecrease] = false;
              }
            }
          } else {
            conOfLoop = false;
            if (numberStepDecrease == 4) {
              alert(" You have to start again from step: 1")
              arrayOfStepsEnding[numberStepDecrease] = false;
            } else {
              alert(" Execute next step: " + (numberStepDecrease + 2))
              arrayOfStepsEnding[numberStepDecrease] = false;
            }
          }
        }
        conOfLoop = true;
      }
      /*       counterArrayOfZero = 0; */
    }

    return {
      drawGame, finishGame, fillBoard, findEmptyFieldsInMatrix, findCorsToFillDigit, findMissingDigit, fillMissingDigit, secondStepFindOneMissingDigit,
      secondStepFindOneMissingID, secondStepFindCoor, fillDigitInRightPlace, firstStep, secondStep, anotherStep, isZero,
      gamesLevelEasy, gamesLevelMedium, gamesLevelHard, findMiniMatrixWithZeros, checkMiniMatrixForMissingDigit, findMissingDigits, findCorsToFillDigitExtend, newGame, firstStepFindEmptyField,
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Anton&family=Barlow+Semi+Condensed:wght@600;700&family=Lobster&family=Open+Sans:ital,wght@1,300&family=Oswald&family=Pangolin&display=swap');

* {
  margin: 0;
  padding: 0;
}

.containerApp {
  align-items: center;
  background-image: linear-gradient(to top, #032709, #003019, #003a28, #004337, #004d46, #005a56, #006768, #00757a, #008993, #049eae, #0fb4cb, #1ec9e8);
  /*   background-image: linear-gradient(to left top, #ca00d1, #b13bd8, #9851db, #815fd9, #6d69d3, #6166cb, #5563c2, #4a5fb9, #3e50af, #3242a5, #27339b, #1b2490); */
  display: flex;
  flex-direction: column;
  height: 100vh;
  justify-content: center;
  overflow-y: scroll;
  text-align: center;
  width: 100vw;


  &__title {
    height: 6vh;

    h1 {
      color: #004d46;
      font-size: 2rem;
      font-family: 'Anton', sans-serif;
      letter-spacing: 2px;
      padding: 0 10px;
      text-align: center;
      text-shadow: -6px 6px 6px #f4fffb;
    }
  }

  &__box {
    align-items: center;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    height: 90%;
    justify-content: space-around;
    width: 95vw;

    /*   border: solid 1px red;
 */
    &--container {

      align-items: flex-start;
      /* border: dotted 2px green; */
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      height: 100%;
      justify-content: center;
      width: 65%;

      .containerApp__buttons {
        align-items: center;
        background-image: linear-gradient(to left top, #032709, #003019, #003a28, #004337, #004d46, #005451, #005c5c, #026368, #066a72, #0c707c, #137786, #1b7e90);
        /*       background-image: linear-gradient(to left top, #ca00d1, #b13bd8, #9851db, #815fd9, #6d69d3, #6166cb, #5563c2, #4a5fb9, #3e50af, #3242a5, #27339b, #1b2490); */
        border-radius: 12px;
        /*       box-shadow: -6px -6px 10px #aad5c5; */
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        height: 16%;
        justify-content: center;
        margin: 4vh 0;
        width: auto;
        /*   border: dotted 2px red; */
      }


      .containerApp__game--steps {
        align-self: flex-start;
        align-items: center;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        height: auto;
        width: 52%;
        /*       border: solid 4px white; */
      }

      .containerApp__game {
        align-items: center;
        border-radius: 8px;
        color: aqua;
        display: flex;
        flex-direction: column;
        height: 50%;
        justify-content: center;
        width: 45%;
        /*      border: dotted 3px yellow; */

        .containerApp__game--board {
          align-content: space-around;
          box-shadow: 6px 6px 10px #aad5c5;
          background-image: linear-gradient(to left top, #032709, #003019, #003a28, #004337, #004d46, #005451, #005c5c, #026368, #066a72, #0c707c, #137786, #1b7e90);
          /*         background-image: linear-gradient(to left top, #ca00d1, #b13bd8, #9851db, #815fd9, #6d69d3, #6166cb, #5563c2, #4a5fb9, #3e50af, #3242a5, #27339b, #1b2490); */
          border-radius: 12px;
          display: flex;
          flex-wrap: wrap;
          justify-content: center;
          padding: 8px;
          height: 60%;
          width: 80%;
          /*       border: dotted 2px red; */
        }



      }
    }



    .containerApp__screen {
      align-self: flex-start;
      background: linear-gradient(to left bottom, #032709, #003019, #003a28, #004337, #004d46, #005451, #005c5c, #026368, #066a72);
      /*       background-image: linear-gradient(to left, #5563c2, #4a5fb9, #3e50af, #3242a5, #27339b, #1b2490); */
      border-radius: 12px;
      box-shadow: 6px 6px 10px whitesmoke;
      color: whitesmoke;
      height: auto;
      margin: 4vh auto;
      width: 30%;
      /*     border: dotted 2px green; */

    }

  }


}
</style>
