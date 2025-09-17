<template>
  <div class="container">
    <div class="container-head">
      <div>www.alin123.top</div>
      <p>所有文件解压密码均为：<span>alin123</span></p>
      <p>本站为公益站点请Ctrl + D 保存网站www.alin123.top到收藏夹</p>
    </div>

    <div class="container-main">
      <div class="container-main-left">
        <p v-for="(item, index) in dataList" :key="index" @click="scrollToSection(index)">
          {{ item.title }}
        </p>
      </div>
      <div class="container-main-right">
        <div class="list-container">
          <div class="list-item" :id="'list-item' + index" v-for="(item, index) in dataList" :key="index">
            <h3>{{ item.title }}</h3>
            <div class="list-item-content">
              <div class="list-item-content-item" v-for="(list, idx) in item.children" :key="list.title"
                @click="handleClick(list, idx)" :name="list.title" :disabled="list.children.length === 0">
                <el-image class="img" :src="map[list.title]">
                  <template #error>
                    <div class="image-slot">
                      <el-icon>{{ list.title.toUpperCase()[0] }}</el-icon>
                    </div>
                  </template>
                </el-image>
                <p>{{ list.title }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- 连接弹窗 -->
    <el-dialog v-model="dialogVisible" :title="dialogInfo.title" width="980">
      <div class="tips">所有文件解压密码均为:alin123</div>
      <div class="dialog-content" v-for="item in dialogInfo.children" :key="item.title">
        <p class="dialog-content-title">{{ item.title }}</p>
        <p>下载: <a :href="item.url" target="_blank"> {{ item.url }}</a></p>
      </div>
      <template #footer>
        <div class="dialog-footer">
          <el-button type="primary" size="large" @click="dialogVisible = false">
            确定
          </el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'
import sourceData from '@/assets/data.json'
let map = {
  "office": "img/icons/office.svg",
  "wps": "img/icons/wps.svg",
  "Acrobat": "img/icons/Acrobat DC.svg",
  "After Effects": "img/icons/after-effects.svg",
  "Animate": "img/icons/animate.svg",
  "Audition": "img/icons/audition.svg",
  "Bridge": "img/icons/bridge.svg",
  "Character Animator": "img/icons/character-animator.svg",
  "Dimension": "img/icons/dimension.svg",
  "Dreamweaver": "img/icons/dreamweaver.svg",
  "illustrator": "img/icons/illustrator.svg",
  "InCopy": "img/icons/incopy.svg",
  "InDesign": "img/icons/indesign.svg",
  "Lightroom": "img/icons/lightroom-classic.svg",
  "Midea Encoder": "img/icons/media-encoder.svg",
  "Photoshop": "img/icons/photoshop.svg",
  "Photoshop Elements": "img/icons/Photoshop-Elements.svg",
  "Prelude": "img/icons/Prelude.svg",
  "Premiere": "img/icons/premiere.svg",
  "Premiere Elements": "img/icons/PremiereElements.svg",
  "CAD": "img/icons/AutoCAD.svg",
  "CAD Plant 3D": "img/icons/AutoCAD.svg",
  "CAD MEP ": "img/icons/AutoCAD.svg",
  "CAD MAP 3D": "img/icons/AutoCAD.svg",
  "CAD精简版": "img/icons/AutoCAD.svg",
  "CAD建筑版": "img/icons/AutoCAD.svg",
  "CAD机械版": "img/icons/AutoCAD.svg",
  "CAD电气版": "img/icons/AutoCAD.svg",
  "CAD迷你系列": "img/icons/AutoCAD.svg",
  "ACDSee": "img/icons/ACDSee.png",
  "Capture One": "img/icons/Capture One.png",
  "Corel Painter": "img/icons/corel-painter.png",
  "SAI": "img/icons/SAI.png",
  "SketchBook": "img/icons/SketchBook.png",
  "3ds Max": "img/icons/3DSMax.png",
  "Artlantis": "img/icons/Artlantis.png",
  "blender": "img/icons/blender.png",
  "CorelCAD": "img/icons/CorelCAD.png",
  "Lumion": "img/icons/lumion.png",
  "SketchUp Pro": "img/icons/SketchUpPro.png",
  "V-Ray for 3dsMax": "img/icons/V-Ray.svg",
  "V-Ray for Rhino": "img/icons/V-Ray.svg",
  "V-Ray for SketchUp": "img/icons/V-Ray.svg",
  "Maya": "img/icons/MAYA.png",
  "CINEMA 4D": "img/icons/CINEMA 4D.png",
  "Cubase": "img/icons/Cubase.png",
  "达芬奇": "img/icons/DaVinciResolve.png",
  "EDIUS Pro": "img/icons/edius-pro-250.png",
  "Nuendo": "img/icons/nuendo.png",
  "Civil 3D": "img/icons/Civil3D.png",
  "Fuzor": "img/icons/Fuzor.png",
  "Navisworks": "img/icons/Navisworks.png",
  "Revit": "img/icons/Revit.png",
  "Tekla Structures": "img/icons/tekla.png",
  "Vectorworks": "img/icons/Vectorworks.png",
  "CATIA Composer": "img/icons/Catia-P3-V5-6R2020.png",
  "Catia": "img/icons/Catia-P3-V5-6R2020.png",
  "Creo Parametric": "img/icons/Creo Parametric.svg",
  "Inventor": "img/icons/Inventor.png",
  "Mastercam": "img/icons/Mastercam.png",
  "Powermill": "img/icons/Powermill.png",
  "Rhinoceros": "img/icons/Rhinoceros.svg",
  "Solid Edge": "img/icons/Solid Edge.png",
  "SolidWorks": "img/icons/SolidWorks.svg",
  "Unigraphics NX": "img/icons/Unigraphics NX.png",
  "Altium Designer": "img/icons/Altium Designer.png",
  "EPLAN Electric P8": "img/icons/eplan.png",
  "Multisim": "img/icons/Multisim_200.png",
  "Proteus": "img/icons/Proteus.png",
  "After Effects": "img/icons/After Effects.jpg",
  "Amos": "img/icons/Amos.png",
  "Blender": "img/icons/Blender.png",
  "CAD MAP3D": "img/icons/CAD MAP3D.png",
  "CAD MEP": "img/icons/CAD MEP.png",
  "CAD Plant3D": "img/icons/CAD Plant3D.png",
  "CAD建筑版": "img/icons/CAD建筑版.png",
  "CAD机械版": "img/icons/CAD机械版.png",
  "CAD电气版": "img/icons/CAD电气版.png",
  "CAD迷你看图": "img/icons/CAD迷你看图.png",
  "CAXA 3D实体设计": "img/icons/CAXA 3D实体设计.jpg",
  "CAXA CAPP工艺图表": "img/icons/CAXA CAPP工艺图表.png",
  "CAXA电子图板": "img/icons/CAXA电子图板.png",
  "Civil 3D": "img/icons/Civil 3D.png",
  "CorelCAD": "img/icons/CorelCAD.png",
  "Cytoscape": "img/icons/Cytoscape.png",
  "Dreamweaver": "img/icons/Dreamweaver.jpg",
  "EPLAN Electric": "img/icons/EPLAN Electric.jpg",
  "eplan": "img/icons/eplan.png",
  "Fuzo": "img/icons/Fuzo.jpg",
  "IHS EViews": "img/icons/IHS EViews.jpg",
  "Illustrator": "img/icons/Illustrator.webp",
  "Lightroom": "img/icons/Lightroom.jpg",
  "Lingo": "img/icons/Lingo.png",
  "Maple": "img/icons/Maple.png",
  "Mastercam_icon": "img/icons/Mastercam_icon.png",
  "Mathematica": "img/icons/Mathematica.jpg",
  "MATLAB": "img/icons/MATLAB.jpg",
  "Media Encoder": "img/icons/Media Encoder.webp",
  "Microsoft": "img/icons/Microsoft.webp",
  "Minitab": "img/icons/Minitab.webp",
  "Mplus": "img/icons/Mplus.png",
  "NCSS": "img/icons/NCSS.png",
  "NX": "img/icons/NX.png",
  "Office": "img/icons/Office.jpg",
  "Origin": "img/icons/Origin.png",
  "Photoshop": "img/icons/Photoshop.jpg",
  "PowerMill": "img/icons/PowerMill.png",
  "Prelude": "img/icons/Prelude.jpg",
  "Premiere": "img/icons/Premiere.jpg",
  "Proe": "img/icons/Proe.jpg",
  "Rhino": "img/icons/Rhino.jpg",
  "SketchUp": "img/icons/SketchUp.png",
  "SPSS": "img/icons/SPSS.jpg",
  "stata": "img/icons/stata.png",
  "Tableau": "img/icons/Tableau.png",
  "UG NX": "img/icons/UG NX.png",
  "V-Ray for 3ds Max": "img/icons/V-Ray for 3ds Max.png",
  "V-ray for su": "img/icons/V-ray for su.png",
  "VectorWorks": "img/icons/VectorWorks.png",
  "天正": "img/icons/天正.png"
}
const dataList = ref(sourceData)
const activeTab = ref(0)
const eleOffsetTops = ref([])
const dialogVisible = ref(false)
const dialogInfo = ref({})

onMounted(() => {
  sourceData.forEach((_, index) => {
    const box = document.querySelector('#list-item' + index)
    const distanceToTop = box.offsetTop;
    eleOffsetTops.value[index] = distanceToTop
  });
})
// 新增滚动函数
const scrollToSection = (index) => {
  activeTab.value = index
  // 获取滚动容器
  const container = document.querySelector('body')
  if (container) {
    window.scrollTo({
      behavior: 'smooth',
      top: eleOffsetTops.value[index],
    });
  }
}
const handleClick = (item, index) => {
  dialogVisible.value = true
  dialogInfo.value = item
}
</script>
<style lang="scss" scoped>
.container {
  height: 100%;
  padding-top: 134px;

  .container-main {
    margin: 0 auto;
    color: #d6d6d6;
    display: flex;
    width: 1000px;
    position: relative;

    .container-main-left {
      position: fixed;
      top: 134px;
      width: 120px;
      background: #202328;
      padding: 12px 25px;
      box-shadow: 0 0 2px rgb(98 124 153 / 10%);
      border-radius: 5px;

      p {
        line-height: 40px;
        cursor: pointer;
      }
    }

    .container-main-right {
      width: 100%;
      padding-left: 120px;

      .list-container {
        color: #d6d6d6;

        .list-item {
          border-radius: 5px;
          box-shadow: 0 0 2px rgb(98 124 153 / 10%);
          background: #202328;
        }
      }
    }
  }
}

.container-head {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 9;
  height: 110px;
  text-align: center;
  box-shadow: 0 5px 40px 0 rgba(17, 58, 93, .1);
  backdrop-filter: saturate(180%) blur(6px);
  background: #202328;
  color: #d6d6d6;
  font-size: 22px;
  padding-top: 10px;

  p {
    font-size: 16px;

    span {
      color: red;
    }
  }
}


.list-container {
  display: flex;
  flex-wrap: wrap;
  padding: 0 30px;
}

.list-item {
  width: 100%;
  padding: 15px 25px;
  margin-bottom: 30px;
  border-radius: 5px;
  cursor: pointer;

  h3 {
    font-weight: 500;
    font-size: 22px;
    margin-bottom: 20px;
  }

  .list-item-content {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;

    .list-item-content-item {
      transition: all .3s;
      width: 243px;
      padding: 14px;
      border: 1px solid #15181d;
      border-radius: 5px;
      display: flex;
      align-items: center;

      &:hover {
        transform: translateY(-5px);
        color: #217aff;
      }

      .img {
        margin-right: 8px;
        width: 35px;
        height: 35px;
      }

      .image-slot {
        height: 100%;
        display: flex;
        align-items: center;
      }
    }
  }
}

.tips {
  background: #fff0f0;
  color: #9f2222;
  height: 48px;
  line-height: 48px;
  padding: 0 20px;
  margin-bottom: 20px;
}

.dialog-content {
  font-size: 16px;
  margin-bottom: 30px;

  .dialog-content-title {
    font-size: 25px;
    font-weight: 600;
    margin-bottom: 10px;
  }
}

::v-deep(.el-dialog__title) {
  font-size: 30px;
  font-weight: 600;
}

::v-deep(.el-dialog__body) {
  max-height: 60vh;
  overflow-x: auto;
}
</style>