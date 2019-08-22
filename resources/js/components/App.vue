<template>
    <div class="app">
        <div class="app-content">
            <ol>
                <li>Binary <button v-on:click="triggerBinary">Try</button></li>
                <li>Fibonachi <button v-on:click="triggerFibonachi">Try</button></li>
                <li>Colors <button v-on:click="triggerColors">Try</button></li>
                <li>Mors <button v-on:click="triggerMors">Try</button></li>
            </ol>
            <p>
                The final computer code:
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
                <div class="content" >
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
                <div class="content" >
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
                <div class="content" >
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
                <div class="content" >
                    <div class="title m-b-md">
                        <mors
                            v-on:gotAnswer="gotAnswer"
                        ></mors>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>
<style>
    body{
        background-color:#d2d2d2;
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
        justify-content:space-between;
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
    section > button{
        z-index: 60;
        background-color: white;
        position: fixed;
        top: 0;
        left: 0;
        border: none;
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
            { 'name': 'binary', 'answer' : null},
            { 'name': 'fibonachi', 'answer' : null},
            { 'name': 'colors', 'answer' : null},
            { 'name': 'mors', 'answer' : null},
          ]
        }
      },
      computed: {
        gotAllAnswers: function () {
          return true
        }
      },
      methods: {
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
            this.gameMatch.forEach(function(game) {
              console.log(game.name, data.name, game.name === data.name);
              if(game.name === data.name) {
                game.answer = data.answer;
              }
            })
        }
      }
    }
</script>
