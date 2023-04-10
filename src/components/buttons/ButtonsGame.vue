<template>
    <div class="containerApp__game__buttons">
        <div class="containerApp__game__buttons--title">
            <p>EASY</p>
        </div>
        <div class="containerApp__game__buttons--box">
            <button class="buttons--game" @click="drawGame(0)">1</button>
            <button class="buttons--game" @click="drawGame(1)">2</button>
            <button class="buttons--game" @click="drawGame(2)">3</button>
            <button class="buttons--game" @click="drawGame(3)">4</button>
            <button class="buttons--game" @click="drawGame(4)">5</button>
            <button class="buttons--game" @click="drawGame(5)">6</button>
        </div>
        <div class="containerApp__game__buttons--title">
            <p>MEDIUM</p>
        </div>
        <div class="containerApp__game__buttons--box">
            <!--             <button class="buttons--game" @click="nextMove">Wykonaj kolejny ruch</button>
            <button class="buttons--game" @click="checkGame">Sprawdź planszę - mały kwadracik</button>
            <button class="buttons--game" @click="finishGame">Rozwiąż planszę</button>
            <button class="buttons--game" @click="fillBoard">Wypełnij mini macierze</button> -->
        </div>
        <div class="containerApp__game__buttons--title">
            <p>HARD</p>
        </div>
        <div class="containerApp__game__buttons--box">
                <button class="buttons--game" @click="drawGame(6)">1</button>
            <button class="buttons--game" @click="drawGame(7)">1a</button>
            <!--             <button class="buttons--game" @click="nextMove">Wykonaj kolejny ruch</button>
            <button class="buttons--game" @click="checkGame">Sprawdź planszę - mały kwadracik</button>
            <button class="buttons--game" @click="finishGame">Rozwiąż planszę</button>
            <button class="buttons--game" @click="fillBoard">Wypełnij mini macierze</button> -->
        </div>
    </div>
</template>

<script>
export default {
    name: 'ButtonsGame',
    props: ['fillBoard', 'drawGame'],
};

</script>

<style lang="scss" scoped>
.containerApp__game__buttons {
    display: flex;
    border: solid 3px whitesmoke;
    border-radius: 12px;
    flex-direction: column;
    justify-content: space-around;
    height: 420px;
    margin: 20px;
    width: 40vw;

    &--title {
        color: whitesmoke;
        font-size: 24px;
        margin: 10px;
        text-align: center;
    }

    &--box {
        align-items: center;
       
        display: flex;
        flex-direction: row;
        justify-content: start;

        .buttons--game {
            background-color: rgb(55, 54, 54);
            border: solid 2px whitesmoke;
            color: whitesmoke;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            height: auto;
            margin: 10px;
            padding: 10px;
            width: auto;
        }
    }
}
</style>