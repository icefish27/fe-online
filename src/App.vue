<template>
  <div class="content">

    <el-container>
      <!-- 头部 -->
      <el-header>
        <el-menu :default-active="activeIndex_headerMenu" class="el-menu-demo" mode="horizontal"
          @select="handleSelect_headerMenu">
          <el-menu-item index="1">成员管理</el-menu-item>
          <el-menu-item index="2">职位管理</el-menu-item>
          <el-menu-item index="3">成员维护</el-menu-item>
        </el-menu>
      </el-header>


      <el-container>

        <!-- 侧栏 -->
        <el-aside width="200px">
          <div class="aside_one">
            <span>部门</span>
            <el-icon>
              <Plus />
            </el-icon>
          </div>

          <div class="aside_two">
            <el-menu default-active="2" class="el-menu-vertical-demo" >
              <el-sub-menu index="1">
                <template #title>
                  <el-icon>
                    <Share />
                  </el-icon>
                  <span>厦门嗨行旅游 * {{org_data.length}}</span>
                </template>
                <el-menu-item-group>

                  <el-menu-item :index="'1-'+ (index+1)" v-for="(item,index) in org_data"
                  @click="send_to_child =item"
                  >
                    <el-icon>
                      <ArrowRight />
                    </el-icon>{{ item.name }}
                  </el-menu-item>

                </el-menu-item-group>
              </el-sub-menu>
              <el-menu-item index="2">
                <el-icon>
                  <Share />
                </el-icon>
                <span>未分配部门</span>
              </el-menu-item>
            </el-menu>

          </div>
        </el-aside>


        <!-- 内容 -->
        <el-main>
          <organization :msg="send_to_child" />
        </el-main>




      </el-container>
    </el-container>




  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'


// 头部菜单 =========================================================
const activeIndex_headerMenu = ref('1')
const handleSelect_headerMenu = (key, keyPath) => {
  console.log("handleSelect_headerMenu", key, keyPath)
}


// 侧边栏菜单 ========================================================
// 组织数据
const org_data = ref([
  {
    id: '2016005003',
    name: 'Tom',
  }
])

const send_to_child = ref({
  id: '1111',
  name: "tom"
})


// 主体 =============================================================
import Organization from './components/Organization.vue';
import orgApi from './api/org'

//====================================================================
onMounted(async () => {
  const res_org = await orgApi.query()
  // console.log(res_org)
  org_data.value = res_org

})

</script>

<style scoped>
.content {
  width: 100%;
  height: 100%;

  .el-container {
    width: 100%;

    /* height: 100%; */
    .el-header {
      padding: 0;
    }

    .el-aside {
      /* height: 100%; */
      padding: 0;
      /* background-color: rgba(154, 248, 217, 0.377); */
      border-right: 1px solid rgb(220, 223, 230);
    }

    .el-main {
      /* height: 100%; */
      padding: 0;
      /* background-color: rgba(203, 255, 196, 0.308); */
    }
  }
}

.aside_one {
  display: flex;
  justify-content: space-between;
  align-items: center;

  height: 50px;
  /* margin-top: 5px; */
  padding-left: 10px;
  padding-right: 10px;
  /* border-top: 1px solid black; */
  border-bottom: 1px solid rgb(220, 223, 230);
}
</style>
