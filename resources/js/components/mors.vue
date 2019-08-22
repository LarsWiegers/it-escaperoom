<template>
    <div class="container" v-bind:class="containerClass">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-body">
                        <audio  controls src="/sounds/morse_code.wav">
                        </audio>
                        <input v-model="givenAnswer">
                        <button v-on:click="checkAnswer">
                            check answer
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="scss" scoped>
    .container {
        color: white;
        padding: 20px;
        min-height: 100vh;
        min-width: 100vw;
        background-color: white;
    }
    .container.wrong {
        background-color: rgba(230,0,0,0.5);
    }
    .container.right {
        background-color: rgba(0,255,0,1);
    }
    .button {
        background-color: unset;
        border: 2px solid white;
        border-radius: 5px;
        padding: 5px 10px;
        transition: 250ms ease-in-out;
        color: white;
    }
    label {
        font-size: 16px;
    }
    input{
        border: none;
        background-color: transparent;
        border-bottom: 2px solid black;
        font-size: 20px;
        color: black;
    }
    .button:hover {
        background-color: white;
        color: black;
    }
    p#answer {
        font-size: 20px;
    }
</style>
<script>
    export default {
        data() {
            return {
                countDownDate: null,
                containerClass: null,
                time: null,
                gotAnswerRight: false,
                numberNeeded: 9,
                answer: 7452,
                givenAnswer: "",
            }
        },
        methods: {
            checkAnswer() {
                if(this.givenAnswer == this.answer) {
                    this.containerClass = "right";
                    clearInterval(this.time);
                    this.gotAnswerRight = true;
                  this.$emit('gotAnswer', {
                    'name': 'mors',
                    'answer': this.numberNeeded,
                  });
                } else {
                    this.containerClass = "wrong";
                }
                let self = this;
                setInterval(function () {
                    self.containerClass = "";
                }, 1000);
            }
        }
    }
</script>
