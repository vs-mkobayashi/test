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

window.onload = () => {
  const video  = document.querySelector("#camera");
  const canvas = document.querySelector("#picture");

  /** カメラ設定 */
  const constraints = {
    audio: false,
    video: {
      width: 300,
      height: 200,
      facingMode: "user"   // PC で確認用にフロントカメラを利用する
      // facingMode: { exact: "environment" }  // リアカメラを利用する場合
    }
  };

  /**
   * カメラを<video>と同期
   */
  navigator.mediaDevices.getUserMedia(constraints)
  .then( (stream) => {
    video.srcObject = stream;
    video.onloadedmetadata = (e) => {
      video.play();
    };
  })
  .catch( (err) => {
    console.log(err.name + ": " + err.message);
  });

  /**
   * シャッターボタン
   */
   document.querySelector("#shutter").addEventListener("click", () => {
    const ctx = canvas.getContext("2d");

    // 演出的な目的で一度映像を止める
    video.pause();  // 映像を停止
    setTimeout( () => {
      video.play();    // 0.5秒後にカメラ再開
    }, 500);

    // canvasに画像を貼り付ける
    ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
    // save
    var jpeg = canvas.toDataURL("image/jpeg")       // JPEG
    console.log(jpeg);
    var download = $("<a></a>").appendTo("body").css("display","none");
    download.prop({"href" : jpeg, "download": "canvas.jpg" });
    console.log(download);
    download.get(0).click();
    download.remove();
  });

　　　　$("#save").on( "click", function(){
　　　　　　　　var jpeg = canvas.toDataURL("image/jpeg")       // JPEG
    console.log(jpeg);
　　　　　　　　var download = $("<a></a>").appendTo("body").css("display","none");
　　　　　　　　download.prop({"href" : jpeg, "download": "canvas.jpg" });
    console.log(download);
　　　　　　　　download.get(0).click();
　　　　　　　　download.remove();
  }
};
</script>

<h> test </h>
<input type="button" value="button" onclick="buttonClick()">
<input type="button" value="button2" onclick="button2Click()">
<input type="file" accept="image/*;capture=camera">
<video id="local_video" autoplay playsinline>
    <source>
</video>

<video id="camera" width="300" height="200"></video>
<canvas id="picture" width="300" height="200"></canvas>
<form>
  <button type="button" id="shutter">シャッター</button>
</form>
<input id="save" type="button" value="save">
