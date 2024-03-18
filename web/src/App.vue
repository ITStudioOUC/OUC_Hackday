<template>
  <WaveViewer
      wavePosition="bottom"
      :waveLayerCount="4"
      :space="100"
      :waveGap="200"
      :waveHeight="50">
    <div>
    </div>
  </WaveViewer>
  <div>
<!--    <img id="endImage" src="@/assets/imgs/foreground.jpg">-->
    <img id="startImage" src="@/assets/imgs/background.jpg">
  </div>


</template>

<script>
import WaveViewer from "@/components/Wave.vue";
import {PixelWave} from "@/assets/js/PixelWave"

export default {
  name: 'App',
  components: {
    WaveViewer,
  },
  data(){
    return {
      pixelwave: null
    }
  },
  mounted() {
    function start () {
      //preload next image
      console.log('start');
    }

    function middle () {
      //exchange content
      console.log('middle');
      document.getElementById('startImage').style.display = "none";
    }
    function end () {
      //end callback
      console.log('end');
    }
    const _this = this
    this.pixelwave = new PixelWave({
      canvasTop: 500, // for a fixed header
    });
    document.getElementById('startImage').addEventListener('click', function() {
      _this.pixelwave.start(start, middle, end);
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
