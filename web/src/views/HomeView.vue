<template>
  <div>
    <div class="image-canvas-wrapper"  oncontextmenu="return false" unselectable='on' onselectstart='return false;'
      onmousedown='return false;'>
      <!-- DICOM CANVAS -->
        <span id="loadProgress">Diocm加载: </span>
      <div ref="canvas" class="image-canvas" oncontextmenu="return false"></div>
    </div>
  </div>
</template>


<script>
import Hammer from "hammerjs";
import dicomParser from "dicom-parser";
import cornerstone from "cornerstone-core";
import cornerstoneTools from "cornerstone-tools";
import cornerstoneMath from "cornerstone-math";
import cornerstoneWADOImageLoader from "cornerstone-wado-image-loader";


cornerstoneWADOImageLoader.external.cornerstone = cornerstone;
cornerstoneWADOImageLoader.external.dicomParser = dicomParser;
cornerstoneTools.external.cornerstoneMath = cornerstoneMath;
cornerstoneTools.external.cornerstone = cornerstone;
cornerstoneTools.external.Hammer = Hammer;

//引用试例 想要应用更多工具请前往官网查看
const LengthTool = cornerstoneTools.LengthTool; //长度工具
const AngleTool = cornerstoneTools.AngleTool; //角度工具
cornerstoneTools.addTool(LengthTool);
cornerstoneTools.addTool(AngleTool);
//激活工具
cornerstoneTools.setToolActive("LengthTool", { mouseButtonMask: 1 });
//mouseButtonMask：1 代表鼠标左键使用工具



export default {
    name: 'HomeView',
    components: {
        
    },
    setup() {
        //初始化
        cornerstoneTools.init({
            globalToolSyncEnabled: true,
        });

            // Grab Tool Classes
        const WwwcTool = cornerstoneTools.WwwcTool;
        const PanTool = cornerstoneTools.PanTool;
        const PanMultiTouchTool = cornerstoneTools.PanMultiTouchTool;
        const ZoomTool = cornerstoneTools.ZoomTool;
        const ZoomTouchPinchTool = cornerstoneTools.ZoomTouchPinchTool;
        const ZoomMouseWheelTool = cornerstoneTools.ZoomMouseWheelTool;

        // Add them
        cornerstoneTools.addTool(PanTool);
        cornerstoneTools.addTool(ZoomTool);
        cornerstoneTools.addTool(WwwcTool);
        cornerstoneTools.addTool(PanMultiTouchTool);
        cornerstoneTools.addTool(ZoomTouchPinchTool);
        cornerstoneTools.addTool(ZoomMouseWheelTool);

        // Set tool modes
        cornerstoneTools.setToolActive("Pan", { mouseButtonMask: 4 }); // Middle
        cornerstoneTools.setToolActive("Zoom", { mouseButtonMask: 2 }); // Right
        // cornerstoneTools.setToolActive("Wwwc", { mouseButtonMask: 1 }); // Left & Touch
        cornerstoneTools.setToolActive("PanMultiTouch", {});
        cornerstoneTools.setToolActive("ZoomMouseWheel", {});
        cornerstoneTools.setToolActive("ZoomTouchPinch", {});


        // 获取imageId
        const imageId = "wadouri:https://9z04x8ykjr.codesandbox.io/000000.dcm";
        //获取dom节点并渲染数据
        const element = document.getElementById("imageBox");
        cornerstone.enable(element);
        cornerstone.loadAndCacheImage(imageId).then((image) => {
            cornerstone.displayImage(element, image);
          });
    },
    mounted() {
        // Enable Canvas
        this.canvas1 = this.$refs.canvas1;
        this.canvas2 = this.$refs.canvas2;
        cornerstone.enable(this.canvas1, {
          renderer: "webgl"
        });
        cornerstone.enable(this.canvas2, {
          renderer: "webgl"
        });
    }
}
</script>
