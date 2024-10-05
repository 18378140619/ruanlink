<script setup>
import {ref} from 'vue'
import sourceData from '@/assets/data.json'

console.log(sourceData)

const dataList = ref(sourceData)

const currentIndex = ref('')

const handleClick = (event, item, i, j, k) => {
  currentIndex.value = [i, j, k].join('-')
  window.open(item.url, '_blank')
}
</script>

<template>
  <div class="container">
    <div class="password">
      <el-text size="large" type="primary">压缩包解压密码：ruancang.net</el-text>
    </div>
    <el-tabs tab-position="left">
      <el-tab-pane v-for="(data,i) in dataList" :key="data" :label="data.tab">
        <el-collapse accordion>
          <el-collapse-item v-for="(list,j) in data.list" :key="list" :title="list.title" :name="list.title"
                            :disabled="list.nav.length===0">
            <template #title>
              <!--https://www.cnblogs.com/Fooo/p/17918184.html-->
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
              <div class="list-item" v-for="(item,k) in list.nav" :key="item" @click="handleClick($event,item,i,j,k)"
                   :class="{active:[i, j, k].join('-') === currentIndex}">
                <div class="name">{{ item.name }}</div>
                <div class="download">download</div>
              </div>
            </div>
          </el-collapse-item>
        </el-collapse>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<style scoped>
/*容器*/
.container {
  width: 1000px;
  margin: 0 auto;
}

.password {
  height: 40px;
  line-height: 40px;
  padding-bottom: 40px;
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