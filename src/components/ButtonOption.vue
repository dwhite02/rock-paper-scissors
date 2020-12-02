<template>
    <button v-on:click="$emit('option', button.option)" class="rps-button-option">
        <img class="h-16" v-bind:src="button.img"/>
    </button>
</template>

<script>
export default {
    props: ['button']
}
</script>

<style lang="scss" scoped>
    $rps-yellow: #F3CA3E;

    .rps-button-option {
        background-color: $rps-yellow;
        width: 60px;
        height:60px;
        transform: rotate(45deg);
        box-shadow: 8px 10px #000;
        transition: background-color .2s;

        &:hover {
            background-color: #FF5F58;
        }

        img {
            transform: rotate(-45deg);
            margin: auto;
            padding: 10px;
        }
    }
</style>