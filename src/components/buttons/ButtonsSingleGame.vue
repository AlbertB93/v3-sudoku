<template>
    <div class="containerApp__game__helpButtons">
        <div class="buttons--main--box">
            <button class="single--button" @click="fillBoard"> Wypełnij wewnętrzne macierze</button>
            <button class="single--button" @click="fillMissingDigit"> Wpisz brakującą cyfrę</button>
            <button class="single--button" @click="finishGame"> ROZWIĄŻ SUDOKU</button>
        </div>
        <div class="buttons--help--box">
            <button class="single--button" @click="findEmptyFieldsInMatrix"> Podaj macierze z ilością pustych
                miejsc</button>
            <button class="single--button" @click="findCorsToFillDigit"> Znajdź współrzędne na wpisanie brakujące
                cyfry</button>
            <button class="single--button" @click="findMissingDigit"> Sprawdź jakiej cyfry brakuje</button>
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
        'fourthStepFindOneMissingID', 'fourthStepFindMissingDigits', 'fourthStepFindCorsToFillDigit', 'fourthStepFillDigitsInRightPlace', 'fourthStep'],

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

        background-color: rgb(55, 54, 54);
        border-radius: 12px;
        flex-direction: column;
        flex-wrap: nowrap;
        height: auto;
        margin-top: 30px;
        width: 150px;
    }

    .single--button {
        background-color: rgb(18, 22, 22);
        border: solid 2px whitesmoke;
        border-radius: 8px;
        color: whitesmoke;
        cursor: pointer;
        font-size: 10px;
        margin: 8px;
        padding: 6px;
        width: auto;
    }
}
</style>