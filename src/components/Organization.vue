!<template>
  <div class="organization">
    <div class="main_one">
            <el-breadcrumb separator="/">
              <el-breadcrumb-item> <el-icon>
                  <Share />
                </el-icon> 厦门嗨行旅游</el-breadcrumb-item>
              <el-breadcrumb-item> <span style="font-weight: 600; color: black;">{{ msg_name }}</span> </el-breadcrumb-item>
            </el-breadcrumb>
          </div>

          <div class="main_two">

            <el-input class="el-input" v-model="search" style="width: 240px" placeholder="搜索(Command+G)" size="large" @input="change_input">
              <template #prefix>
                <el-icon>
                  <Search />
                </el-icon>
              </template>
            </el-input>

            <el-select v-model="search" size="large" style="width: 45px" empty placeholder="" suffix-icon="" @change="change_value">
              <template #prefix>
                <el-icon>
                  <ArrowDownBold />
                </el-icon>
              </template>
              <el-option v-for="item in search_options" :key="item.value" :label="item.label" :value="item.value" />
            </el-select>

            <el-select v-model="login_state" size="large" style="width: 120px" empty placeholder="登录状态">
              <el-option v-for="item in login_state_options" :key="item.value" :label="item.label" :value="item.value" />
            </el-select>

            <span style="color:rgb(168, 171, 178); font-size: 14px; padding-left: 10px; padding-right: 10px">{{ user_tableData.length }} 个成员</span>
          </div>

          <div class="main_three">
          <el-table 
          ref="multipleTableRef"
          :data="change_user_tableData" 
          stripe style="width: 100%"
          >

            <el-table-column type="selection" width="55" />

            <el-table-column prop="name" label="姓名"  />
            <el-table-column prop="id" label="用户名"  />
          </el-table>
        </div>

  </div>
</template>

<script setup  lang="ts">
import { ref,onMounted, computed, watch } from "vue"
import userApi from '../api/user'
const props = defineProps(['msg'])


// 主体的搜索框 =======================================================
const search = ref('')
const login_state = ref('')
// 搜索的选项
const search_options = ref([
  {
    value: 'Option1',
    label: 'Option1',
  }
])
search_options.value = Array.from("abcdefghijklmnopqrstuvwxyz").map(item=>{
    return {
        value: item,
        label: item
    }
})


// 登录状态的选项
const login_state_options = [
  {
    value: "登录",
    label: '登录',
  },
  {
    value: "离线",
    label: '离线',
  },
]


// 表格数据
const user_tableData = ref([
  {
    id: '2016005003',
    name: 'Tom',
  }
])
// 供选择切换显示的数据
const change_user_tableData = ref([
  {
    id: '2016005003',
    name: 'Tom',
  }
])

// 封装加载数据的方法
const load_data = async () => {
  const res_user = await userApi.query({})
//   console.log(res_user)
  user_tableData.value = res_user
  change_user_tableData.value = res_user
}

onMounted(()=>{
    load_data()
})

const msg_name = computed(()=>{
    return props.msg.name
})

watch(msg_name, async (new_val, old_val) => {
  console.log(new_val,old_val)
  load_data()
})


// 输入搜索关键字 查询。 ==============================================
const search_by_word = (word: string) => {  
  change_user_tableData.value = user_tableData.value.filter(item => {  
    return item.name.includes(word);  
  });  
};
// 防抖算法进行搜索
const debounce_search = () => {
    // let timeId = null
    let timeId: number | null = null;
    return function inner_func (word: string)  {
        // 存在则清除定时器，重新计时
        if (timeId) {
           clearTimeout(timeId)
        }
        // 启动定时器，结束后执行搜索
        timeId = setTimeout(() => {
            search_by_word(word)
        }, 1000);
    }
}
const a_func_for_search_with_debounce = debounce_search()

// 多选框被切换选项
const change_value = (value: any) => {
    console.log(value)
    a_func_for_search_with_debounce(value)
}
// 输入框的搜索查询
const change_input = () => {
    console.log(search.value)
    a_func_for_search_with_debounce(search.value)
}

</script>

<style  scoped>
.organization {
    width: 100%;
    height:auto
}
.main_one {
  height: 50px;
  /* margin-top: 5px; */
  padding-left: 10px;
  padding-right: 10px;
  /* border-top: 1px solid black; */
  border-bottom: 1px solid rgb(220, 223, 230);

  display: flex;
  align-items: center;
}

.main_two {

  height: 50px;
  /* margin-top: 5px; */
  padding-left: 10px;
  padding-right: 10px;
  /* border-top: 1px solid black; */
  border-bottom: 1px solid rgb(220, 223, 230);

  display: flex;
  align-items: center;

  .el-input {
    border-color: none;
    background-color: none;

    .el-input__wrapper {
      border-color: none;
      background-color: none;

      .el-input__inner {
        border-color: none;
        background-color: none;
      }
    }
  }

}

.main_three {
  /* margin-top: 5px; */
  padding-left: 10px;
  padding-right: 10px;
  /* border-top: 1px solid black; */
}
</style>
