<template>
    <div class="containerApp__game__helpButtons">
        <div class="buttons--main--box">
            <button class="single--button" @click="fillBoard"> Wypełnij wewnętrzne macierze</button>
            <button class="single--button" @click="fillMissingDigit"> Wpisz brakującą cyfrę</button>
            <button class="single--button" @click="finishGame"> ROZWIĄŻ SUDOKU</button>
        </div>
        <div class="buttons--help--box">
            <button class="single--button" @click="findEmptyFieldsInMatrix"> 1. Podaj macierze z ilością pustych
                miejsc</button>
            <button class="single--button" @click="findCorsToFillDigit"> 1. Znajdź współrzędne na wpisanie brakujące
                cyfry</button>
            <button class="single--button" @click="findMissingDigit"> 1. Sprawdź jakiej cyfry brakuje</button>
        </div>
        <div class="buttons--help--box">
            <button class="single--button" @click="secondStepFindOneMissingDigit"> 2. Czy jest jakaś cyfra n-1?</button>
            <button class="single--button" @click="secondStepFindOneMissingID"> 2. Jakie ID brakuje?</button>
            <button class="single--button" @click="secondStepFindCoor"> 2. Jakie współrzędne ?</button>
        </div>
        <div class="buttons--help--box">
            <button class="single--button" @click="thirdStepFindOneMissingID"> 3. Znajdź macierz, która ma dwa zera</button>
            <button class="single--button" @click="thirdStepFindMissingDigits"> 3. Znajdź brakujące dwie cyfry</button>
            <button class="single--button" @click="thirdStepFindCorsToFillDigit"> 3. Znajdź współrzędne do wpisania</button>
            <button class="single--button" @click="thirdStepFillDigitsInRightPlace"> 3. Wpisz cyfrę w jedno z dwóch
                miejsc</button>
        </div>
        <div class="buttons--help--box">
            <button class="single--button" @click="fourthStepFindOneMissingID"> 4a. Znajdź macierz gdzie są 3 zera</button>
            <button class="single--button" @click="fourthStepFindMissingDigits"> 4b. Sprawdź jakich liczb brakuje</button>
            <button class="single--button" @click="fourthStepFindCorsToFillDigit"> 4c. Znajdź miejsca zerowe</button>
            <button class="single--button" @click="fourthStepFillDigitsInRightPlace"> 4c. Znajdź odpowiednie
                miejsce</button>
        </div>
                <div class="buttons--help--box">
                <button class="single--button" @click="fifthStepFindMiniMatrixWithTwoZeros"> 5. Znajdź miniMacierze z dwoma pustymi miejscami</button>
            </div>
        <div class="buttons--help--box">
            <button class="single--button" @click="firstStep"> PIERWSZY KROK</button>
            <button class="single--button" @click="secondStep"> DRUGI KROK</button>
            <button class="single--button" @click="thirdStep"> TRZECI KROK</button>
            <button class="single--button" @click="fourthStep"> CZWARTY KROK</button>
        </div>
    </div>
</template>


<script>


export default {
    name: 'ButtonsSingleGame',
    props: ['findEmptyFieldsInMatrix', 'findCorsToFillDigit', 'findMissingDigit', 'fillMissingDigit', 'fillBoard', 'secondStepFindOneMissingDigit', 'secondStepFindOneMissingID', 'secondStepFindCoor', 'thirdStepFindOneMissingID', 'thirdStepFindMissingDigits', 'thirdStepFindCorsToFillDigit', 'thirdStepFillDigitsInRightPlace', 'finishGame', 'firstStep', 'secondStep', 'thirdStep',
        'fourthStepFindOneMissingID', 'fourthStepFindMissingDigits', 'fourthStepFindCorsToFillDigit', 'fourthStepFillDigitsInRightPlace', 'fourthStep', 'fifthStepFindMiniMatrixWithTwoZeros'],

}



</script>


<style lang="scss" scoped>
.containerApp__game__helpButtons {
    /*     border: solid 3px whitesmoke; */
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    height: auto;
    margin: 10px;
    width: 90vw;

    .buttons--main--box,
    .buttons--help--box {



        box-shadow: -4px 6px 10px #aad5c5;
   background-color: #1b7e90;
        /*   background-color: #000d56; */
             background-image: linear-gradient(to left top, #032709, #003019, #003a28, #004337, #004d46, #005451, #005c5c, #026368, #066a72, #0c707c, #137786, #1b7e90);
/*              background-image: linear-gradient(to bottom right, #032709, #003019, #003a28, #004337, #004d46, #005a56, #006768, #00757a, #008993, #049eae, #0fb4cb, #1ec9e8); */
        border-radius: 12px;
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        justify-content: space-around;
        height: auto;
        margin-top: 30px;
        width: 150px;
    }

    .single--button {
        background-color: transparent;
        border: solid 1px whitesmoke;
        border-radius: 8px;
        color: white;
        cursor: pointer;
        font-size: 14px;
        font-weight: 600;
        margin: 8px;
        padding: 6px;
        width: auto;
    }
}
</style>