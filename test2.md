<script>
window.onload = () => {
  const video  = document.querySelector("#camera");
  /** カメラ設定 */
  const constraints = {
    audio: false,
    video: {
      width: 300,
      height: 200,
      facingMode: { exact: "environment" }
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
};
</script>

<html>
  <body>
    <video id="camera" width="300" height="200"></video>
  </body>
</html>
