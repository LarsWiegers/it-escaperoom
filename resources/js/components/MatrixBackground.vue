<template>
    <div>
        <canvas id="canvas"></canvas>
        <img id="logo" width="400px;" src="https://s3.amazonaws.com/freecodecamp/freecodecamp_logo.svg" height="200px"/>
    </div>
</template>
<style>
    body{
        background-color:#d2d2d2;
    }
    #canvas{
        background-color:#000;
        display:block;
        margin:auto;
        width: 100%;
        height: 100%;
        position: fixed;
        top: 0;
        right: 0;
        left: 0;
        bottom: 0;
    }
    #logo{
        display:none;
    }
</style>
<script>
    export default {
        name: 'matrix-background',
        mounted() {
            let canvas = document.getElementById('canvas');
            let ctx = canvas.getContext('2d');
            let t = text();
            let logo = document.getElementById('logo');
            let lines = [];
            window.setInterval(draw, 100);

            function draw() {
                if (Math.floor(Math.random() * 2) === 0 && lines.length < 100) {
                    lines.push(new textLine());
                }
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                lines.forEach(function(tl) {

                    ctx.drawImage(tl.text, tl.posX, tl.animate(), 20, 1000);
                });
                ctx.drawImage(logo, 100, 155, 400, 70);

            }

            function textLine() {
                this.text = t;
                this.posX = (function() {
                    return Math.floor(Math.random() * canvas.width);
                })();
                this.offsetY = -1000;
                this.animate = function() {
                    if (this.offsetY >= 0) {
                        this.offsetY = -1000;
                    }
                    this.offsetY += 10;
                    return this.offsetY;
                };
            }

            function text() {
                let offscreenCanvas = document.createElement('canvas');
                offscreenCanvas.width = "30";
                offscreenCanvas.height = "1000";
                offscreenCanvas.style.display = "none";
                document.body.appendChild(offscreenCanvas);
                let octx = offscreenCanvas.getContext('2d');
                octx.textAlign = "center";
                octx.shadowColor = "lightgreen";
                octx.shadowOffsetX = 2;
                octx.shadowOffsetY = -5;
                octx.shadowBlur = 1;
                octx.fillStyle = 'darkgreen';
                octx.textAlign = "left";
                let step = 10;
                for (let i = 0; i < 100; i++) {
                    let charCode = 0;
                    while (charCode < 60) {
                        charCode = Math.floor(Math.random() * 100);
                    }
                    octx.fillText(String.fromCharCode(charCode), 0, step);
                    step += 10;
                }
                return offscreenCanvas;
            }
        }
    }
</script>
