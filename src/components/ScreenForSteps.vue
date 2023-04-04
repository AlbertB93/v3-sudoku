<template>
    <div class="containerApp__game__screen">
        <h3> Co uzupełniliśmy w sudoku:</h3>

        <div class="rowOfStep" id="rowOfStep0">

        </div>
        <div class="rowOfStep" id="rowOfStep1">

        </div>
        <div class="rowOfStep" id="rowOfStep2">

        </div>
        <div class="rowOfStep" id="rowOfStep3">

        </div>
        <div class="rowOfStep" id="rowOfStep4">

        </div>
        <div class="rowOfStep" id="rowOfStep5">

        </div>
        <div class="rowOfStep" id="rowOfStep6">

        </div>
        <div class="rowOfStep" id="rowOfStep7">

        </div>
        <div class="rowOfStep" id="rowOfStep8">

        </div>
        <div class="rowOfStep" id="rowOfStep9">

        </div>
        <div class="rowOfStep" id="rowOfStep10">

        </div>
        <div class="rowOfStep" id="rowOfStep11">

        </div>
        <div class="rowOfStep" id="rowOfStep12">

        </div>
        <div class="rowOfStep" id="rowOfStep13">

        </div>
        <div class="rowOfStep" id="rowOfStep14">

        </div>
        <div class="rowOfStep" id="rowOfStep15">

        </div>
        <div class="rowOfStep" id="rowOfStep16">

        </div>
    </div>
</template>

<script>
export default {
    name: 'ScreenForSteps',
    /* props: ['fillBoard'], */
};

</script>

<style lang="scss" scoped>
.containerApp__game__screen {
    border: solid 2px whitesmoke;
    color: rgb(184, 0, 0);
    height: 700px;
    width: 400px;

    .rowOfStep {
        /*         border: solid 1px orange;
        height: 20px; */
        margin: 2px auto;
        padding: 2px;
        /*         width: 90%; */
    }


}
</style>