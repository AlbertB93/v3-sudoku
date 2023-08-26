<template>

<div class="containerApp__buttons">
        <SingleButtonGame />
    </div>
</template>

<script>
import SingleButtonGame from "./SingleButtonGame.vue";
export default {
    name: 'ButtonsGame',
    components: {
        SingleButtonGame,
    },
    props: ['drawGame', 'newGame', 'finishGame',],
};

</script>

<style lang="scss" scoped>
.containerApp__buttons {
        align-items: center;
        background-image: linear-gradient(to left top, #032709, #003019, #003a28, #004337, #004d46, #005451, #005c5c, #026368, #066a72, #0c707c, #137786, #1b7e90);
        border-radius: 12px;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        height: 16vh;
        justify-content: center;
        margin: 4vh 0;
        width: auto;

    .gameButtons--title {
        border-bottom: solid 1px whitesmoke;
        color: whitesmoke;
        font-size: 1.2rem;
        font-weight: 600;
        margin: 10px;
        text-align: center;
    }




}
</style>