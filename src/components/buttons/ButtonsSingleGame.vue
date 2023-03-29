<template>
    <div class="containerApp__game__helpButtons">
        <div class="buttons--main--box">
            <button class="single--button" @click="fillBoard"> Wypełnij wewnętrzne macierze</button>
            <button class="single--button" @click="fillMissingDigit"> Wpisz brakującą cyfrę</button>
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
    </div>
</template>


<script>


export default {
    name: 'ButtonsSingleGame',
    props: ['findEmptyFieldsInMatrix', 'findCorsToFillDigit', 'findMissingDigit', 'fillMissingDigit', 'fillBoard', 'secondStepFindOneMissingDigit', 'secondStepFindOneMissingID', 'secondStepFindCoor', 'thirdStepFindOneMissingID', 'thirdStepFindMissingDigits', 'thirdStepFindCorsToFillDigit', 'thirdStepFillDigitsInRightPlace'],

}



</script>


<style lang="scss" scoped>
.containerApp__game__helpButtons {

    display: flex;
    flex-direction: column;
    height: 500px;
    width: 800px;

    .buttons--main--box,
    .buttons--help--box {
        background-color: rgb(55, 54, 54);
        border-radius: 12px;
        flex-wrap: nowrap;
        margin: 10px;
    }

    .single--button {
        background-color: rgb(18, 22, 22);
        border: solid 2px whitesmoke;
        border-radius: 8px;
        color: whitesmoke;
        cursor: pointer;
        margin: 8px;
        padding: 6px;
        width: 40%;
    }
}
</style>