<script>
	function buttonClick(){
    var promise = navigator.mediaDevices.getUserMedia({ audio: false, video: true });
	        window.alert("button 1");
		console.log(promise);
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
      });
   }
</script>

<h> test </h>
<input type="button" value="button" onclick="buttonClick()">
<input type="button" value="button2" onclick="button2Click()">
