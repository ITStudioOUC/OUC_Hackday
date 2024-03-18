<template>
  <div ref="wave" class="wave-div" :style="divStyle">
    <slot></slot>
    <svg ref="wall" class="wave-wall" :id="svgId" :style="wallStyle">
    </svg>
  </div>
</template>

<script>
import Snap from 'snapsvg-cjs';
const mina = window.mina;
export default {
  name: 'WaveViewer',
  props: {
    // The gap bewteen two wave, default 300px.
    waveGap: {
      type: Number,
      default: 150,
    },
    // height of wave
    waveHeight: {
      type: Number,
      default: 40,
    },
    // layer amount of wave. default 3
    waveLayerCount: {
      type: Number,
      default:4,
    },
    // default color
    waveColor: {
      type: Array,
      default: function(){
        return ['#40ccd4','#71d8df','rgba(99,190,255,0.5)','rgb(56,133,255)'];
      },
    },
    waveBackgroundColor: {
      type: String,
      default: '#ccf3fa',
    },
    // position bottom
    wavePosition: {
      type: String,
      default: 'bottom',
    },
    space: {
      type: Number,
      default: 20,
    },
  },
  data () {
    return {
      svgId: `wall_12345`,
      waveLayer: [], // wave Object list.
      waveLayerDatas: [], // wave data list.
      width: 0,
      height: 0,
      snap: null,
      // width / waveGap = waveCount
      waveCount: 0,
      wallStyle: {
        height: 20
      },
      divStyle: {
        // height: 200,
        paddingBottom: 0,
        paddingTop: 0,
        background: this.waveBackgroundColor,
      }
    }
  },
  mounted(){
    // Data init.
    this.width = this.$refs.wave.offsetWidth;
    this.height = this.$refs.wave.offsetHeight;
    this.wallStyle.height = this.waveHeight*2 + 40;
    if (this.wavePosition === 'bottom'){
      this.wallStyle.bottom = 0;
      // this.divStyle.paddingBottom = `${this.space}px`;
    }else{
      this.wallStyle.top = 0;
      // this.divStyle.paddingTop = `${this.space}px`;
    }

    // Snap init.
    this.snap = Snap(`#${this.svgId}`);
    this.waveCount = Math.ceil(this.width / this.waveGap) + 1;

    // Calculate all the wave data.
    this.calWaveData();

    // Set up the wave object.
    for (let i in this.waveLayerDatas){
      this.waveLayer.push(this.snap.path(this.waveLayerDatas[i].items[0]));
      this.animate(i);
      this.waveLayer[i].attr({
        fill: this.waveColor[i],
      });
    }
  },
  methods:{
    // SVG path Generate function.
    waveto(x,y,gap,h,count,direction,offset){
      offset = offset || 0;
      direction = direction || 1;
      count = count || 1;

      let paths = [];
      let oy = y;
      y = Math.random() * y;
      let start = {x,y};
      // bounder warning
      if ((y + direction * h) < 0 || (y - direction*h) < 0){
        y = oy;
      }
      for (let i=0;i<count;i++){
        paths.push(`C${x+gap+offset} ${y + direction * h },${x+gap+10+offset} ${y+ direction * h},${x+2*gap+offset} ${y} S${x+3*gap+offset} ${y - direction*h},${x+4*gap+offset} ${y}`);
        x = x + 4*gap;
      }
      return `M${start.x} ${y} ${paths.join(' ')} A95 95 0 0 1 0 100 Z`;
    },
    // Calculator
    calWaveData(){
      for (let i = 0; i < this.waveLayerCount; i++){
        let waveLayerData = {items:[]};
        for (let k = 0; k < 3; k++){
          // TODO: x, y affect for wave, may be for the width of region
          waveLayerData.items.push(this.waveto(50 + 75 * k,this.waveHeight * 2,this.waveGap
              ,this.waveHeight * Math.random(),
              this.waveCount,k%2==0?-1:1,Math.random()*50));
        }

        this.waveLayerDatas.push(waveLayerData);
      }
    },
    // Animation
    animate(i){
      let that = this;
      this.waveLayer[i].animate({
        d: this.waveLayerDatas[i].items[1]
      },1500 + 500*i,mina.easeinout,()=>{
        this.waveLayer[i].animate({
          d: this.waveLayerDatas[i].items[2]
        },1500 + 500*i,mina.easeinout,() => {
          this.waveLayer[i].animate({
            d: this.waveLayerDatas[i].items[0]
          },1500 + 500*i,mina.easeinout,that.animate(i));
        });
      })
    }
  }
}
</script>

<style lang="scss">
  //.wave-div {
  //  position: relative;
  //  overflow: hidden;
  //  z-index: -2;
  //  height: 150px;
  //  bottom: 0;
  //}
  .wave-wall{
    z-index:-1;
    width:130%;
    height:50%;
    position:absolute;
    left:-30%;
  }

</style>
