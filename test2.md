<script>
'use strict';
window.onload = () => {
  const stream = await navigator.mediaDevices.getUserMedia({
        audio: false,  // オーディオデバイスは使用しない
        video: true // デフォルトのカメラデバイスを使用する
    });
  const video = document.querySelector('video');
  video.srcObject = stream;
};
</script>

<html>
  <body>
    <video id="video" autoplay playsinline></video>
  </body>
</html>
