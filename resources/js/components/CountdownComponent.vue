<template>
    <div class="container" v-bind:class="containerClass">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-body">
                        <!-- Display the countdown timer in an element -->
                        <span>Time left:</span><span id="demo"></span>
                        <br>
                        <br>
                        <br>
                        <span id="lost" v-show="showYouLostText">You lost</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style>
    .container {
        background-color: rgba(0,0,0,1);
        color: white;
        padding: 20px;
        min-height: calc( 100vh - 40px );
        display: flex;
        justify-content: center;
        align-content: center;
        align-items: center;
        font-size: 32px;
        max-height: unset !important;
    }
    .container.wrong {
        background-color: rgba(255,0,0,1);
    }
    .container.right {
        background-color: rgba(0,255,0,1);
    }
    #lost {
        font-size: 60px;
        text-transform: uppercase;
    }
</style>
<script>
    export default {
        data() {
            return {
                countDownDate: null,
                containerClass: null,
                time: null,
                showYouLostText: false,
            }
        },
        mounted() {
            this.reset();
        },
        methods: {
            reset() {
                this.timer();
            },
            timer() {
                let self = this;
                // Set the date we're counting down to
                this.countDownDate = new Date(new Date().getTime() + ( 60000  * 8 ) ).getTime();
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
                        var audio = new Audio('sounds/Explosion.mp3');
                        audio.play();
                        self.containerClass = 'wrong';
                        self.showYouLostText = true;
                    }
                }, 1000);
            },
        }
    }
</script>
