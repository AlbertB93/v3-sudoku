<template>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <button class="singleButtonNew" @click="secondStepFindOneMissingDigit"> Krok 1</button>
            <button class="singleButtonNew2" @click="displayDescription"> Opis</button>
            <button class="singleButtonNew" @click="firstStep"> Wykonaj</button>
        </div>
        <div class="stepDescription"> Lorem ipsum dolor sit amet, consectetur adipisicing elit.
            Laboriosam ipsum accusantium neque velit assumenda culpa quisquam nihil, suscipit, tempora repudiandae,
            placeat facere a libero! Eaque necessitatibus reprehenderit fugit ipsam mollitia?</div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <button class="singleButtonNew" @click="secondStepFindOneMissingDigit"> Krok 2</button>
            <button class="singleButtonNew2" @click="displayDescription"> Opis</button>
            <button class="singleButtonNew" @click="secondStep"> Wykonaj</button>
        </div>
        <div class="stepDescription"> Lorem ipsum dolor sit amet, consectetur adipisicing elit.
            Laboriosam ipsum accusantium neque velit assumenda culpa quisquam nihil, suscipit, tempora repudiandae,
            placeat facere a libero! Eaque necessitatibus reprehenderit fugit ipsam mollitia?</div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <button class="singleButtonNew" @click="secondStepFindOneMissingDigit"> Krok 3</button>
            <button class="singleButtonNew2" @click="displayDescription"> Opis</button>
            <button class="singleButtonNew" @click="thirdStep"> Wykonaj</button>
        </div>
        <div class="stepDescription"> Lorem ipsum dolor sit amet, consectetur adipisicing elit.
            Laboriosam ipsum accusantium neque velit assumenda culpa quisquam nihil, suscipit, tempora repudiandae,
            placeat facere a libero! Eaque necessitatibus reprehenderit fugit ipsam mollitia?</div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <button class="singleButtonNew" @click="secondStepFindOneMissingDigit"> Krok 4</button>
            <button class="singleButtonNew2" @click="displayDescription"> Opis</button>
            <button class="singleButtonNew" @click="firstStep"> Wykonaj</button>
        </div>
        <div class="stepDescription"> Lorem ipsum dolor sit amet, consectetur adipisicing elit.
            Laboriosam ipsum accusantium neque velit assumenda culpa quisquam nihil, suscipit, tempora repudiandae,
            placeat facere a libero! Eaque necessitatibus reprehenderit fugit ipsam mollitia?</div>
    </div>
    <div class="containerApp__game--steps--singleStep">
        <div class="singleStep--box">
            <button class="singleButtonNew" @click="secondStepFindOneMissingDigit"> Krok 5</button>
            <button class="singleButtonNew2" @click="displayDescription"> Opis</button>
            <button class="singleButtonNew" @click="firstStep"> Wykonaj</button>
        </div>
        <div class="stepDescription"> Lorem ipsum dolor sit amet, consectetur adipisicing elit.
            Laboriosam ipsum accusantium neque velit assumenda culpa quisquam nihil, suscipit, tempora repudiandae,
            placeat facere a libero! Eaque necessitatibus reprehenderit fugit ipsam mollitia?</div>
    </div>
</template>


<script>



export default {
    name: 'ButtonsSingleGame',

    props: ['findEmptyFieldsInMatrix', 'findCorsToFillDigit', 'findMissingDigit', 'fillMissingDigit', 'fillBoard', 'secondStepFindOneMissingDigit', 'secondStepFindOneMissingID', 'secondStepFindCoor', 'thirdStepFindCorsToFillDigit', 'thirdStepFillDigitsInRightPlace', 'finishGame', 'firstStep', 'secondStep', 'thirdStep', 'drawGame',
        'fourthStepFindCorsToFillDigit', 'fourthStepFillDigitsInRightPlace', 'fourthStep', 'findMiniMatrixWithZeros', 'findMissingDigits', 'findCorsToFillDigitExtend', 'fourthStepFillDigitsInRightPlace5', 'fifthStep'
    ],
    setup() {
        function displayDescription() {
            let test = document.querySelector('.stepDescription');

            /*   test.classList.add("hidden") */

            test.classList.contains("hidden") ? test.classList.remove("hidden") : test.classList.add("hidden")
        }

        return { displayDescription }
    }
}

</script>


<style lang="scss" scoped>
.containerApp__game--steps--singleStep {

    box-shadow: -4px 6px 10px #aad5c5;
    background-color: #1b7e90;
    background-image: linear-gradient(to left top, #032709, #003019, #003a28, #004337, #004d46, #005451, #005c5c, #026368, #066a72, #0c707c, #137786, #1b7e90);
    border-radius: 12px;
    display: flex;
    flex-direction: column;
    flex-wrap: nowrap;
    justify-content: space-around;
    height: auto;
    margin-top: 20px;
    width: 80%;
/*     border: solid 2px red;  */

    .singleStep--box {
        display: flex;
        flex-direction: row;
        justify-content: space-around;

        .singleButtonNew,
        .singleButtonNew2 {
            background-color: transparent;
            border: solid 1px whitesmoke;
            border-radius: 8px;
            color: white;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            margin: 8px;
            padding: 6px;
            width: auto;
        }

    }


    .stepDescription {
        border-top: solid 1px whitesmoke;
        color: whitesmoke;
        display: none;
        padding: 4px;
    }


    .hidden {
        display: block;
    }

}
</style>