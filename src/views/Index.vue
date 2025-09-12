<template>
  <div class="container">
    <div class="container-head">
      <div>小林软仓</div>
    </div>

    <div class="container-main">
      <div class="container-main-left">
        <p v-for="(item, index) in dataList" :key="index">
          {{ item.tab }}
        </p>
      </div>
      <div class="container-main-right">
        <div class="list-container">
          <div class="list-item" v-for="(item, index) in dataList" :key="index"
            @click="handleClick($event, item, i, j, k)">
            <h3>{{ item.tab }}</h3>
            <div v-for="(list, j) in item.list" :key="list" :title="list.title" :name="list.title"
              :disabled="list.nav.length === 0">
              <el-image class="img" :src="list.icon">
                <template #error>
                  <div class="image-slot">
                    <el-icon>{{ list.title.toUpperCase()[0] }}</el-icon>
                  </div>
                </template>
              </el-image>
              {{ list.title }}
              <div class="list-container">
                <div class="list-item" v-for="(item, k) in list.nav" :key="item"
                  @click="handleClick($event, item, i, j, k)" :class="{ active: [i, j, k].join('-') === currentIndex }">
                  <div class="name">{{ item.name }}</div>
                  <div class="download">download</div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- <el-tabs tab-position="left">
        <el-tab-pane v-for="(data, i) in dataList" :key="data" :label="data.tab">
          <el-collapse accordion>
            <el-collapse-item v-for="(list, j) in data.list" :key="list" :title="list.title" :name="list.title"
              :disabled="list.nav.length === 0">
              <template #title>
                <el-image class="img" :src="list.icon">
                  <template #error>

                    <div class="image-slot">
                      <el-icon>{{ list.title.toUpperCase()[0] }}</el-icon>
                    </div>
                  </template>
</el-image>
{{ list.title }}
</template>
<div class="list-container">
  <div class="list-item" v-for="(item, k) in list.nav" :key="item" @click="handleClick($event, item, i, j, k)"
    :class="{ active: [i, j, k].join('-') === currentIndex }">
    <div class="name">{{ item.name }}</div>
    <div class="download">download</div>
  </div>
</div>
</el-collapse-item>
</el-collapse>
</el-tab-pane>
</el-tabs> -->
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import sourceData from '@/assets/data.json'
const dataList = ref(sourceData)

const currentIndex = ref('')

const handleClick = (event, item, i, j, k) => {
  currentIndex.value = [i, j, k].join('-')
  window.open(item.url, '_blank')
}
</script>
<style lang="scss" scoped>
.container {
  // width: 1000px;
  width: 100vw;
  height: 100%;
  padding-top: 94px;

  .container-main {
    margin: 0 auto;
    color: #d6d6d6;
    display: flex;
    .container-main-left {
      width: 120px;
      background: #202328;
      padding: 25px;
      box-shadow: 0 0 2px rgb(98 124 153 / 10%);
      border-radius: 5px;

      p {
        line-height: 40px;
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
  height: 64px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 5px 40px 0 rgba(17, 58, 93, .1);
  backdrop-filter: saturate(180%) blur(6px);
  background: #202328;
  color: #fff;
  font-size: 22px;
}


.container-main {
  width: 980px;
}


/*列表*/
.img {
  width: 25px;
  height: 25px;
  margin-right: 10px;
  background: #eee;
  border-radius: 3px;
}

.image-slot {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: var(--el-fill-color-light);
  color: var(--el-text-color-secondary);
  font-size: 30px;
}

.image-slot .el-icon {
  font-size: 20px;
}

.list-container {
  display: flex;
  flex-wrap: wrap;
  padding: 5px 30px;
}

.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 45%;
  padding: 15px;
  background: #f5f5f5;
  margin-right: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
  color: #333;
  cursor: pointer;
}

/*选中状态*/
.active {
  background-color: #ff8c00ff;
}
</style>