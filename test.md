
<script type="text/javascript" src="https://serratus.github.io/quaggaJS/examples/js/quagga.js"></script>
<script src="https://code.jquery.com/jquery.min.js"></script>
<style type="text/css">
  #photo-area.viewport {
   height: 240px;
   width:320px;
  }
  #photo-area.viewport canvas, video {
    float: left;
    width:320px;
    height: 240px;
  }
  #photo-area.viewport canvas.drawingBuffer, video.drawingBuffer {
    margin-left: -320px;
  }
</style>

<script>
$(function () {

    startScanner();

});

const startScanner = () => {
    Quagga.init({
        inputStream: {
            name: "Live",
            type: "LiveStream",
            target: document.querySelector('#photo-area'),
            constraints: {
                facingMode: "environment"
            },
        },
        decoder: {
            readers: ["ean_reader", "ean_8_reader"]
        },

    }, function (err) {
        if (err) {
            console.log(err);
            return
        }

        console.log("Initialization finished. Ready to start");
        Quagga.start();

        // Set flag to is running
        _scannerIsRunning = true;
    });

    Quagga.onProcessed(function (result) {
        var drawingCtx = Quagga.canvas.ctx.overlay,
            drawingCanvas = Quagga.canvas.dom.overlay;

        if (result) {
            if (result.boxes) {
                drawingCtx.clearRect(0, 0, parseInt(drawingCanvas.getAttribute("width")), parseInt(drawingCanvas.getAttribute("height")));
                result.boxes.filter(function (box) {
                    return box !== result.box;
                }).forEach(function (box) {
                    Quagga.ImageDebug.drawPath(box, {
                        x: 0,
                        y: 1
                    }, drawingCtx, {
                        color: "green",
                        lineWidth: 2
                    });
                });
            }

            if (result.box) {
                Quagga.ImageDebug.drawPath(result.box, {
                    x: 0,
                    y: 1
                }, drawingCtx, {
                    color: "#00F",
                    lineWidth: 2
                });
            }

            if (result.codeResult && result.codeResult.code) {
                Quagga.ImageDebug.drawPath(result.line, {
                    x: 'x',
                    y: 'y'
                }, drawingCtx, {
                    color: 'red',
                    lineWidth: 3
                });
            }
        }
    });

    //barcode read call back
    Quagga.onDetected(function (result) {
        document.getElementById("scaned_code").value = result.codeResult.code;
        console.log(result.codeResult.code);
        
        //this.Quagga.stop();
    });
  }
</script>

<h> barcode reader </h>
<div id="photo-area" class="viewport"></div>

<video id="camera" width="300" height="200"></video>
code: <input id="scaned_code" type="text" value="" readonly>
