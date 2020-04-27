<template>
    <div id="app">
        <div class="wrapper clearfix">
            <Players
                v-bind:scorePlayer="scorePlayer"
                v-bind:activePlayer="activePlayer"
                v-bind:currentScore="currentScore"
                v-bind:isWinner="isWinner"
            ></Players>

            <Controls
                v-bind:isPlaying="isPlaying"
                v-on:handleNewGame="handleNewGame"
                v-on:handleRollDice="handleRollDice"
                v-on:handleHoldScore="handleHoldScore"
                v-bind:finalScore="finalScore"
                v-on:handleChangeScore="handleChangeScore"
            ></Controls>

            <Dices
                v-bind:dices="dices"
            ></Dices>

            <PopupRule
                v-bind:isOpenPopup="isOpenPopup"
                v-on:handleConfirm="handleConfirm"
            ></PopupRule>
        </div>
    </div>
</template>


<script>
    import Players from './components/Players';
    import Controls from "./components/Controls";
    import Dices from "./components/Dices";
    import PopupRule from "./components/PopupRule";

    export default {
        name: 'app',
        components: {PopupRule, Dices, Controls, Players},
        data() {
            return {
                isPlaying: false,
                isOpenPopup: false,
                activePlayer: 1,
                scorePlayer : [0, 0],
                currentScore: 30,
                dices: [5, 5],
                finalScore: 10
            }
        },
        methods: {
            handleConfirm(){
                this.isPlaying = true;
                this.isOpenPopup = false;
                this.activePlayer = 0;
                this.scorePlayer = [0, 0];
                this.currentScore = 0;
                this.dices = [1, 1]
            },

            handleNewGame(){
                //hien thi pop-up
                this.isOpenPopup = true
            },

            nextPlayer(){
                this.activePlayer = this.activePlayer === 0 ? 1 : 0;
                this.currentScore = 0;
            },

            handleRollDice(){
                if (this.isPlaying){
                    var dice1 = Math.floor(Math.random() * 6) + 1;
                    var dice2 = Math.floor(Math.random() * 6) + 1;

                    this.dices = [dice1, dice2];

                    if (dice1 === 1 || dice2 === 1){
                        //doi luot
                        let activePlayer = this.activePlayer
                        setTimeout(function () {
                            alert(`Player ${activePlayer + 1} roll the dice 1. Sorry !`)
                        },10)

                        this.nextPlayer();
                    } else{
                        //cong don diem cho nguoi choi
                        this.currentScore = this.currentScore + dice1 + dice2;
                    }
                } else{
                    alert('Press New Game')
                }
            },

            handleHoldScore(){
                if (this.isPlaying) {
                    let {scorePlayer, activePlayer, currentScore} = this;

                    let scoreOld = scorePlayer[activePlayer];
                    this.$set(this.scorePlayer, activePlayer, scoreOld + currentScore);

                    if (! this.isWinner){
                        this.nextPlayer();
                    }

                } else{
                    alert('Press New Game')
                }
            },

            handleChangeScore(e){
                var number = parseInt(e.target.value)
                if (isNaN(number)){
                    this.finalScore = ''
                }else {
                    this.finalScore = number
                }
            }
        },

        computed:{
            isWinner(){
                let {scorePlayer, finalScore} = this;
                if (scorePlayer[0] >= finalScore || scorePlayer[1] >= finalScore){
                    //Dung cuoc choi
                    this.isPlaying = false;
                    return true;
                }
                return false;
            }
        }
    }
</script>

<style>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;

    }

    .clearfix::after {
        content: "";
        display: table;
        clear: both;
    }

    body {
        background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url('/public/assets/back.jpg');
        background-size: cover;
        background-position: center;
        font-family: Lato;
        font-weight: 300;
        position: relative;
        height: 100vh;
        color: #555;
    }

    .wrapper {
        width: 1000px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: #fff;
        box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
        overflow: hidden;
    }
</style>
