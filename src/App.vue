<template>
  <div id="app">
    <canvas  id="dashBoard" :style="styleObj"></canvas>
    <div id="preview-textfield"></div>
  </div>
</template>

<script>
  import "./gauge/assets/prettify.js";
  import "./gauge/assets/fd-slider/fd-slider.js";
  import Gauge from 'Gauge'
export default {
  name: 'data-board',
  data () {
    return {
    }
  },
  props:{
    opts:{
      type:Object,
      default:()=>{
        return{
          gradientType: 1,
          lines: 6, // The number of lines to draw
          angle: 0, // The length of each line
          lineWidth: 0.2, // The line thickness
          pointer: {
            length: 0.25, // The radius of the inner circle
            strokeWidth: 0.085, // The rotation offset
            color: '#FA803E' // Fill colorEF9050
          },
          limitMax: 'false',   // If true, the pointer will not go past the end of the gauge
          colorStart: '#F6BB5D',   // Colors
          colorStop: '#FA803E',    // just experiment with them
          strokeColor: '#213272',   // to see which ones work best for you
          generateGradient: true
        }
      }
    },
    maxValue:{
      type: Number,
      required:true,
      default:100
    },
    animationSpeed:{
      type: Number,
      required:false,
      default:32
    },
    actualValue:{
      type: Number,
      required:true,
      default:50
    },
    styleObj:{
      type:Object,
      required:false,
      default:()=>{}
    }
  },
  mounted(){
    console.log(Gauge)
   setTimeout(()=>{
     let target = document.getElementById('dashBoard'); // your canvas element
     let gauge = new Gauge.Gauge(target).setOptions(this.opts); // create sexy gauge!
     gauge.maxValue = this.maxValue; // set max gauge value
     gauge.animationSpeed = this.animationSpeed; // set animation speed (32 is default value)
     gauge.set(this.actualValue); // set actual value
   },0)
  }
}
</script>

<style  scoped>
@import "gauge/assets/main.css";
@import "gauge/assets/prettify.css";
@import "gauge/assets/fd-slider/fd-slider.css";
@import "gauge/assets/fd-slider/fd-slider-tooltip.css";
</style>
