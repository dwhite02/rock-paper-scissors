<template>
    <div class="screen">
        <div> 
            <img class="rps-logo h-20 sm:h-48 lg:h-64 mx-auto" src="/images/title.png"/>
        </div>
        <div> 
            <div v-on:click="$emit('begin')" class="overflow-hidden rps-button mx-auto"> <p>Start Game</p> </div>
        </div>
    </div>
</template>



<style scoped lang="scss">
    .rps-button {
        background-color: #FFE606;
        border-radius: 30px;
        color: #fff;
        padding: 10px 40px;
        font-size: 40px;
        font-weight: 700;
        margin-top: 12%;
        position: relative;
        top:0;
        box-shadow: 0 8px 10px rgba(113,0,0,.5);
        transition: top .3s, box-shadow .35s;
        cursor: pointer;
        

        &:hover {
            top: 4px;
            box-shadow: 0 3px 8px;

            &::before {
            left: 125%;
            opacity: .65;
        }
        }

        &::before {
            position: absolute;
            content: "";
            top: 0;
            left: -15%;
            background-color: #fff;
            width: 11%;
            height: 100%;
            transform: skew(-15deg);
            transition: all .5s;
            opacity: 1;
        }
    }

    

    .screen {
        background: linear-gradient(135deg, rgba(255,51,173,1) 0%, rgba(225,134,14,1) 100%);
        padding: 40px;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }

</style>