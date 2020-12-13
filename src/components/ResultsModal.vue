<template>
    <div class="rps-modal flex items-center"> 
        <div class="rps-card mx-auto w-3/4 sm:w-1/2 md:w-1/3"> 
            <h2> MATCH {{match.text}}! </h2>
            <img class="h-32" v-bind:src="match.img" >
            <div class="rps-button-group">
                <button v-on:click="$emit('play-again')"> Play Again? </button>
                <a href="/"> New Game </a>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['match'],
}
</script>

<style scoped lang="scss">

    .rps-button-group {
        display: flex;
        width: 100%;
        justify-content: center;

        button, a {
            padding: 5px 10px;
            border: 3px solid #000;
            border-radius: 10px;
            font-size: 18px;
            margin: 15% 2% 0 2%;
            font-weight: bold;
            transition: all .2s;
            text-align: center;


            &:hover {
                background-color: #000;
                color: #fff;
            }
        }
    }

    .rps-card {
        background-color: #fff;
        border-radius: 10px;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 25px;
        box-shadow: -12px 14px #F3CA3E;

        h2 {
            font-size: 65px;
            color: #F3CA3E;
            text-shadow: -4px 5px rgba(0,0,0,.86) ;
            margin-bottom: 5%;
            text-align: center;
        }
    }

    .rps-modal {
        background-color: rgba(0,0,0,.75);
    }

    @media (max-width: 768px) {
        .rps-card {

            h2 {
                font-size: 52px;
            }
        }
    }
</style>