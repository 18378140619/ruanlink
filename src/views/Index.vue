<template>
  <div class="container">
    <div class="container-head">
      <div>小林软仓</div>
      <p>所有文件解压密码均为:alin123本站为公益站点，请Ctrl + D 保存网站www.alin123.top到收藏夹</p>
    </div>

    <div class="container-main">
      <div class="container-main-left">
        <p v-for="(item, index) in dataList" :key="index" @click="scrollToSection(index)">
          {{ item.tab }}
        </p>
      </div>
      <div class="container-main-right">
        <div class="list-container">
          <div class="list-item" :id="'list-item' + index" v-for="(item, index) in dataList" :key="index">
            <h3>{{ item.tab }}</h3>
            <div class="list-item-content">
              <div class="list-item-content-item" v-for="(list, idx) in item.list" :key="list" :title="list.title"
                @click="handleClick(list, idx)" :name="list.title" :disabled="list.nav.length === 0">
                <el-image class="img" :src="list.icon">
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
    <el-dialog v-model="dialogVisible" :title="dialogInfo.title" width="980" :before-close="handleClose">
      <div class="tips">所有文件解压密码均为:alin123</div>
      <div class="dialog-content" v-for="item in dialogInfo.nav" :key="item.title">
        <p class="dialog-content-title">{{ item.name }}</p>
        <p>下载: <a :href="item.url"> {{ item.url }}</a></p>
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
  padding-top: 104px;

  .container-main {
    margin: 0 auto;
    color: #d6d6d6;
    display: flex;
    width: 1000px;
    position: relative;

    .container-main-left {
      position: fixed;
      top: 104px;
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
  height: 80px;
  text-align: center;
  box-shadow: 0 5px 40px 0 rgba(17, 58, 93, .1);
  backdrop-filter: saturate(180%) blur(6px);
  background: #202328;
  color: #d6d6d6;
  font-size: 22px;
  padding-top: 10px;

  p {
    font-size: 16px;
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
</style>