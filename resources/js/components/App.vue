<template>
    <div class="app">
        <div class="app-content" v-show="!hideCompPuzzels">
            <ol>
                <li>Puzzel 1
                    <button v-on:click="triggerBinary">Try</button>
                </li>
                <li>Puzzel 2
                    <button v-on:click="triggerFibonachi">Try</button>
                </li>
                <li>Puzzel 3
                    <button v-on:click="triggerColors">Try</button>
                </li>
                <li>Puzzel 4
                    <button v-on:click="triggerMors">Try</button>
                </li>
            </ol>
            <p>
                1.
                <span v-for="game in gameMatch">
                <span v-if="game.answer === null">X</span>
                <span v-else>{{game.answer}}</span>
            </span>
            </p>
            <p v-show="gotAllAnswers">
                All the other puzzles are in the room
            </p>
        </div>
        <section v-show="showBinary">

            <matrix-background></matrix-background>
            <div class="flex-center position-ref full-height">
                <div class="content">
                    <div class="title m-b-md">
                        <example-component
                            v-on:gotAnswer="gotAnswer"
                        ></example-component>
                    </div>
                </div>
            </div>
            <button v-on:click="triggerBinary">Back</button>
        </section>
        <section v-show="showFibonachi" style="background-color: rgba(0,0,0,1);">
            <button v-on:click="triggerFibonachi">Back</button>
            <div class="flex-center position-ref full-height">
                <div class="content">
                    <div class="title m-b-md">
                        <fibonachi
                            v-on:gotAnswer="gotAnswer"
                        ></fibonachi>
                    </div>
                </div>
            </div>
        </section>
        <section v-show="showColors" style="background-color: rgba(255,255,255,1);">
            <button v-on:click="triggerColors">Back</button>
            <div class="flex-center position-ref full-height">
                <div class="content">
                    <div class="title m-b-md">
                        <colors
                            v-on:gotAnswer="gotAnswer"
                        ></colors>
                    </div>
                </div>
            </div>
        </section>
        <section v-show="showMors" style="background-color: rgba(255,255,255,1);">
            <button v-on:click="triggerMors">Back</button>
            <div class="flex-center position-ref full-height">
                <div class="content">
                    <div class="title m-b-md">
                        <mors
                            v-on:gotAnswer="gotAnswer"
                        ></mors>
                    </div>
                </div>
            </div>
        </section>
        <div class="other-answers" v-show="hideCompPuzzels">
            <p>Fill here your codes in you found in the room</p>
            <p>
                <input type="text" v-model="answerTyped">
                <button v-on:click="checkAnswers">Check</button>
            </p>
            <p>
                <span class="gotten-answer" v-if="foundIndex != null && foundAnswer != null">{{foundIndex}}. {{foundAnswer}}</span>
            </p>
        </div>
    </div>
</template>
<style>
    body {
        background-color: #d2d2d2;
        font-size: 25px;
    }

    .app {
        width: 100vw;
        height: 100vh;
        overflow: hidden;
    }

    .app-content {
        display: flex;
        flex-direction: column;
        position: fixed;
        justify-content: center;
        align-items: center;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        z-index: 30;
    }

    ol li {
        display: flex;
        justify-content: space-between;
        min-width: 200px;
    }

    button {
        padding: 8px;
        text-transform: uppercase;
    }

    section {
        display: flex;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        justify-content: center;
        align-items: center;
        z-index: 50;
    }

    section > button {
        z-index: 60;
        background-color: white;
        position: fixed;
        top: 0;
        left: 0;
        border: none;
    }
    .other-answers {
        display: flex;
        justify-content: center;
        align-items: center;
        align-content: center;
        min-height: 100vh;
        flex-direction: column;
    }
</style>
<script>
  export default {
    name: 'app',
    data() {
      return {
        showFibonachi: false,
        showBinary: false,
        showColors: false,
        showMors: false,
        gameMatch: [
          {'name': 'binary', 'answer': null},
          {'name': 'fibonachi', 'answer': null},
          {'name': 'colors', 'answer': null},
          {'name': 'mors', 'answer': null},
        ],
        otherAnswers: [
          {'index': 2, 'answer': 8574, 'givenCode': 7365},
          {'index': 3, 'answer': 1793, 'givenCode': 9264},
          {'index': 4, 'answer': 4265, 'givenCode': 6045}
        ],
        foundIndex: null,
        foundAnswer: null,
        gotAllAnswers: false,
        hideCompPuzzels: false,
        answerTyped: "",
      }
    },
    methods: {
      checkAnswers() {
        let self = this;
         this.otherAnswers.forEach(function(answer) {
            if(self.answerTyped == answer.givenCode) {
              self.foundIndex = answer.index;
              self.foundAnswer = answer.answer;
              setTimeout(function() {
                self.foundIndex = null;
                self.foundAnswer = null;
              }, 30000);
            }
        });
      },
      triggerMors() {
        this.showMors = !this.showMors;
      },
      triggerColors() {
        this.showColors = !this.showColors;
      },
      triggerFibonachi() {
        this.showFibonachi = !this.showFibonachi;
      },
      triggerBinary() {
        this.showBinary = !this.showBinary;
      },
      gotAnswer(data) {
        this.gameMatch.forEach(function (game) {
          if (game.name === data.name) {
            game.answer = data.answer;
          }
        })
        var found = false;
        for (var i = 0; i < this.gameMatch.length; i++) {
          if (this.gameMatch[i].answer === null) {
            found = true;
            break;
          }
        }
        if(found) {
            console.log("found a false");
        }else {
          this.gotAllAnswers = true;
          let self = this;
          setTimeout(function() {
            self.hideCompPuzzels = true;
          }, 30000);
        }
      }
    }
  }
</script>
