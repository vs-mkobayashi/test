<video id="video" autoplay playsinline></video>
<script>
  const stream = await navigator.mediaDevices.getUserMedia({ audio: false, video: true });
  const video = document.querySelector('video');
  video.srcObject = stream;
</script>
