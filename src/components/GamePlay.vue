<template>
    <div class="play relative">
        <header class="rps-header">
            <a href="/"> 
                <img class="rps-logo h-12 mx-auto" src="/images/title.png"/>
            </a>
            
        </header>
        <h1 class="rps-round mx-auto"> ROUND {{currentRound}} </h1>
        <div v-show="modes.freePlay"  class="rps-score mx-auto flex">
            <div class="w-1/3">
                <p class="text-center"> Wins: <span style="color:#FF5F58"> {{playerScore}} </span></p>
            </div>
            <div class="w-1/3">
                <p class="text-center"> Ties: <span style="color:#FF5F58"> {{tieScore}} </span></p>
            </div>
            <div class="w-1/3">
                <p class="text-center"> Wins: <span style="color:#FF5F58"> {{cpuScore}} </span></p>
            </div>
        </div>
        <div class="rps-content flex justify-around mx-auto container">
            <div class="rps-player flex-auto w-1/3">
                <div class="rps-box">
                    <h2> Player1 </h2>
                    <img class="h-24 md:h-32 lg:h-48" v-bind:src="playerImage"/>
                </div>
                <div 
                v-if="modes.roundOf3 || modes.roundOf5" 
                v-html="modes.roundOf3 ? roundCount.three : modes.roundOf5 ? roundCount.five : ''" 
                class="rps-circles">
                </div>
            </div>
            <div  class="rps-info flex-auto w-1/3 hidden sm:block">
                <div v-show="roundResult" class="rps-results">
                    <h2> {{roundResultText}} </h2>
                    <p> {{roundResultPlays}} </p>
                    <button v-on:click="continueGame"> Click To Continue </button>
                </div>
            </div>
            <div class="rps-cpu flex-auto w-1/3 ">
                <div class="rps-box">
                    <h2> CPU </h2>
                    <img class=" h-24 md:h-32 lg:h-48" v-bind:src="cpuImage"/>
                </div>
                <div 
                v-if="modes.roundOf3 || modes.roundOf5" 
                v-html="modes.roundOf3 ? roundCount.three : modes.roundOf5 ? roundCount.five : ''" 
                class="rps-circles">
                </div>
            </div>
        </div>
        <div  class="rps-info sm:hidden block mx-auto">
            <div v-show="roundResult" class="rps-results mobile">
                <h2> {{roundResultText}} </h2>
                <p> {{roundResultPlays}} </p>
                <button v-on:click="continueGame"> Click To Continue </button>
            </div>
        </div>
        <div v-show="!roundResult" class="rps-button-group w-2/3 sm:w-1/2 lg:w-1/4 xl:w-1/5 flex justify-between mx-auto"> 
            <ButtonOption 
                v-for="button in buttons"
                v-bind:key="button.id"
                v-bind:button="button"
                v-on:option="playRound"
            />
        </div>
        <button class="rps-reset" v-on:click="resetGame"> Start Over  </button>
        <ResultsModal 
            v-show="modalActive"
            v-bind:match="matchResult"
            v-on:play-again="playAgain"
            class="modal"
        />
    </div>
</template>

<script>
import ButtonOption from './ButtonOption.vue'
import ResultsModal from './ResultsModal.vue'


export default {
    components: {
		ButtonOption, ResultsModal
    },
    props: ['modes', 'state'],
    data () {
        return {
            buttons: [
                {
                    id: 1,
                    img: "/images/rock-btn.png",
                    option: "Rock"
                },
                {
                    id: 2,
                    img: "/images/paper-btn.png",
                    option: "Paper"
                },
                {
                    id: 3,
                    img: "/images/scissors-btn.png",
                    option: "Scissors"
                }
            ],
            choices: ["Rock", "Paper", "Scissors"],
            cpuScore: 0,
            playerScore: 0,
            tieScore: 0,
            cpuImage: '',
            playerImage: '',
            currentRound: 1,
            roundResult: false,
            roundResultPlays: "",
            roundResultText: "",
            matchResult: {
                text: "",
                img: ""
            },
            modalActive: false,
            roundCount: {
                three: 
                    `<span class="circle"></span>
                    <span class="circle"></span>
                    <span class="circle"></span>`,
                five:  
                    `<span class="circle"></span>
                    <span class="circle"></span>
                    <span class="circle"></span>
                    <span class="circle"></span>
                    <span class="circle"></span>`
            }
        }
    },
    methods: {
        continueGame: function () {
            this.currentRound++
            this.roundResult = false;
            this.cpuImage = ''
            this.playerImage = ''

            const images = document.querySelectorAll('.rps-box img');

            images[0].classList.remove('img-slide-l')
            images[1].classList.remove('img-slide-r')
        },
        gameOver: function(){
            if(this.playerScore > this.cpuScore) {
                // Player wins
                this.matchResult.text = "WON";
                this.matchResult.img = "/images/won-icon.png";
            } else {
                // Computer Wins
                this.matchResult.text = "LOSS";
                this.matchResult.img = "/images/loss-icon.png";
            }
            
            // Make Modal Active
            this.modalActive = true;

            // Transition for modal to appear
            setTimeout(() => {
                this.$children[3].$el.style.opacity = "1";
                this.$children[3].$el.style.height = "100%";
            },10); 
            
        },
        playAgain: function() {
            this.resetGame();
            setTimeout(() => {
                this.$children[3].$el.style.opacity = "0";
                this.$children[3].$el.style.height = "0";
            },200);

            this.modalActive = false;
        },
        playRound: function(playerChoice) {
            
            // Pick a random number to select CPU choice
            let randomIndex = Math.floor(Math.random() * (this.choices.length));
            let cpuChoice = this.choices[randomIndex];

            //console.log('Player:', playerChoice)
            //console.log('Cpu:', cpuChoice)

            // Select CPU & Player Image
            this.cpuImage = `/images/${cpuChoice}.png`
            this.playerImage = `/images/${playerChoice}.png`

            // Add slide in effect for images
            const images = document.querySelectorAll('.rps-box img');
            images[0].classList.add('img-slide-l')
            images[1].classList.add('img-slide-r')
        

            // Results for tie
            if(playerChoice == cpuChoice) {
                this.tieScore++;
                this.resultText("tie", playerChoice, cpuChoice);
            }
            
            // Results for rock
            if(playerChoice == "Rock") {
                if(cpuChoice == "Scissors") {
                    this.playerScore++;
                    this.resultText("won", playerChoice, cpuChoice);
                    setTimeout(() => {
                        this.fillCircle(".rps-player .circle", this.playerScore);
                    },10);
                } else if(cpuChoice == "Paper") {
                    this.cpuScore++;
                    this.resultText("lost", playerChoice, cpuChoice);
                    setTimeout(() => {
                        this.fillCircle(".rps-cpu .circle", this.cpuScore)
                    },10);
                }
            }
            // Results for paper
            else if (playerChoice == "Paper") {
                if(cpuChoice == "Rock") {
                    this.playerScore++;
                    this.resultText("won", playerChoice, cpuChoice);
                    setTimeout(() => {
                        this.fillCircle(".rps-player .circle", this.playerScore);
                    },10);
                } else if(cpuChoice == "Scissors") {
                    this.cpuScore++;
                    this.resultText("lost", playerChoice, cpuChoice);
                    setTimeout(() => {
                        this.fillCircle(".rps-cpu .circle", this.cpuScore);
                    },10);
                }
            }
            // Results for scissors
            else if(playerChoice == "Scissors") {
                if(cpuChoice == "Paper") {
                    this.playerScore++;
                    this.resultText("won", playerChoice, cpuChoice);
                    setTimeout(() => {
                        this.fillCircle(".rps-player .circle", this.playerScore);
                    },10);
                }else if(cpuChoice == "Rock") {
                    this.cpuScore++;
                    this.resultText("lost", playerChoice, cpuChoice);
                    setTimeout(() => {
                        this.fillCircle(".rps-cpu .circle", this.cpuScore);
                    },10);
                }
            }
            
            else {
                alert("Uh-oh. You didn't make a valid choice. We don't allow " + this.playerChoice);
            }

            /* 
            The value of 'this' is different inside the setTimeout.
            If you're using ES6, you can use an arrow function: 
            */

            setTimeout(() => { this.roundResult = true}, 600);

            // Round of 5
            if (this.modes.roundOf5) {
                const winningScore = 5;
                if(this.playerScore == winningScore || this.cpuScore == winningScore) {
                    setTimeout(() => {
                        this.gameOver();
                    },700);
                };
            // Round of 3
            } else if (this.modes.roundOf3) {
                const winningScore = 3;
                if(this.playerScore == winningScore || this.cpuScore == winningScore) {
                    setTimeout(() => {
                        this.gameOver();
                    },700);
                };
            }
        },
        resetGame: function () {

            // Reset game scores
            this.playerScore = 0;
            this.cpuScore = 0;
            this.tieScore = 0;
            
            // Empty image containers
            this.cpuImage = ''
            this.playerImage = ''

            // Clear text for round results
            this.roundResultText = "";
            this.roundResultPlays = "";

            // Set round to round 1 
            // Reset results from appearing on screen
            this.roundResult = false;
            this.currentRound = 1;

            // Reset circles back to empty
            if (this.modes.roundOf5 || this.modes.roundOf3 ) {
                let circles = document.querySelectorAll('.circle');

                for (const circle of circles) {
                    circle.style.backgroundColor = "white"
                }
            }
        },
        resultText: function (result, player, cpu) {
            switch(result) {
                case "tie":
                    this.roundResultText = "It's a Tie!";
                    this.roundResultPlays = "You both played " + player;
                    break;

                case "won":
                    this.roundResultText = "You won!";
                    this.roundResultPlays = player + " beats " + cpu;
                    break;
                    
                case "lost":
                    this.roundResultText = "You lost!";
                    this.roundResultPlays = cpu + " beat your " + player;
                    break;

                default:
                    console.log("Unexpected result status");
            }
        },
        fillCircle: function (group, pos) {
            if (this.modes.roundOf5 || this.modes.roundOf3 ) {
                let circles = document.querySelectorAll(group);
            
                console.log(pos)
                let circle = circles[pos - 1];

                circle.style.backgroundColor = "black"
            }
        }
    }
}
</script>

<style lang="scss">
    .rps-circles  {
        display: flex;
        justify-content: center;
        margin-top: 5%;
        

        .circle {
            border-radius: 100%;
            background-color: white;
            border: 5px solid #000;
            width: 3vw;
            height: 3vw;
            display: block;
            transition: all .4s;
            margin: 0 2%;
        }
    }

    @media (max-width: 768px) {
        .rps-circles .circle {
            width: 4vw;
            height: 4vw;
        }
    }

    @media (max-width: 392px) {
        .rps-circles .circle {
            width: 6vw;
            height: 6vw;
        }
    }
</style>
<style scoped lang="scss">
    $rps-yellow: #F3CA3E;

    .img-slide-r {
        transform: scaleX(-1) translateX(0) !important;
    }

    .img-slide-l {
        transform: scaleX(1) translateX(0) !important;
    }

    .modal {
        position: fixed;
        z-index: 1;
        top: 0;
        right: 0;
        left: 0;
        bottom: 0;
        transition: all .4s;
        height: 0;
        opacity: 0;
    }

    .rps-box {
        border: 5px solid #000;
        border-radius: 30px;
        padding: 20px;
        max-width: 362px;
        width: 100%;
        height: 408px;
        margin: auto;
        box-sizing: border-box;
        overflow: hidden;
        position: relative;

        h2 {
            text-align: center;
            font-size: 60px;
            letter-spacing: 3px;
        }

        img {
            padding-top: 8%;
            margin: auto;
            transition: all .4s;
        }
    }

    .rps-button-group {
        position: absolute;
        bottom: 8%;
        left: 0;
        right: 0;
    }

    .rps-content, .rps-score {
        max-width: 1200px;
    }

    .rps-cpu {
        img {
            transform: scaleX(-1) translateX(-50em);
        }
    }

    .rps-header {
        background-color: $rps-yellow;
        padding: 10px 0;
    }

    .rps-info {
        max-width: 459px;
    }

    .rps-player {

        img {
            transform: translateX(-50em)
        }
    }

    .rps-results {

        text-align: center;
        margin-top: 25%;

        button {
            padding: 5px 10px;
            border: 3px solid #000;
            border-radius: 10px;
            font-size: 18px;
            margin-top: 15%;
            font-weight: bold;
            transition: all .2s;

            &:hover {
                background-color: #000;
                color: #fff;
            }
        }

        h2 {
            font-size: 50px;
            color: $rps-yellow;
            line-height: 0.85;
        }

        p {
            font-size: 30px;
            font-weight: 500;
        }
        
    }

    .rps-reset {
        position: fixed;
        right: 1%;
        bottom: 1%;
        background-color: #FF5F58;
        padding: 5px 20px;
        color: white;
        font-weight: bold;
        border-radius: 10px;
        font-size: 18px;

    }

    .rps-round {
        font-size: 80px;
        color: #FF5F58;
        text-shadow: -5px 8px $rps-yellow;
        text-align: center;
        margin-top: 3%;
    }

    .rps-score {

        p {
            font-size: 30px;
            font-weight: bold;
        }
    }

    /* 
    MEDIA QUERIES
     */
        @media(max-width: 1024px) {
            .rps-content, .rps-score {
                max-width: 1000px;
                padding: 10px 50px;
            }

            .rps-button-group {
                position: relative;
                bottom: 0;
                margin-top: 50px;
            }

            .rps-round {
                margin: 5% 0;
            }
            
        }

        @media(max-width: 768px) {
            .rps-box {
                height: 328px;
            }

            .rps-results {

                margin-top: 35%;

                h2 {
                    font-size: 40px;
                }

                p {
                    font-size: 18px;
                }
            }
        }

        @media(max-width: 392px) {
            .rps-player, .rps-cpu {
                padding: 10px;

                .rps-box {
                    height: 228px;

                    h2 {
                        font-size: 170% !important;
                    }
                }

            }

            .rps-content, .rps-score {
                padding: 10px 0px;
            }

            .rps-results.mobile {
                margin-top: 3%;

                button {
                    margin-top: 5%;
                }
            }

            .rps-score {
                p {
                    font-size: 120%;
                }
            }
        }
    
</style>
