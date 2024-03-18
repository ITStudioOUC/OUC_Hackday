<template>
  <div class="common-layout">
      <el-container>
        <el-main>
          <div id="content" class="bg_blue" style="width: 800px;height: 600px;margin-left: auto;margin-right: auto">
          </div>
        </el-main>
        <el-footer>
          <WaveViewer
              wavePosition="bottom"
              :waveLayerCount="4"
              :space="100"
              :waveGap="200"
              :waveHeight="50">
            <div>
            </div>
          </WaveViewer>
        </el-footer>
      </el-container>
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
      document.getElementById('content').style["background-color"] = "#ffa100";
    }
    function end () {
      //end callback
      console.log('end');
    }
    const _this = this
    this.pixelwave = new PixelWave({
      canvasTop: 0, // for a fixed header
      delayMiddle: 1,
      speedIn: 0.9,
      speedOut: 0.9,
    });
    document.getElementById('content').addEventListener('click', function() {
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
.bg_yellow {
  background-color: #ffa100;
}
.bg_blue {
  background-color: #001f7a;
}
.el-footer {
  --el-footer-padding: 0px
}
.el-main {
  --el-main-padding: 0px
}

</style>

