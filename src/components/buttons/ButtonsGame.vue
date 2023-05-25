<template>
    <div class="containerApp__buttons--gameButtons">
        <button class="button__game" @click="drawGame('easy')">
            NEW GAME
        </button>
        <div class="gameButtons--title">
            <p> Draw Game</p>
        </div>
        <button class="button__draw" @click="drawGame('easy')">
            EASY
        </button>
        <button class="button__draw" @click="drawGame('medium')">
            MEDIUM
        </button>
        <button class="button__draw" @click="drawGame('hard')">
            HARD
        </button>
        <button class="button__game" @click="drawGame('medium')">
            FINISH GAME
        </button>
    </div>
</template>

<script>
export default {
    name: 'ButtonsGame',
    props: ['fillBoard', 'drawGame'],
};

</script>

<style lang="scss" scoped>
.containerApp__buttons {


    .gameButtons--title {
        border-bottom: solid 1px whitesmoke;
        color: whitesmoke;
        font-size: 1.2rem;
        font-weight: 600;
        margin: 10px;
        text-align: center;
    }


    .button__game {
        background: transparent;
        border: none;
        box-shadow: 4px 4px 10px #aad5c5;
        color: whitesmoke;
        cursor: pointer;
        font-size: 1rem;
        margin: 12px;
        padding: 8px;
    }

    .button__game:hover {
        background-color: #3242a5;
        box-shadow: none;
        border: solid 2px whitesmoke;
        color: whitesmoke;
        transition-duration: .5s;
        transform: scale(1.1);
    }

    .button__draw {
        background: transparent;
        border: none;
        border-radius: 60px;
        box-shadow: 4px 4px 10px #aad5c5;
        color: whitesmoke;
        cursor: pointer;
        font-size: 0.8rem;
        height: 70px;
        margin: 12px;
        padding: 8px;
        width: 70px;
    }

    .button__draw:hover {
        background-color: #3242a5;
        box-shadow: none;
        border: solid 2px whitesmoke;
        color: whitesmoke;
        transition-duration: .5s;
        transform: scale(1.1);
    }

}
</style>