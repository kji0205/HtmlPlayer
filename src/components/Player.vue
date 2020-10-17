<template>
  <div>
    <video
      controls=""
      muted
      autoplay=""
      src="http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4"
      type="audio/x-mpegurl"
      id="player"
    ></video>



    <div class="footer-layer">
      <progress id="progressbar" value="30" max="100"></progress>
      <div class="progress-bar">
        <div class="indicator">O</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Player',
  mounted() {

    let dragItem = document.querySelector(".indicator");
    let container = document.querySelector(".progress-bar");

    let player = document.querySelector('#player');

    player.controls = false;
    player.style.width = "70%";
    
    // let progressBar = document.querySelector("indicator");

    container.addEventListener("touchstart", dragStart, false);
    container.addEventListener("touchend", dragEnd, false);
    container.addEventListener("touchmove", drag, false);

    container.addEventListener("mousedown", dragStart, false);
    container.addEventListener("mouseup", dragEnd, false);
    container.addEventListener("mousemove", drag, false);

    let active = false;
    let currentX;
    let currentY;
    let initialX;
    let initialY;
    let xOffset = 0;
    let yOffset = 0;

    function dragStart(e) {
      if (e.type === "touchstart") {
        initialX = e.touches[0].clientX - xOffset;
        initialY = e.touches[0].clientY - yOffset;
      } else {
        initialX = e.clientX - xOffset;
        initialY = e.clientY - yOffset;
      }

      if (e.target === dragItem) {
        active = true;
      }
    }

    function dragEnd() {
      initialX = currentX;
      initialY = currentY;

      active = false;
    }

    function drag(e) {
      if (active) {

        e.preventDefault();

        if (e.type === "touchmove") {
          currentX = e.touches[0].clientX - initialX;
          currentY = e.touches[0].clientY - initialY;
        } else {
          currentX = e.clientX - initialX;
          currentY = e.clientY - initialY;
        }

        xOffset = currentX;
        yOffset = currentY;

        setTranslate(currentX, currentY, dragItem);
      }
    }

    function setTranslate(xPos, yPos, el) {
      // el.style.transform = "translate3d(" + xPos + "px, " + yPos + "px, 0)";
      el.style.transform = "translateX(" + xPos + "px)";
    }

  },
}

</script>

<style scoped>
  #progressbar {
    width: 100%;
    background-color: #2c3e50;
    z-index: 10000;
  }
  #player {
    position: fixed;
    right: 0;
    bottom: 0;
    min-width: 100%;
    min-height: 100%;
  }
  .footer-layer {
    position: fixed;
    bottom: 20%;
    background: rgba(0, 0, 0, 0.5);
    color: #f1f1f1;
    width: 100%;
    /*padding: 20px;*/
  }
  .indicator {
    /*font-size: 30px;*/
    transform: scale(30);
  }
</style>
