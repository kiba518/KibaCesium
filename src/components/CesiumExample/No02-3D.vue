<template>
  <div class="map-box">
    <div id="cesiumContainer"></div>
  </div>
</template>

<script>
export default {
  name: "",
  mounted() {
    this.show3DTile();
  }
  ,
  methods: {
    show3DTile() {
      var url = 'http://192.168.19.112:9003/model/tEn5ey1J3/tileset.json';
      // eslint-disable-next-line no-unused-vars
      const viewer = new Cesium.Viewer("cesiumContainer", {
        terrainProvider: Cesium.createWorldTerrain(),
      });
      window.viewer = viewer;
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
      window.viewer.flyTo(tileset)
      viewer.camera.setView({
        destination: Cesium.Cartesian3.fromDegrees(-117.16, 32.71, 15000.0)
      });

       
    },
    // set(tileset) { 
    //   var item = new Object();
    //   item.longitudeOffset = 0;
    //   item.latitudeOffset = 0;
    //   item.heightOffset = 0;
    //   console.log("变量定义完成")
    //   tileset.readyPromise.then(function (palaceTileset) {
    //     var longitude = item.longitudeOffset //模型需要改变的经度
    //     var latitude = item.latitudeOffset //模型需要改变的纬度
    //     //获取3Dtlies的bounds范围
    //     var boundingSphere = palaceTileset.boundingSphere;
    //     console.log(boundingSphere, "获取3Dtlies的bounds范围")
    //     //获取3Dtlies的范围中心点的弧度
    //     var cartographic = Cesium.Cartographic.fromCartesian(boundingSphere.center);
    //     //定义3Dtlies改变之后中心点的弧度
    //     var offsetvalue = Cesium.Cartographic.fromDegrees(longitude, latitude, item.heightOffset)
    //     // debugger
    //     //模型本身的位置
    //     var surface = Cesium.Cartesian3.fromRadians(cartographic.longitude, cartographic.latitude, cartographic.height);
    //     //模型改变的位置
    //     var offset = Cesium.Cartesian3.fromRadians(offsetvalue.longitude, offsetvalue.latitude, item.heightOffset);
    //     //定义模型的改变状态
    //     var translation = Cesium.Cartesian3.subtract(offset, surface, new Cesium.Cartesian3());
    //     //修改模型的位置
    //     palaceTileset.modelMatrix = Cesium.Matrix4.fromTranslation(translation);

    //     // center: Cartesian3
    //     // x: 6378137.000000002
    //     // y: 1.853533215800951e-9
    //     // z: 1.1798190202583214e-10

    //     // radius: 208.27809838022412

    //     var headingAngle = 7.1077496389876024807;
    //     var pitchAngle = 190;
    //     var rollAngle = 208.27809838022412;
    //     var lon = 6378137.000000002;
    //     var lat = 1.853533215800951e-9;
    //     var height = 1.1798190202583214e-10;
    //     var isSet = true;
    //     if (isSet) {
    //       console.log("=================")
    //       var homeCameraView = {
    //         // 初始化相机经纬度
    //         destination: Cesium.Cartesian3.fromDegrees(lon, lat, height),
    //         orientation: {
    //           heading: Cesium.Math.toRadians(headingAngle),
    //           pitch: Cesium.Math.toRadians(pitchAngle), //从上往下看为-90
    //           roll: Cesium.Math.toRadians(rollAngle)
    //         }
    //       }
    //       window.viewer.scene.camera.setView(homeCameraView);

    //       // CameraView.duration = 2.0; homeCameraView.maximumHeight = 2000;
    //       // homeCameraView.pitchAdjustHeight = 2000;
    //       // homeCameraView.endTransform = Cesium.Matrix4.IDENTITY; // Override the default home button
    //       // window.viewer.homeButton.viewModel.command.beforeExecute.addEventListener(function (e) { e.cancel = true; viewer.scene.camera.flyTo(homeCameraView); });

    //     } else {
    //       console.log("=====1============")
    //       window.viewer.flyTo(tileset)
    //     }


    //   })
    // }
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
