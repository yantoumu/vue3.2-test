
<template>
  <div class="table-box">
    <!-- 标题-->
    <div class="title">
      <h2>
        CRUD Demo
      </h2>
    </div>
    <!-- query -->
    <div class="query-box">
      <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索" />
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
        @selection-change="handleSelectionChange"
        border>


      <el-table-column type="selection" width="55" />
      <el-table-column prop="name" label="姓名" width="120" />
      <el-table-column prop="email" label="邮箱" width="120" />
      <el-table-column prop="phone" label="电话" width="120" />
      <el-table-column prop="state" label="状态" width="120" />
      <el-table-column prop="address" label="地址" width="600" />

      <el-table-column fixed="right" label="操作" width="120">
        <template #default="scope">
          <el-button link type="primary" size="small" @click="handleRowDel(scope.row)" style="color: #F56C6C">删除</el-button>
          <el-button link type="primary" size="small" style="color: aqua">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- dialog -->
    <el-dialog v-model="dialogFormVisible" :title="dialogType=='add'? '新增': '编辑'">
      <el-form :model="tableForm">
        <el-form-item label="姓名" :label-width="60">
          <el-input v-model="tableForm.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="邮箱" :label-width="60">
          <el-input v-model="tableForm.email" autocomplete="off" />
        </el-form-item>
        <el-form-item label="电话" :label-width="60">
          <el-input v-model="tableForm.phone" autocomplete="off" />
        </el-form-item>
        <el-form-item label="状态" :label-width="60">
          <el-input v-model="tableForm.state" autocomplete="off" />
        </el-form-item>
        <el-form-item label="地址" :label-width="60">
          <el-input v-model="tableForm.address" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogConfirm">
          确定
        </el-button>
      </span>
      </template>
    </el-dialog>
  </div>
</template>
<script setup>

/*  数据  */
let queryInput =$ref("")
let tableData =$ref([
  {
    id:"1",
    name: 'Tom',
    email:"123@qq.com",
    state: 'California',
    phone:"13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:"2",
    name: 'Tom',
    email:"123@qq.com",
    state: 'California',
    phone:"13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:"3",
    name: 'Tom',
    email:"123@qq.com",
    state: 'California',
    phone:"13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id:"4",
    name: 'Tom',
    email:"123@qq.com",
    state: 'California',
    phone:"13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },

])
let multipleSelection = $ref([])
let dialogFormVisible = $ref(false)
let tableForm = $ref({
  name:"张三",
  email:"123@qq.com",
  phone:"13827183123",
  state:"在职",
  address:"北京"
})
let dialogType =$ref('add')

/* 方法 */

// 删除一条
const handleRowDel=({id})=>{
console.log(id)
  // 通过id过去到条目对应的索引值
  let  index =tableData.findIndex(item=>item.id===id)
  // 通过索引值进行删除对应条目
  tableData.splice(index,1)
}

const handleDelList = ()=>{
  multipleSelection.forEach(id=>{
    handleRowDel({id})
  })
  multipleSelection = []
}


// 选中
const handleSelectionChange = (val) => {
  // multipleSelection = val
  // console.log(val);
  //选中之前清空
  multipleSelection = []
  val.forEach(item =>{
    multipleSelection.push(item.id)
  })
}


// 添加
const handleAdd=()=>{
  dialogFormVisible = true
  //选中新增,清空数据
  tableForm={}
}

// 确认
const dialogConfirm=()=>{
  //点击确认.关闭弹窗
  dialogFormVisible=false
  // 拿到数据
  // 添加到 table
  tableData.push({
    id:(tableData.length+1).toString(),
    ...tableForm
  })
  console.log(tableData);

}


</script>
<style scoped>
.table-box{
  margin: 200px;
  width: 800px;
}
.title{
text-align: center;
}
.query-box{
display: flex;
  justify-content: space-between;
  margin-bottom: 20px;

}
.query-input{
  width: 200px;

}

</style>

