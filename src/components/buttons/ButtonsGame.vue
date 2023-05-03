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
            <button class="buttons--game" @click="drawGame(6)">7</button>
            <button class="buttons--game" @click="drawGame(7)">8</button>
            <button class="buttons--game" @click="drawGame(8)">9</button>
            <button class="buttons--game" @click="drawGame(9)">10</button>

        </div>
        <div class="containerApp__game__buttons--title">
            <p>MEDIUM</p>
        </div>
        <div class="containerApp__game__buttons--box">
            <button class="buttons--game" @click="drawGame(0)">1</button>
            <button class="buttons--game" @click="drawGame(1)">2</button>
            <button class="buttons--game" @click="drawGame(2)">3</button>
            <button class="buttons--game" @click="drawGame(3)">4</button>
            <button class="buttons--game" @click="drawGame(4)">5</button>
            <button class="buttons--game" @click="drawGame(5)">6</button>
            <button class="buttons--game" @click="drawGame(6)">7</button>
            <button class="buttons--game" @click="drawGame(7)">8</button>
            <button class="buttons--game" @click="drawGame(8)">9</button>
            <button class="buttons--game" @click="drawGame(9)">10</button>
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
                <button class="buttons--game" @click="drawGameHard(0)">1</button>
                <button class="buttons--game" @click="drawGameHard(1)">2</button>
                <button class="buttons--game" @click="drawGameHard(2)">2a</button>
                      <button class="buttons--game" @click="drawGameHard(3)">2b</button>
                      <button class="buttons--game" @click="drawGameHard(4)">3</button>

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
    props: ['fillBoard', 'drawGame','drawGameHard'],
};

</script>

<style lang="scss" scoped>
.containerApp__game__buttons {
    display: flex;
/*     border: solid 3px whitesmoke;
    border-radius: 12px;
    box-shadow: -4px 6px 10px white; */
      /*   border: solid 3px rgb(73, 31, 225); */
    border-radius: 12px;
    box-shadow: -4px 6px 10px #4ea4f0;
      background: linear-gradient(#2e8bba, #000d56);
    flex-direction: column;
    justify-content: space-around;
    height: 420px;
    margin: 20px;
    padding: 0px 30px;
    width: auto;

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
/*             background-color: rgb(55, 54, 54);
            border: solid 2px whitesmoke;
            border-radius: 20px;
            color: whitesmoke; */
                      background-color: rgb(0, 0, 0);
 background-color: #9198e5;
            border: solid 2px rgb(57, 5, 200);
            border-radius: 20px;
                box-shadow: -2px 2px 10px rgb(57, 5, 200);
                  background-color: #000d56;
            color: whitesmoke;;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            height: 40px;
            margin: 10px;
            padding: 5px;
            width: 40px;
        }
    }
}
</style>