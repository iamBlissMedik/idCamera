<script setup>
import { ref, onMounted } from "vue";
const videoCamera = ref(null);
const takePhoto = ref(true);
const canvas = ref(null);
const cameraSnap = async () => {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({
      video: true,
    });
    videoCamera.value.srcObject = stream;
  } catch (err) {
    console.log(err);
  }
};
const capturePhoto = () => {
  canvas.value.getContext("2d").drawImage(videoCamera.value, 0, 0, 320, 337.5);
  takePhoto.value = !takePhoto.value;
  let tracks = videoCamera.value.srcObject.getTracks();
  tracks.forEach((track) => {
    track.stop();
  });
  console.log(tracks);
};

const retakePhoto = () => {
  takePhoto.value = true;
  cameraSnap();
};
onMounted(() => {
  cameraSnap();
});
</script>
<template>
  <div class="cont shadow-2xl">
    <!-- camera -->
    <div class="content grid gap-3" v-if="takePhoto">
      <h1 class="text-2xl font-extrabold font-mono">FACE VERIFICATION</h1>
      <p class="font-medium text-center">
        We will use this to match the picture in your id for verification
        purpose.
      </p>
      <!-- camera -->
      <div id="content">
        <div class="overlay">
          <div class="overlay-helper">
            <div class="overlay-element top-left"></div>
            <div class="overlay-element top-right"></div>
            <div class="overlay-element bottom-left"></div>
            <div class="overlay-element bottom-right"></div>
          </div>
        </div>
      </div>
      <div class="camera-cont">
        <video
          ref="videoCamera"
          class="camera"
          :width="220"
          :height="357.5"
          autoplay
        ></video>
      </div>
      <!-- instruction -->
      <div class="instruction">
        <!-- icon -->
        <p></p>
        <p>Place your head straight and look at the camera</p>
      </div>
      <!-- button -->
      <div class="mt-6">
        <button
          class="bg-purple-800 hover:bg-purple-600 active:bg-violet-700 focus:outline-none focus:ring focus:ring-violet-300 rounded px-7 py-2 text-white"
          @click="capturePhoto"
        >
          Capture
        </button>
      </div>
    </div>
    <!-- canvas -->
    <div class="h-fit" v-show="!takePhoto">
      <!-- image -->
      <div class=" w-fit h-fit">
        <canvas width="320" height="337.5" ref="canvas"> </canvas>
      </div>

      <!-- button -->
      <div class="mt-6">
        <button
          class="bg-purple-800 hover:bg-purple-600 active:bg-violet-700 focus:outline-none focus:ring focus:ring-violet-300 rounded px-7 py-2 text-white"
          @click="retakePhoto"
        >
          RETAKE
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cont {
  padding: 2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 10px;
  width: 500px;
}

.content {
  padding: 0 10px 10px 10px;
  text-align: center;
  position: relative;
}

.camera-cont {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 165px;

}

.camera {
  border-radius: 10px;
}
.overlay {
  --border-style: 8px solid purple;

  position: absolute;
  width: 420px; /* same size as the image */
  height: 220px;
}

.overlay-helper {
  position: relative;
  width: 100%; /* make this element same size as the wrapper */
  height: 100%;
}
.overlay-element {
  position: absolute;
  width: 57px; /* controls the length of the borders */
  height: 50px; /* controls the height of the borders */
}

.overlay .top-left {
  border-left: var(--border-style);
  border-top-left-radius: 12px;
  border-top: var(--border-style);
  top: 6px;
  left: 92px;
}

.overlay .top-right {
  border-right: var(--border-style);
  border-top: var(--border-style);
  border-top-right-radius: 12px;
  top:6px;
  right: 98px;
}

.overlay .bottom-left {
  border-left: var(--border-style);
  border-bottom: var(--border-style);
  border-bottom-left-radius: 12px;
  bottom: 38px;
  left: 91px;
}

.overlay .bottom-right {
  border-right: var(--border-style);
  border-bottom: var(--border-style);
  border-bottom-right-radius: 12px;
  bottom: 38px;
  right: 98px;
}

.instruction {
  display: flex;
  justify-content: center;
}
</style>
