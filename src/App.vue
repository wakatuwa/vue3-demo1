<template>
<div class="table-box">
  <!-- 标题 -->
  <div class="title">
    最简单的 CRUD demo
  </div>
  <!-- query -->
  <div class="query-box">
    <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" />
    <el-button type="primary" @click="handleAdd">增加</el-button>
  </div>
  <!-- table -->
  <el-table 
    ref="multipleTableRef"
    :data="tableData"
    style="width: 100%"
    @selection-change="handleSelectionChange">
    <el-table-column type="selection" width="55" />
    <el-table-column prop="name" label="姓名" width="120" />
    <el-table-column prop="email" label="邮箱" width="120" />
    <el-table-column prop="phone" label="电话" width="120" />
    <el-table-column prop="state" label="状态" width="120" />
    <el-table-column prop="address" label="地址" width="300" />
    <el-table-column fixed="right" label="操作" width="120">
      <template #default>
        <el-button link type="primary" size="small" @click="handleRowClick" style="color: #F56C6C">删除</el-button>
        <el-button link type="primary" size="small">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
  <!-- dialog -->
  <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add'?'新增':'编辑'" width="500">
    <el-form :model="tableForm">
      <el-form-item label="姓名" :label-width="80">
        <el-input v-model="tableForm.name" autocomplete="off" />
      </el-form-item>
      <el-form-item label="邮箱" :label-width="80">
        <el-input v-model="tableForm.email" autocomplete="off" />
      </el-form-item>
      <el-form-item label="电话" :label-width="80">
        <el-input v-model="tableForm.phone" autocomplete="off" />
      </el-form-item>
      <el-form-item label="状态" :label-width="80">
        <el-input v-model="tableForm.state" autocomplete="off" />
      </el-form-item>
      <el-form-item label="地址" :label-width="80">
        <el-input v-model="tableForm.address" autocomplete="off" />
      </el-form-item>
    </el-form>
    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="dialogConfirm">
          确认
        </el-button>
      </div>
    </template>
  </el-dialog>
</div>
</template>


<script setup lang="ts">
import { ref } from 'vue';

  //数据
  let queryInput = ref("")
  let tableData = ref([
  { 
    id:"1",    
    name: 'Tom',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  { 
    id:"2",    
    name: 'Tom',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  { 
    id:"3",    
    name: 'Tom',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  { 
    id:"4",    
    name: 'Tom',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  
  ])
  let multipleSelection = ref([])
  let dialogFormVisible = ref(false)
  let tableForm = ref({
    name: "张三",
    email: "123@qq.com",
    phone: "13088888888",
    state: "在职",
    address: "广东省"
  })
  let dialogType = ref("add")
  //方法
  const handleRowClick = () => {
    console.log("handleRowClick")
  }

  const handleSelectionChange = (val) => {
    multipleSelection.value = val
    
  }

  const handleAdd = () => {
    dialogFormVisible.value = true
    tableForm.value = {
      name: "",
      email: "",
      phone: "",
      state: "",
      address: ""
    }
  }

  const dialogConfirm = () => {
    //第一步拿到数据 
    
    //第二步添加到tableData
    tableData.value.push({
      id: (tableData.value.length + 1).toString(),
      ...tableForm.value
    })

    //第三步关闭弹窗
    dialogFormVisible.value = false
  }


</script>



<style scoped>
.table-box {
  margin: 200px auto;
  width: 800px;
}
.title {
  text-align: center;
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 20px;
}
.query-box {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}
.query-input {
  width: 200px;
}
</style>
