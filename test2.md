<video id="video" autoplay playsinline></video>
<script>
  const video = document.getElementById("video")
  navigator.mediaDevices.getUserMedia({
    video: true,
    audio: false,
    facingMode: "environment",
  }).then(stream => {
    video.srcObject = stream;
    video.play()
  }).catch(e => {
    console.log(e)
  })
</script>
