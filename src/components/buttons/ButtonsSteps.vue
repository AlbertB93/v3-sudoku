<template>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <label class="buttonLabel"> Step 1</label>
            <SingleButtonStep :displayDescription="displayDescription" :numberOfDescription="arrNumberOfDescriptions[0]"
                :executeFunctionStep="firstStep" />
        </div>
        <div class="stepDescription"> <button @click="findEmptyFieldsInMatrix">1</button> Find miniMatrix ( 2 x 3 ) or
            single row or single column, which has one "zero" <br>
            <img src="../../../images/miniMatrix.png" alt="matrix" class="stepDescription__image">
            <br>
            <button @click="firstStepFindEmptyField">2</button> Find correct coordinates ( which has "0 in field") <br>
            <button @click="findMissingDigit">3</button> Find missing digit<br>
            <button @click="fillMissingDigit"> 4</button> Fill missing digit
        </div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <label class="buttonLabel"> Step 2</label>
            <SingleButtonStep :displayDescription="displayDescription" :numberOfDescription="arrNumberOfDescriptions[1]"
                :executeFunctionStep="secondStep" />
        </div>
        <div class="stepDescription">
            <button @click="secondStepFindOneMissingDigit">1</button> Find digit, which occurs in the game n-1 times. <br>
            <button @click="secondStepFindOneMissingID">2</button> Find miniMatrx, which doesn't have this digit. <br>
            <button @click="secondStepFindCoor">3</button> Find the right place ( corrdinates ) in this miniMatrix <br>
            <button @click="fillMissingDigit">4</button> Fill the missing digit in the right place.
        </div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <label class="buttonLabel"> Step 3</label>
            <SingleButtonStep :displayDescription="displayDescription" :numberOfDescription="arrNumberOfDescriptions[2]"
                :executeFunctionStep="anotherStep" />
        </div>
        <div class="stepDescription">
            <button @click="findMiniMatrixWithZeros(2)">1</button> Find one miniMatrix / row / column, which has two zero
            places.<br>
            <button @click="findMissingDigits(2)">2</button> Find two missing digits in it. <br>
            <button @click="findCorsToFillDigitExtend(2)">3</button> Find coordinates for zero places. <br>
            <button @click="thirdStepFillDigitsInRightPlace(2, 2)">4</button> Try to fill one of two missing digit in the
            right
            place. <br>
            <button @click="fillMissingDigit">5</button> If it's impossible and you found more than one miniMatrix / row /
            column, which has two zeros, try to another
            miniMatrix / row / column. <br>
            If you found only one you have to go to step 4.
        </div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <label class="buttonLabel"> Step 4</label>
            <SingleButtonStep :displayDescription="displayDescription" :numberOfDescription="arrNumberOfDescriptions[3]"
                :executeFunctionStep="anotherStep" />
        </div>
        <div class="stepDescription"> 1. Find one miniMatrix / row / column, which has three zero places.<br>
            2. Find three missing digits in it. <br>
            3. Find coordinates for zero places. <br>
            4. Try to fill one of three missing digit in the right place. <br>
            5. If it's impossible and you found more than one miniMatrix / row / column, which has two zeros, try to another
            miniMatrix / row / column. <br>
            If you found only one you have to go to step 5.
        </div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <label class="buttonLabel"> Step 5</label>
            <SingleButtonStep :displayDescription="displayDescription" :numberOfDescription="arrNumberOfDescriptions[4]"
                :executeFunctionStep="anotherStep" />
        </div>
        <div class="stepDescription"> 1. Find one miniMatrix / row / column, which has four zero places.<br>
            2. Find four missing digits in it. <br>
            3. Find coordinates for zero places. <br>
            4. Try to fill one of four missing digit in the right place. <br>
            5. If it's impossible and you found more than one miniMatrix / row / column, which has two zeros, try to another
            miniMatrix / row / column. <br>
            If you found only one and it's impossible to fill digit - sorry, this App can't solve this game.
        </div>
    </div>
</template>


<script>
import SingleButtonStep from './SingleButtonStep.vue';

export default {
    name: 'ButtonsSingleGame',
    components: { SingleButtonStep },
    props: ['findEmptyFieldsInMatrix', 'firstStepFindEmptyField', 'findMissingDigit', 'fillMissingDigit',
        'secondStepFindOneMissingDigit', 'secondStepFindOneMissingID', 'secondStepFindCoor',
        'findMiniMatrixWithZeros', 'findMissingDigits', 'findCorsToFillDigitExtend', 'thirdStepFillDigitsInRightPlace',
        'thirdStepFindCorsToFillDigit', 'fourthStepFindCorsToFillDigit', 'fourthStepFillDigitsInRightPlace', 'fourthStepFillDigitsInRightPlace5', 'findCorsToFillDigit',
        'firstStep', 'secondStep', 'anotherStep',
    ],
    setup() {

        const arrNumberOfDescriptions = [0, 1, 2, 3, 4];
        let numberOfDescription;

        function displayDescription(descriptionNumber) {
            let arrayOfDescriptions = document.getElementsByClassName('stepDescription');
            arrayOfDescriptions[descriptionNumber].classList.contains("hidden") ? arrayOfDescriptions[descriptionNumber].classList.remove("hidden") : arrayOfDescriptions[descriptionNumber].classList.add("hidden");
        }
        return { displayDescription, arrNumberOfDescriptions, numberOfDescription };
    },
}
</script>


<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@200;300&display=swap');

.containerApp__game--steps--singleStep {
    background-color: #1b7e90;
    background-image: linear-gradient(to left top, #032709, #003019, #003a28, #004337, #004d46, #005451, #005c5c, #026368, #066a72, #0c707c, #137786, #1b7e90);
    border-radius: 12px;
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
    justify-content: space-around;
    height: auto;
    margin: 6px 0px;
    width: 90%;

    .singleStep--box {
        display: flex;
        flex-direction: row;
        justify-content: space-around;

        .buttonLabel {
            background-color: transparent;
            border: solid 1px whitesmoke;
            border-radius: 6px;
            color: white;
            font-size: 1rem;
            font-weight: 400;
            margin: 8px;
            padding: 4px;
            width: auto;
        }
    }

    .stepDescription {
        border-top: solid 1px whitesmoke;
        color: whitesmoke;
        display: none;
        font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
        font-family: 'Raleway', sans-serif;
        text-align: left;
        padding: 12px;

        &__image {
            margin: 4px 30%;
            max-height: 100px;
            max-width: 100px;

        }
    }

    .hidden {
        display: block;
    }
}
</style>