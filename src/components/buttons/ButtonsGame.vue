<template>
    <div class="containerApp__game__buttons">
        <div class="containerApp__game__buttons--title">
<p>Przyciski do gry jak będziemy wczytywać Sudoku. Aktualnie korzystamy tylko z ostatniego.
</p>
        </div>
        <div class="containerApp__game__buttons--box">
            <button class="buttons--game" @click="confirmBoardGame">Zatwierdź planszę</button>
            <button class="buttons--game" @click="nextMove">Wykonaj kolejny ruch</button>
            <button class="buttons--game" @click="checkGame">Sprawdź planszę - mały kwadracik</button>
            <button class="buttons--game" @click="finishGame">Rozwiąż planszę</button>
            <button class="buttons--game" @click="fillBoard">Wypełnij mini macierze</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ButtonsGame',
    props: ['fillBoard'],
};

</script>

<style lang="scss" scoped>
.containerApp__game__buttons {
    display: flex;
    border: solid 3px yellowgreen;
    flex-direction: column;
    height: 100px;
    margin: 20px;
    width: 90vw;

    &--title {
        color: yellowgreen;
        margin: 10px;
        text-align: center;
    }

    &--box {
        align-items: center;
        display: flex;
        flex-direction: row;
        justify-content: space-around;

        .buttons--game {
            background-color: aqua;
            border: solid 2px white;
            border-radius: 12px;
            cursor: pointer;
            font-size: 10px;
            font-weight: 600;
            height: auto;
            margin: 10px 0px;
            padding: 12px;
            width: auto;
        }
    }
}
</style>