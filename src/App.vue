<template>
<div class="table-box">
  <!-- 标题 -->
  <div class="title">
    最简单的 CRUD demo
  </div>
  <!-- query -->
  <div class="query-box">
    <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" @input="handleQueryName"/>
    <div class="btn-list">
      <el-button type="primary" @click="handleAdd">增加</el-button>
      <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length>0">删除多选</el-button>
    </div>
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
      <template #default="scope">
        <el-button link type="primary" size="small" @click="handleRowDel(scope.row)" style="color: #F56C6C">删除</el-button>
        <el-button link type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
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
    name: 'Tom1',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  { 
    id:"2",    
    name: 'Tom2',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  { 
    id:"3",    
    name: 'Tom3',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  { 
    id:"4",    
    name: 'Tom4',
    email: '123@qq.com',
    phone: '13088888888',
    state: 'California',
    address: 'No. 189, Grove St, Los Angeles',
  },
  
  ])
  let tableDataCopy = Object.assign([],tableData.value)
  let multipleSelection = ref<string[]>([])
  let dialogFormVisible = ref(false)
  let tableForm = ref({
    id:"99",
    name: "张三",
    email: "123@qq.com",
    phone: "13088888888",
    state: "在职",
    address: "广东省"
  })
  let dialogType = ref("add")

  //方法

  //搜索
  const handleQueryName = (val) => {
    // console.log(val);
    if(val.length > 0){
      tableData.value = tableData.value.filter(item => item.name.match(val))
    } else { 
      tableData.value = tableDataCopy
    }
  }
  //编辑
  const handleEdit = (row) => {
    dialogFormVisible.value = true
    dialogType.value = "edit"
    tableForm.value = row
  }

  //删除一条
  const handleRowDel = ({id}) => {
    //1. 通过id获取对应条目的索引值
    let index = tableData.value.findIndex(item => item.id === id)
    //2. 通过索引值删除对应条目
    tableData.value.splice(index, 1)
  }
  
  const handleDelList = () => {
    //1. 通过id获取对应条目的索引值,循环调用handleRowDel
    multipleSelection.value.forEach(id => {
      handleRowDel({id})
    })
    multipleSelection.value = [] // 清空之前的选中数据
  }

  //选中
    const handleSelectionChange = (val: typeof tableData.value) => {
      multipleSelection.value = [] // 清空之前的选中数据
      val.forEach(item => {
        multipleSelection.value.push(item.id) // 将选中数据的编号添加到multipleSelection数组中
      })
    }

  //新增
  const handleAdd = () => {
    dialogType.value = "add"
    dialogFormVisible.value = true
    tableForm.value = {
      id:"",
      name: "",
      email: "",
      phone: "",
      state: "",
      address: ""
    }
  }

  //确认
  const dialogConfirm = () => {
    //判断是新增还是编辑
    if(dialogType.value === "add") {
    //第一步拿到数据  
    //第二步添加到tableData
    tableData.value.push({
      ...tableForm.value,
      id: (tableData.value.length + 1).toString(), 
    })
    } else {
      //1.获取当前这条的索引
       let index = tableData.value.findIndex(item => item.id === tableForm.value.id)
       console.log(index);
       tableData.value[index] = tableForm.value;
      //2.替换当前索引值对应的数据
    }
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
