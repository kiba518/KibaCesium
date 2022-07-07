<template>
  <div class="map-box">
    <div id="cesiumContainer"></div>
  </div>
</template>

<script>
export default {
  name: "",
  mounted() {
    // var viewer = new Cesium.CesiumWidget('cesiumContainer')

    // eslint-disable-next-line no-undef
    //var viewer = new Cesium.Viewer("cesiumContainer");

    // eslint-disable-next-line no-console
    // console.log(viewer)
    this.show3DTile();
  }
  ,
  methods: {
    init() {

      let url = "http://192.168.50.28:6080/arcgis/rest/services/SampleWorldCities/MapServer";
      // eslint-disable-next-line no-undef
      let geogle = new Cesium.ArcGisMapServerImageryProvider({ url: url });
      // eslint-disable-next-line no-undef
      this.viewer = new Cesium.Viewer('cesiumContainer', {
        baseLayerPicker: false,         //是否显⽰图层选择控件
        selectionIndicator: false,
        // geocoder: false,                //是否显⽰地名查找控件
        // sceneModePicker: false,         //是否显⽰投影⽅式控件
        // navigationHelpButton: false,    //是否显⽰帮助信息控件
        // homeButton: false,              //是否显⽰Home按钮
        // fullscreenButton: false,        //是否显⽰全屏按钮
        // timeline:false,                 //时间轴控件
        // animation:false,                //动画控件
        imageryProvider: geogle,
        // terrainProvider:new Cesium.createWorldTerrain({
        //   requestVertexNormals:true,
        //   requestWaterMask:true
        // }),
      });
    },
    show3DTile() {
      var url = 'http://192.168.19.112:9003/model/tEn5ey1J3/tileset.json';
      // eslint-disable-next-line no-unused-vars
      const viewer = new Cesium.Viewer("cesiumContainer", {
        terrainProvider: Cesium.createWorldTerrain(),
      });
       // eslint-disable-next-line no-unused-vars
      let tileset = viewer.scene.primitives.add(new Cesium.Cesium3DTileset({
        url: url,
        // modelMatrix: Cesium.Matrix4.fromArray([1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 0, 1]),
        baseScreenSpaceError: 1024,
        //【重要】数值加大，能让最终成像变模糊
        skipScreenSpaceErrorFactor: 16,
        skipLevels: 1,
        immediatelyLoadDesiredLevelOfDetail: false,
        loadSiblings: false,
        cullWithChildrenBounds: true,
        skipLevelOfDetail: true, //开启跳级加载
        //这个参数默认是false，同等条件下，叶子节点会优先加载。但是Cesium的tile加载优先级有很多考虑条件，
        //这个只是其中之一，如果skipLevelOfDetail=false，这个参数几乎无意义。所以要配合skipLevelOfDetail=true来使用，
        //此时设置preferLeaves=true。这样我们就能最快的看见符合当前视觉精度的块，对于提升大数据以及网络环境不好的前提下有一点点改善意义。
        preferLeaves: true,
        //【重要】内存建议显存大小的50%左右，内存分配变小有利于倾斜摄影数据回收，提升性能体验
        maximumMemoryUsage: 1024
        //控制切片视角显示的数量，可调整性能
        // maximumScreenSpaceError: 2,//最大的屏幕空间误差
        // maximumNumberOfLoadedTiles: 100000, //最大加载瓦片个数
      }));
    }
  }
};
</script> 
<style scoped>
.map-box {
  width: 100%;
  height: 100%;
}

#cesiumContainer {
  width: 100%;
  height: 100%;
}
</style>
