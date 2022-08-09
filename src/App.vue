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
      <el-input v-model="queryInput" class="query-input" placeholder="请输入姓名搜索"/>
      <div class="btn-list">
        <el-button type="primary" @click="handleAdd">增加</el-button>
        <el-button v-if="multipleSelection.length>0" type="danger" @click="handleDelList">删除多选</el-button>
      </div>
    </div>
    <!-- table -->
    <el-table
        ref="multipleTableRef"
        :data="tableData"
        border
        style="width: 100%"
        @selection-change="handleSelectionChange">
      <el-table-column type="selection" width="55"/>
      <el-table-column label="姓名" prop="name" width="120"/>
      <el-table-column label="邮箱" prop="email" width="120"/>
      <el-table-column label="电话" prop="phone" width="120"/>
      <el-table-column label="状态" prop="state" width="120"/>
      <el-table-column label="地址" prop="address" width="600"/>

      <el-table-column fixed="right" label="操作" width="120">
        <template #default="scope">
          <el-button link size="small" style="color: #F56C6C" type="primary" @click="handleRowDel(scope.row)">删除
          </el-button>
          <el-button link size="small" style="color: aqua" type="primary" @click="handleEdit(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- dialog -->
    <el-dialog v-model="dialogFormVisible" :title="dialogType=='add'? '新增': '编辑'">
      <el-form :model="tableForm">
        <el-form-item :label-width="60" label="姓名">
          <el-input v-model="tableForm.name" autocomplete="off"/>
        </el-form-item>
        <el-form-item :label-width="60" label="邮箱">
          <el-input v-model="tableForm.email" autocomplete="off"/>
        </el-form-item>
        <el-form-item :label-width="60" label="电话">
          <el-input v-model="tableForm.phone" autocomplete="off"/>
        </el-form-item>
        <el-form-item :label-width="60" label="状态">
          <el-input v-model="tableForm.state" autocomplete="off"/>
        </el-form-item>
        <el-form-item :label-width="60" label="地址">
          <el-input v-model="tableForm.address" autocomplete="off"/>
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
let queryInput = $ref("")
let tableData = $ref([
  {
    id: "1",
    name: 'Tom',
    email: "123@qq.com",
    state: 'California',
    phone: "13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: "2",
    name: 'Tom',
    email: "123@qq.com",
    state: 'California',
    phone: "13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: "3",
    name: 'Tom',
    email: "123@qq.com",
    state: 'California',
    phone: "13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: "4",
    name: 'Tom',
    email: "123@qq.com",
    state: 'California',
    phone: "13629183123",
    address: 'No. 189, Grove St, Los Angeles',
  },

])
let multipleSelection = $ref([])
let dialogFormVisible = $ref(false)
let tableForm = $ref({
  name: "张三",
  email: "123@qq.com",
  phone: "13827183123",
  state: "在职",
  address: "北京"
})
let dialogType = $ref('add')

/* 方法 */

// 编辑
const handleEdit=(row )=>{
  //打开弹窗
  dialogFormVisible=true
  dialogType='edit'
  console.log(row);
  tableForm ={...row}
}


// 删除一条
const handleRowDel = ({id}) => {
  console.log(id)
  // 通过id过去到条目对应的索引值
  let index = tableData.findIndex(item => item.id === id)
  // 通过索引值进行删除对应条目
  tableData.splice(index, 1)
}

const handleDelList = () => {
  multipleSelection.forEach(id => {
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
  val.forEach(item => {
    multipleSelection.push(item.id)
  })
}


// 添加
const handleAdd = () => {
  dialogFormVisible = true
  //选中新增,清空数据
  tableForm = {}
  dialogType='add'
}

// 确认
const dialogConfirm = () => {
  // 点击确认.关闭弹窗
  dialogFormVisible = false
  // 判断是新增还是编辑
  if (dialogType=='add'){
    // 拿到数据
    // 添加到 table
    tableData.push({
      id: (tableData.length + 1).toString(),
      ...tableForm
    })
  }else {
    // 1. 获取到当前的索引
    let  index= tableData.findIndex(item=>item.id===tableForm.id)
    // 2. 替换当前索引值对应的数据
    console.log(index);
    tableData[index]=tableForm
  }

}


</script>
<style scoped>
.table-box {
  margin: 200px;
  width: 800px;
}

.title {
  text-align: center;
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

