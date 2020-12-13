<template>
    <div class="screen">
        <div> 
            <img class="rps-logo h-18 sm:h-40 lg:h-48 mx-auto" src="/images/title.png"/>
        </div>
        <div class="rps-button-group mx-auto"> 
            <button v-on:click="selectMode" class="rps-button-game" type="button">Free-Play</button>
            <button v-on:click="selectMode" class="rps-button-game" type="button">Round of 3</button>
            <button v-on:click="selectMode" class="rps-button-game" type="button">Round of 5</button>
        </div>
        <div> 
            <button type="button" v-on:click="$emit('begin',gameMode, errMessage)" class="rps-button mx-auto"> Start Game </button>
        </div>
        <div v-if="errorActive" class="rps-error w-100 md:w-1/2 lg:w-1/3 xl:w-1/4"> 
            <p> Please Select a Game Mode </p>
        </div>
    </div>
</template>

<script>
export default {
    data () {
        return {
            gameMode: '',
            errorActive: false
        }
    },
    methods: {
        selectMode: function (event) {
            let buttons = document.querySelectorAll('.rps-button-group button')
            for (const button of buttons) {
                button.classList.remove('active')
            }
            event.target.classList.add('active')

            this.gameMode = event.target.textContent;

            if (this.errorActive) {
                this.errorActive = false;
            }
            console.log(this.gameMode)
        },
        errMessage: function() {
            this.errorActive = true;
        }

    }
}
</script>

<style scoped lang="scss">

    @keyframes appear {
        0% {opacity: 0;}
        100% {opacity: 1;}
    }

    .active {
        background-color: white;
        color: #6C2B06 !important;
    }

    .rps-button {
        background-color: #FFE606;
        overflow: hidden;
        border-radius: 30px;
        color: #6C2B06;
        padding: 10px 40px;
        font-size: 2.2vw;
        font-weight: 700;
        display: block;
        margin-top: 8%;
        position: relative;
        top:0;
        box-shadow: 0 8px 10px rgba(113,0,0,.5);
        transition: top .3s, box-shadow .35s;
        z-index: 10;
        
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

    .rps-button-game {
        border: 2px solid #fff;
        color: white;
        padding: 7px 45px;
        font-size: 1.5vw;
        letter-spacing: 1px;
        transition: all .3s;
        font-weight: 500;

        &:hover {
            background-color: white;
            color: #6C2B06;
        }

        &:nth-child(1) {
            border-radius: 10px 0 0 10px;
            border-right: 0;
        }

        &:nth-child(3) {
            border-radius: 0 10px 10px 0;
            border-left: 0;
        }
    }

    .rps-button-group {
        margin-top: 5%;
    }

    .rps-error {
        margin: 2% auto;
        animation: appear .3s forwards linear;
    
        p {
            border-radius: 5px;
            text-align: center;
            color: white;
            font-weight: bold;
            background-color: #F72A2A;
            padding: 10px 20px;
            box-shadow: 0 5px 10px rgba(0,0,0,.65);
        }
    }

    .screen {
        background: linear-gradient(135deg, rgba(255,51,173,1) 0%, rgba(225,134,14,1) 100%);
        padding: 40px;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }

    @media (max-width: 392px) {
        .rps-button {
            font-size: 5vw;
        }

        .rps-button-group {
            display: flex;
            flex-direction: column;

            .rps-button-game {
                font-size: 5vw;

                &:nth-child(1) {
                    border-radius:10px 10px 0 0 ;
                    border-bottom: 0;
                    border-right: 2px solid white !important;
                }

                &:nth-child(3) {
                    border-radius: 0 0 10px 10px ;
                    border-top: 0;
                    border-left: 2px solid white!important;
                }
            }

        }

        .rps-error {
            margin-top: 5%;
        }
    }

</style>