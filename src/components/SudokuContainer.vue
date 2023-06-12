<template>
  <div class="containerApp">
    <div class="containerApp__title">
      <h1>Sudoku - poznaj sposób na rozwiązanie...
      </h1>
    </div>
    <div class="containerApp__box">
      <div class="containerApp__box--container">
        <div class="containerApp__buttons">
          <ButtonsGame :drawGame="drawGame" :newGame="newGame" :finishGame="finishGame" :fillBoard="fillBoard" />
        </div>
        <div class="containerApp__game--steps">
          <ButtonsSteps :findEmptyFieldsInMatrix="findEmptyFieldsInMatrix" :findCorsToFillDigit="findCorsToFillDigit"
            :findMissingDigit="findMissingDigit" :fillMissingDigit="fillMissingDigit" :fillBoard="fillBoard"
            :secondStepFindOneMissingDigit="secondStepFindOneMissingDigit"
            :secondStepFindOneMissingID="secondStepFindOneMissingID" :secondStepFindCoor="secondStepFindCoor"
            :thirdStepFillDigitsInRightPlace="thirdStepFillDigitsInRightPlace" :finishGame="finishGame"
            :firstStep="firstStep" :secondStep="secondStep" :thirdStep="thirdStep"
            :fourthStepFillDigitsInRightPlace="fourthStepFillDigitsInRightPlace" :fourthStep="fourthStep"
            :findMiniMatrixWithZeros="findMiniMatrixWithZeros"
            :findCorsToFillDigitExtend="findCorsToFillDigitExtend"
            :fourthStepFillDigitsInRightPlace5="fourthStepFillDigitsInRightPlace5" :fifthStep="fifthStep" :firstStepFindEmptyField="firstStepFindEmptyField"/>
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



    let k = 0;
    let counterThirdStep = 0;
    // counters for third step
    let counterCondition = 0;
    let counterConditionFinish = 0;






 
    let testArr = [1, 2, 3, 4, 5, 6]
    let copyArr = [];
    // for the second help
    let missingID = 99;
    // for the third help
    let ySecondCor = 9;
    let secondMissingDigit = 99;
    let arrayOfZero = [];
    let lengthOfarrayOfZero = 0;
    // for the fourth / fifth help
    let thirdMissingDigit = 99;
    let fourthMissingDigit = 99;
    let yThirdCor = 9;
    let yFourthCor = 9;
    let warunekPom = true;
    let arrayOfCondition = [true, true, true]


 
    let conLoopThirdStep = true;

    // new lets ///////////////////////////////////////
    //counters
    let i = 0;
    let j = 0;
    let counterStepstoFinish = 0;
        let countOfZero = 0;

    // to draw Game
    const gamesLevelEasy = games6x6LevelEasy;
    const gamesLevelMedium = games6x6LevelMedium;
    const gamesLevelHard = games6x6LevelHard;
    let arrayOfInputs;
    let numberOfGame = 0;

    //conditions
    let conFirstStep = true;
    let conSecondStep = true;
    let conThirdStep = true;
    let conFinishGame = false;
    let endCondition = true;

    // other
    let missingDigit = 99;
    let xCor = 0;
    let yCor = 0;
    let yCorrdinate = 0;
    let xCorrdinate = 0;

    // arrays for the help
    let miniMatrixs = [];
    let rows = [];
    let columns = [];

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
    function drawGame(levelOfGame) {
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

    // function, which sholud finish game for one click     // CHECK ONCE AGAIN
    function finishGame() {
      console.log("finishGame");
      fillBoard();
      while (!conFinishGame) {
        while (conFirstStep) {
          console.log("***************** KROK 1 ************************")
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break;
          } else {
            firstStep();
            conThirdStep = true;
          }

        }
        while (conSecondStep) {
          console.log("***************** KROK 2 ************************")
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break;
          } else {
            secondStep();
            conThirdStep = true;
          }
        }
        if (conFirstStep == false && conSecondStep == false && conThirdStep == true) {
          console.log("***************** KROK 3 ************************")
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break;
          } else {
            conFirstStep = true;
            conSecondStep = true;
            thirdStep();
          }
        }
        if (conThirdStep == false) {
          console.log("***************** KROK 4 ************************")
          if (isZero() == -1) {
            conFinishGame = true;
            alert("Gratulacje! Rozwiązano Sudoku!")
            break;
          } else {
            conFirstStep = true;
            conSecondStep = true;
            conThirdStep = true;
            fourthStep();
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
      conFirstStep, conSecondStep, conThirdStep = false;
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
      findCorsToFillDigit();
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
        copyArr[i] = miniMatrixs[xCorrdinate][i];
      }
      copyArr.sort();
      for (i = 0; i < 6; i++) {
        if (copyArr[i + 1] != testArr[i]) {
          console.log("Brakuje cyfry: " + testArr[i]);
          miniMatrixs[xCorrdinate][yCorrdinate] = testArr[i];
          missingDigit = testArr[i];
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
          break;
        } else {
          console.log("Nie zawiera szukanej cyfry - zwiększ counterCondition");
          yCor = i;
          counterCondition++;
          break;
        }
      }
    }


    // FUNCTIONS FOR THE THIRD STEP




    function thirdStepFillDigitsInRightPlace() {
      console.log("thirdStepFillDigitsInRightPlace")

      console.log("Brakuje cyfr: " + missingDigit + " oraz " + secondMissingDigit);
      console.log("współrzędne 1 punktu: " + xCorrdinate + " i " + yCorrdinate);
      console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);


      missingID = xCorrdinate;

      counterCondition = 0;
      counterConditionFinish = 0;

      helpForCorrectCorrdinates(missingID)

      if (counterCondition == 2) {
        console.log("counterCondition = 2. Musimy wykonać dla drugiej cyfry ")
        counterConditionFinish++;
        missingDigit = secondMissingDigit;
        helpForCorrectCorrdinates(missingID)
      } else {
        console.log("Musimy wpisać : " + missingDigit)
        xCor = xCorrdinate;
        console.log("Prawidłowe współrzędne do wpisania: " + xCor + " oraz " + yCor)
      }

      if (counterCondition == 4) {
        console.log("counterCondition = 4. Musimy wykonać dla innej Macierzy ")
        counterConditionFinish++;
      } else {
        console.log("Musimy wpisać : " + missingDigit)
        xCor = xCorrdinate;
        console.log("Prawidłowe współrzędne do wpisania: " + xCor + " oraz " + yCor)
      }
      console.log("counterConditionFinish: " + counterConditionFinish)
      yCorrdinate = yCor;
    }


    // FUNCTIONS FOR THE FOURTH STEP


    function fourthStepFillDigitsInRightPlace() {
      console.log("***fourthStepFillDigitsInRightPlace-2-***")
      /* Wykonać fourthStepHelpFindCondition */
      /* Jeśli (counterCondition == 1) to sprawdzamy, który warunek jest true i wstawiamy do yCor prawidłową współrzędną Y miejsca zerowego */
      /* Jeśli nie to wywołujemy tą samą funkcję tylko dla secondMissingDigit */



      fourthStepHelpFindCondition(missingDigit, 3);

      if (counterCondition == 1) {
        if (arrayOfCondition[0]) {
          yCor = yCorrdinate;
        } else if (arrayOfCondition[1]) {
          yCor = ySecondCor
        } else {
          yCor = yThirdCor
        }
      } else {
        fourthStepHelpFindCondition(secondMissingDigit, 3);
        if (counterCondition == 1) {
          if (arrayOfCondition[0]) {
            missingDigit = secondMissingDigit;
            yCor = yCorrdinate;
          } else if (arrayOfCondition[1]) {
            missingDigit = secondMissingDigit;
            yCor = ySecondCor
          } else {
            missingDigit = secondMissingDigit;
            yCor = yThirdCor
          }
        } else {
          fourthStepHelpFindCondition(thirdMissingDigit, 3);
          console.log(" Jesteśmy tutaj?")
          console.log(" arrayOfInputs warunków?" + arrayOfCondition)
          console.log(" Countercondition?" + counterCondition)
          if (counterCondition == 1) {
            console.log(" Jesteśmy tutaj 2?")
            if (arrayOfCondition[0]) {
              missingDigit = thirdMissingDigit;
              yCor = yCorrdinate;
            } else if (arrayOfCondition[1]) {
              missingDigit = thirdMissingDigit;
              yCor = ySecondCor
            } else {
              missingDigit = thirdMissingDigit;
              yCor = yThirdCor
            }
          } else {
            console.log("Musimy wykonać dla innej macierzy")
            counterCondition = 4;
          }
        }
      }
      xCor = xCorrdinate;
      console.log("Musimy wpisać cyfrę: " + missingDigit + " , we współrzędne: " + xCor + " i " + yCor)
    }

    function fourthStepHelpFindCondition(missingDigit, numberOfZeros) {
      counterCondition = 0;
      if (numberOfZeros == 3) {
        arrayOfCondition[0] = fourthStepHelp(missingDigit, yCorrdinate);
        arrayOfCondition[1] = fourthStepHelp(missingDigit, ySecondCor);
        arrayOfCondition[2] = fourthStepHelp(missingDigit, yThirdCor);
      } else if (numberOfZeros == 4) {
        console.log("JESTEM TUTAJ?????")
        arrayOfCondition[0] = fourthStepHelp(missingDigit, yCorrdinate);
        arrayOfCondition[1] = fourthStepHelp(missingDigit, ySecondCor);
        arrayOfCondition[2] = fourthStepHelp(missingDigit, yThirdCor);
        arrayOfCondition[3] = fourthStepHelp(missingDigit, yFourthCor);
      } else {
        console.log("Something wrong in fourthStepHelpFindCondition")
      }

      for (i = 0; i < 3; i++) {
        if (arrayOfCondition[i]) {
          counterCondition++;
        }
      }
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
          }
          /* else if (yCorrdinate == 1) {
            if ((miniMatrixs[6].some(item => item == missingDigit)) || (miniMatrixs[13].some(item => item == missingDigit))) {
              warunekPom = false;
            }
          }
 */
          else if (yCorrdinate == 2) {
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



    //FUNCTIONS FOR THE FIFTH STEP


    function fourthStepFillDigitsInRightPlace5() {
      console.log("***fourthStepFillDigitsInRightPlace5555-***")
      /* Wykonać fourthStepHelpFindCondition */
      /* Jeśli (counterCondition == 1) to sprawdzamy, który warunek jest true i wstawiamy do yCor prawidłową współrzędną Y miejsca zerowego */
      /* Jeśli nie to wywołujemy tą samą funkcję tylko dla secondMissingDigit */

      fourthStepHelpFindCondition(missingDigit, 4);

      if (counterCondition == 1) {
        console.log("arrayOfInputs warunków: " + arrayOfCondition)
        if (arrayOfCondition[0]) {
          yCor = yCorrdinate;
        } else if (arrayOfCondition[1]) {
          yCor = ySecondCor
        } else if (arrayOfCondition[2]) {
          yCor = yThirdCor
        } else {
          yCor = yFourthCor;
        }
      } else {
        fourthStepHelpFindCondition(secondMissingDigit, 4);
        if (counterCondition == 1) {
          if (arrayOfCondition[0]) {
            missingDigit = secondMissingDigit;
            yCor = yCorrdinate;
          } else if (arrayOfCondition[1]) {
            missingDigit = secondMissingDigit;
            yCor = ySecondCor
          } else if (arrayOfCondition[2]) {
            missingDigit = secondMissingDigit;
            yCor = yThirdCor
          } else {
            missingDigit = secondMissingDigit;
            yCor = yFourthCor;
          }
        } else {
          fourthStepHelpFindCondition(thirdMissingDigit);
          if (counterCondition == 1) {
            console.log(" Jesteśmy tutaj 2?")
            if (arrayOfCondition[0]) {
              missingDigit = thirdMissingDigit;
              yCor = yCorrdinate;
            } else if (arrayOfCondition[1]) {
              missingDigit = thirdMissingDigit;
              yCor = ySecondCor
            } else if (arrayOfCondition[2]) {
              missingDigit = thirdMissingDigit;
              yCor = yThirdCor
            } else {
              missingDigit = thirdMissingDigit;
              yCor = yFourthCor;
            }
          } else {
            fourthStepHelpFindCondition(fourthMissingDigit);
            if (counterCondition == 1) {
              if (arrayOfCondition[0]) {
                missingDigit = fourthMissingDigit;
                yCor = yCorrdinate;
              } else if (arrayOfCondition[1]) {
                missingDigit = fourthMissingDigit;
                yCor = ySecondCor
              } else if (arrayOfCondition[2]) {
                missingDigit = fourthMissingDigit;
                yCor = yThirdCor
              } else {
                missingDigit = fourthMissingDigit;
                yCor = yFourthCor;
              }
            } else {
              console.log("Musimy wykonać dla innej macierzy")
              counterCondition = 4;
            }
          }
        }

      }
      xCor = xCorrdinate;
      console.log("Musimy wpisać cyfrę: " + missingDigit + " , we współrzędne: " + xCor + " i " + yCor)
    }


    // FUNCTIONS FOR THE THREE & FOUR STEP

    function findMiniMatrixWithZeros(numberOfZeros) {
      console.log("findMiniMatrixWithZeros");
      countOfZero = 0;
      arrayOfZero = [];
      k = 0;
      for (i = 0; i < miniMatrixs.length; i++) {
        for (j = 0; j < 6; j++) {
          if (miniMatrixs[i][j] == 0) {
            countOfZero++;
          }
        }
        if (countOfZero == numberOfZeros) {
          arrayOfZero[k] = i
          k++;
          countOfZero = 0;
        } else {
          countOfZero = 0;
        }
      }

      lengthOfarrayOfZero = arrayOfZero.length;
      console.log("arrayOfInputs z ID macierzy, w których są " + numberOfZeros + " msc. zerowe: " + arrayOfZero)
      console.log("Length arrayOfZero: " + lengthOfarrayOfZero)
      countOfZero = 0;
      xCorrdinate = arrayOfZero[0];
    }

    function findMissingDigits(numberOfMissingDigit) {
      console.log("findMissingDigits")
      if (numberOfMissingDigit == 2) {
        missingDigit = 99;
        secondMissingDigit = 99;
      } else if (numberOfMissingDigit == 3) {
        missingDigit = 99;
        secondMissingDigit = 99;
        thirdMissingDigit = 99;
      } else if (numberOfMissingDigit == 4) {
        missingDigit = 99;
        secondMissingDigit = 99;
        thirdMissingDigit = 99;
        fourthMissingDigit = 99;
      } else {
        alert("Błąd w findMissingDigit")
      }
      console.log("macierz szukana: " + miniMatrixs[xCorrdinate])

      for (i = 1; i < 7; i++) {
        if (miniMatrixs[xCorrdinate].some(item => item == i) == false) {
          if (missingDigit == 99) {
            missingDigit = i;
          } else if (secondMissingDigit == 99) {
            secondMissingDigit = i;
          } else if (thirdMissingDigit == 99) {
            thirdMissingDigit = i;
          } else {
            fourthMissingDigit = i;
          }
        }
      }
      if (missingDigit != 99) {
        console.log("Brakuje cyfry: " + missingDigit);
      }
      if (secondMissingDigit != 99) {
        console.log("Brakuje cyfry: " + secondMissingDigit);
      }
      if (thirdMissingDigit != 99) {
        console.log("Brakuje cyfry: " + thirdMissingDigit);
      }
      if (fourthMissingDigit != 99) {
        console.log("Brakuje cyfry: " + fourthMissingDigit);
      }

    }

    function findCorsToFillDigitExtend(a) {

      // sprawdzenie jakiej cyfry brakuje w macierzy
      console.log("thirdStepFindCorsToFillDigitExtend");

      yCorrdinate = miniMatrixs[xCorrdinate].indexOf('0');
      console.log("współrzędne 1 punktu: " + xCorrdinate + " i " + yCorrdinate);

      if (a == 2) {
        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          copyArr[i] = miniMatrixs[xCorrdinate][i];
        }
        copyArr[yCorrdinate] = 9;
        ySecondCor = copyArr.indexOf('0');
        console.log("współrzędne 2 punktu: " + xCorrdinate + " i " + ySecondCor);
      } else if (a == 3) {

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
      } else if (a == 4) {

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

        for (i = 0; i < miniMatrixs[xCorrdinate].length; i++) {
          copyArr[i] = miniMatrixs[xCorrdinate][i];
        }
        copyArr[yCorrdinate] = 9;
        copyArr[ySecondCor] = 9;
        copyArr[yThirdCor] = 9;
        yFourthCor = copyArr.indexOf('0');
        console.log("współrzędne 4 punktu: " + xCorrdinate + " i " + yFourthCor);
      }

    }



    // FINALLY STEPS

    function firstStep() {
      fillBoard();
      if (findEmptyFieldsInMatrix() != -1) {
        console.log("********** KROK NR 1 **********")
        firstStepFindEmptyField();
        findCorsToFillDigit();
        findMissingDigit();
        fillMissingDigit();
      } else {
        alert(" Wykonaj krok nr 2")
        console.log(" Nie wykonano kroku nr 1")
        conFirstStep = false;
      }
      conSecondStep = true;
    }

    function secondStep() {
      console.log("****** KROK NR 2 *******")
      fillBoard();

      if (secondStepFindOneMissingDigit() != -1) {
        secondStepFindOneMissingID();
        secondStepFindCoor();
        fillMissingDigit();
        conFirstStep = true;
        conSecondStep = false;
      } else {
        alert(" Wykonaj krok nr 3")
        conSecondStep = false;
        conFirstStep = false;
        console.log(" Nie wykonano kroku nr 2")
      }
    }


    function thirdStep() {
      console.log("******** KROK NR 3 ******")
      fillBoard();
      findMiniMatrixWithZeros(2);

      counterThirdStep = 0;
      conThirdStep = true;

      if (lengthOfarrayOfZero == 0) {
        console.log("WYKONAJ KROK NR 4")
      } else {
        while (conLoopThirdStep) {
          if (counterThirdStep <= lengthOfarrayOfZero) {
            findMissingDigits(2);
            findCorsToFillDigitExtend(2);
            thirdStepFillDigitsInRightPlace();
            while (counterCondition == 4 && counterThirdStep < lengthOfarrayOfZero - 1) {
              console.log("*******JESTEŚMY TUTAJ counterCondition == 4 **********")
              counterThirdStep++;
              xCorrdinate = arrayOfZero[counterThirdStep];
              console.log(" COUNTER THIRD STEP == " + counterThirdStep)
              console.log(" LENGTH OF ARRAY == " + lengthOfarrayOfZero)
              findMissingDigits(2);
              findCorsToFillDigitExtend(2);
              thirdStepFillDigitsInRightPlace();
            }
            if (counterCondition == 1 || counterConditionFinish == 1) {
              console.log("*******counterCondition jest różny od 4 *********")
              console.log(" COUNTERCONDITION == " + counterCondition)
              conFirstStep = true;
              conSecondStep = true;
              if (xCor > 5) {
                findCorsToFillDigit();
              }
              fillMissingDigit();
              conLoopThirdStep = false;
            }
            else {
              conLoopThirdStep = false;
              alert(" WYKONAJ KROK 4")
              conThirdStep = false;
            }
          } else {
            conLoopThirdStep = false;
            alert(" WYKONAJ KROK 4")
            conThirdStep = false;
          }
        }
        k = 0;
        conLoopThirdStep = true;
      }
    }


    function fourthStep() {
      console.log("******** KROK NR 4 ******")
      findMiniMatrixWithZeros(3);
      findMissingDigits(3);
      findCorsToFillDigitExtend(3);
      fourthStepFillDigitsInRightPlace();

      counterThirdStep = 0;
      conThirdStep = true;

      if (lengthOfarrayOfZero == 0) {
        console.log("Nie ma macierzy z 3 wolnymi polami")
      } else {
        while (conLoopThirdStep) {
          if (counterThirdStep <= lengthOfarrayOfZero) {
            findMissingDigits(3);
            findCorsToFillDigitExtend(3);
            fourthStepFillDigitsInRightPlace();
            while (counterCondition == 4 && counterThirdStep < lengthOfarrayOfZero - 1) {
              console.log("*******JESTEŚMY TUTAJ counterCondition == 4 **********")
              counterThirdStep++;
              xCorrdinate = arrayOfZero[counterThirdStep];
              console.log(" COUNTER THIRD STEP == " + counterThirdStep)
              console.log(" LENGTH OF ARRAY == " + lengthOfarrayOfZero)
              findMissingDigits(3);
              findCorsToFillDigitExtend(3);
              fourthStepFillDigitsInRightPlace();
            }
            if (counterCondition == 1 || counterConditionFinish == 1) {
              console.log("*******counterCondition jest różny od 4 *********")
              console.log(" COUNTERCONDITION == " + counterCondition)
              conFirstStep = true;
              conSecondStep = true;
              if (xCor > 5) {
                findCorsToFillDigit();
              }
              fillMissingDigit();
              conLoopThirdStep = false;
            }
            else {
              conLoopThirdStep = false;
              alert(" WYKONAJ KROK 5")
              conThirdStep = false;
            }
          } else {
            conLoopThirdStep = false;
            alert(" WYKONAJ KROK 5")
            conThirdStep = false;
          }
        }
        k = 0;
        conLoopThirdStep = true;
      }



    }

    function fifthStep() {
      findMiniMatrixWithZeros(4);
      findMissingDigits(4);
      findCorsToFillDigitExtend(4);
      fourthStepFillDigitsInRightPlace5();
      fillMissingDigit();
    }


    return {
      drawGame, finishGame, fillBoard, findEmptyFieldsInMatrix, findCorsToFillDigit, findMissingDigit, fillMissingDigit, secondStepFindOneMissingDigit,
      secondStepFindOneMissingID, secondStepFindCoor, thirdStepFillDigitsInRightPlace, firstStep, secondStep, thirdStep, isZero,
      gamesLevelEasy, gamesLevelMedium, gamesLevelHard, fourthStepFillDigitsInRightPlace, fourthStep, findMiniMatrixWithZeros, checkMiniMatrixForMissingDigit, findMissingDigits, findCorsToFillDigitExtend, fourthStepFillDigitsInRightPlace5, fifthStep, newGame, firstStepFindEmptyField,
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
        margin-top: 4vh;
        width: 60%;
        /*         border: dotted 2px red;  */
      }


      .containerApp__game--steps {
        align-self: flex-start;
        align-items: center;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        height: 50%;
        width: 45%;
        /*        border: solid 4px white; */
      }

      .containerApp__game {
        align-items: center;
        border-radius: 8px;
        color: aqua;
        display: flex;
        flex-direction: column;
        height: 50%;
        justify-content: center;
        width: 52%;
        /*       border: dotted 3px yellow; */

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
