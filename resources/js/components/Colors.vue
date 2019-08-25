<template>
    <div class="container" v-bind:class="containerClass">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-body">
                        <div class="colors">
                            <div v-for="color in colors" class='color' v-bind:class="color.colorName"
                                 v-bind:data-color="color.colorName"
                                    v-on:click="toggleColor">
                                <span>{{color.index}}</span>
                            </div>
                        </div>
                        <div class="submit-answer">
                            <button v-on:click="checkAnswer">
                                check answer
                            </button>
                        </div>
                        <div class="clear">
                            <button v-on:click="clear">
                                clear answer
                            </button>
                        </div>
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
        background-color: rgba(230, 0, 0, 0.5);
    }

    .container.right {
        background-color: rgba(0, 255, 0, 1);
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

    input {
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

    .colors {
        display: flex;
        flex-direction: row;
    }

    .color {
        min-width: 100px;
        min-height: 100px;
        margin: 0 20px;
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
    }

    .answers {
        display: flex;
        flex-direction: row;

        input {
            min-width: 90px;
            max-width: 88px;
            min-height: 20px;
            margin-left: 20px;
            position: relative;
            color: black;
            border: 4px solid black;
            margin-right: 20px;
        }
    }

    .red {
        background-color: rgba(255, 0, 0, 1);
    }

    .blue {
        background-color: rgba(0, 0, 255, 1);
    }

    .green {
        background-color: rgba(0, 255, 0, 1);
    }

    .black {
        background-color: rgba(0, 0, 0, 1);
    }
</style>
<script>
  export default {
    data() {
      return {
        realAnswer: 0,
        countDownDate: null,
        containerClass: null,
        time: null,
        gotAnswerRight: false,
        numberNeeded: 8,
        redNumber: "",
        blueNumber: "",
        greenNumber: "",
        blackNumber: "",
        redAnswer: 1,
        blueAnswer: 2,
        greenAnswer: 3,
        blackAnswer: 4,
        currentIndex: 1,
        colors: [
          {'colorName': 'red', index: 0},
          {'colorName': 'blue', index: 0},
          {'colorName': 'green', index: 0},
          {'colorName': 'black', index: 0},
        ]
      }
    },
    methods: {
        clear() {
          for (let i = 0; i < this.colors.length; i++) {
            this.colors[i].index = 0;
          }
          this.currentIndex = 1;
        },
        toggleColor(event){
          let colorName = event.target.getAttribute('data-color');
          for (let i = 0; i < this.colors.length; i++) {
            if(this.colors[i].colorName === colorName){
              this.colors[i].index = this.colors[i].index + 1;
            }
          }
        },
        checkAnswer()
        {
          if (this.colors[0].index == this.redAnswer &&
              this.colors[1].index == this.blueAnswer &&
              this.colors[2].index == this.greenAnswer &&
              this.colors[3].index == this.blackAnswer) {
            this.containerClass = "right";
            clearInterval(this.time);
            this.gotAnswerRight = true;
            this.$emit('gotAnswer', {
              'name': 'colors',
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
