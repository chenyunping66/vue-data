<!--
 * @Description: In User Settings Edit
 * @Author: your name
 * @Date: 2019-08-15 16:04:17
 * @LastEditTime: 2019-09-14 21:04:25
 * @LastEditors: Please set LastEditors
 -->
<template>
  <div class="hello">
    <!-- <date-picker :value="now" @input="val=>value=val"></date-picker> -->
    <date-picker v-model="now"></date-picker>

    <!-- <div id="this_map_id" style="width:500px;height:400px">
    </!-->
  </div>
</template>
<script>
// import OpenLayers from '../'
// import OpenLayers from '../assets/OpenLayers'
import DatePicker from './data-picker';
export default {
  components: {
    DatePicker,
  },
  // name: 'HelloWorld',
  data () {
    return {
      now: new Date()
    }
  },
  mounted () {
    // this.initMap(),
    // let map;
    // map = new OpenLayers.Map('this_map_id');
    // var layer = new OpenLayers.Layer.WMS(
    //   "OpenLayers WMS",
    //   "http://vmap0.tiles.osgeo.org/wms/vmap0",
    //   {
    //     layers: 'basic'
    //   });
    // map.addLayer(layer);
    // console.log(map);

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus"></style>
