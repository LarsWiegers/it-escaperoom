<template>
    <div class="container" v-bind:class="containerClass">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-body">
                        <!-- Display the countdown timer in an element -->
                        <span>Time left:</span><span id="demo"></span>

                        <p id="binary">{{binary}}</p>
                        <p>
                            <label>
                                Your answer:
                                <input type="numeric" v-model="binaryAnswer">
                                <button class="button" v-on:click="checkAnswer">Check</button>
                            </label>
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style scoped>
    .container {
        background-color: rgba(0,0,0,1);
        color: white;
        padding: 20px;
        max-height: 40vh !important;
        min-height: 40vh !important;
    }
    .container.wrong {
        background-color: rgba(255,0,0,1);
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
        border-bottom: 2px solid white;
        font-size: 20px;
        color: white;
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
                binaryAnswer: '',
                binary: 0,
                realAnswer: 0,
                countDownDate: null,
                containerClass: null,
                time: null,
                gotAnswerRight: false,
                numberNeeded: 4
            }
        },
        mounted() {
            this.reset();
        },
        methods: {
            reset() {
                this.binary = this.gererateRandomBinary();
                this.timer();
            },
            checkAnswer() {
                if(this.binaryAnswer == this.realAnswer) {
                    this.containerClass = "right";
                    clearInterval(this.time);
                    this.gotAnswerRight = true;
                    this.$emit('gotAnswer', {
                      'name': 'binary',
                      'answer': this.numberNeeded,
                    });
                }else {
                    this.containerClass = "wrong";
                    this.reset();
                }
                let self = this;
                setInterval(function() {
                    self.containerClass = "";
                }, 1000);
            },
            timer() {
                let self = this;
                // Set the date we're counting down to
                this.countDownDate = new Date(new Date().getTime() + 60000).getTime();
                // Update the count down every 1 second
                this.time = setInterval(function() {
                    // Get todays date and time
                    let now = new Date().getTime();
                    // Find the distance between now and the count down date
                    let distance = self.countDownDate - now;
                    // Time calculations for days, hours, minutes and seconds
                    let days = Math.floor(distance / (1000 * 60 * 60 * 24));
                    let hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                    let minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
                    let seconds = Math.floor((distance % (1000 * 60)) / 1000);
                    // Display the result in the element with id="demo"
                    document.getElementById("demo").innerHTML = days + "d " + hours + "h "+ minutes + "m " + seconds + "s ";
                    // If the count down is finished, write some text
                    if (distance < 1000) {
                        clearInterval(self.time);
                        self.checkAnswer();
                    }
                }, 1000);
            },
            gererateRandomBinary() {
                var chars = "01";
                var string_length = 6;
                var randomstring = '';
                for (var i=0; i<string_length; i++) {
                    var rnum = Math.floor(Math.random() * chars.length);
                    randomstring += chars.substring(rnum,rnum+1);
                }
                this.realAnswer = parseInt(randomstring, 2);
                return randomstring;
            }
        }
    }
</script>
