<template>
    <div>
        <button class="button__game" @click="newGame()">
            NEW GAME
        </button>
        <button class="button__draw" @click="drawGame('easy')">
            EASY
        </button>
        <button class="button__draw" @click="drawGame('medium')">
            MEDIUM
        </button>
        <button class="button__draw" @click="drawGame('hard')">
            HARD
        </button>
        <button class="button__game" @click="finishGame()">
            FINISH GAME
        </button>
    </div>
</template>


<script>
export default {
    name: 'SingleButtonGame',
}

</script>

<style lang="scss" scoped>
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
    height: 12vh;
    margin: 12px;
    padding: 8px;
    width: 12vh;
}

.button__draw:hover {
    background-color: #3242a5;
    box-shadow: none;
    border: solid 2px whitesmoke;
    color: whitesmoke;
    transition-duration: .5s;
    transform: scale(1.1);
}
</style>