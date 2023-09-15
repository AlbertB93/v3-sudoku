<template>
    <div>
        <button class="singleButton" @click="displayDescription(numberOfDescription)"> Description</button>
        <button class="singleButton" @click="executeFunctionStep(numberOfDescription)"> Execute</button>
    </div>
</template>

<script>
export default {
    props: ['displayDescription', 'numberOfDescription', 'executeFunctionStep'],
}
</script>

<style lang="scss" scoped>
.singleButton {
    background-color: transparent;
    border: none;
    border-radius: 8px;
    box-shadow: 2px 2px 6px #aad5c5;
    color: white;
    cursor: pointer;
    font-size: 1rem;
    margin: 8px;
    padding: 4px 10px;
    width: auto;
}

.singleButton:hover {
    /*  background-color: #3242a5; */
    box-shadow: none;
    border: solid 1px whitesmoke;
    background-color: rgb(196, 193, 193);
    color: #003019;
    font-weight: 300;
    padding: 4px 10px;
    transition-duration: .5s;
    transform: scale(1.1);
}
</style>