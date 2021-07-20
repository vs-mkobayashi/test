<script>
    "use strict";
    // 映像をストリーミングする
    const mediaStreamConstraints = {
        video: true,
        audio: false
    };

    // 動画で再生されるlocalStream。
    let localStream;

    // video要素にMediaStreamを追加する処理。
    function gotLocalMediaStream(mediaStream) {
	console.log(document.querySelector("video"));
        document.getElementById('local_video').srcObject = mediaStream;
	window.alert("button 1　OK");
    }

    // エラー処理
    function handleLocalMediaStreamError(error) {
        console.log("navigator.getUserMedia error: ", error);
	window.alert("navigator.getUserMedia error: ", error);
    }

    function buttonClick(){
        navigator.mediaDevices
  　　　　　　　　　　　　　　　　.getUserMedia(mediaStreamConstraints)
  　　　　　　　　　　　　　　　　.then(gotLocalMediaStream)
  　　　　　　　　　　　　　　　　.catch(handleLocalMediaStreamError);
   }
    function button2Click(){
        navigator.getUserMedia({video: true, audio: false},
	    function(stream) {
	        window.alert("button2 worked");
                var video = document.querySelector('video');
                video.srcObject = stream;
                video.onloadedmetadata = function(e) {
                    video.play();
                };
            },
	    function(err) {
	        window.alert("button2 not worked");
                window.alert("The following error occurred: " + err.name);
            }
       );
   }
</script>

<h> test </h>
<input type="button" value="button" onclick="buttonClick()">
<input type="button" value="button2" onclick="button2Click()">
<video id="local_video" autoplay playsinline>
    <source>
</video>
