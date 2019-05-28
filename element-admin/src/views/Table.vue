<template>
  <div id="table">
    <el-table
      :data="student"
      height="260"
      border
      stripe
      style="width: 100%"
      :row-class-name="tableRowClassName"
    >
      <el-table-column fixed type="selection" width="50"></el-table-column>
      <el-table-column type="index"></el-table-column>
      <el-table-column prop="date" label="日期" width="180"></el-table-column>
      <el-table-column prop="name" label="姓名" width="180"></el-table-column>
      <el-table-column prop="phone" label="手机号" width="180"></el-table-column>
      <el-table-column prop="zip" label="邮编" width="180"></el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>

      <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
          <el-button type="text" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>

    
	<el-table
        :data="student"
		max-height="250"
        border
        stripe
        style="width: 100%"
      >
        <el-table-column fixed type="selection" width="50"></el-table-column>
        <el-table-column type="index"></el-table-column>
        <el-table-column prop="date" label="日期" width="180"></el-table-column>
        <el-table-column prop="name" label="姓名" width="180"></el-table-column>
        <el-table-column prop="phone" label="手机号" width="180"></el-table-column>
        <el-table-column prop="zip" label="邮编" width="180"></el-table-column>
        <el-table-column prop="address" label="地址"></el-table-column>

        <el-table-column fixed="right" label="操作" width="120">
          <template slot-scope="scope">
            <el-button
              @click.native.prevent="deleteRow(scope.$index, tableData)"
              type="text"
              size="small"
            >移除</el-button>
          </template>
        </el-table-column>
      </el-table>
	<el-table  v-loading="loading" :data="student" border stripe style="width: 100%" :row-class-name="tableRowClassName" @selection-change="handleSelectionChange">
			<el-table-column fixed type="selection" width="55">
			</el-table-column>
			<el-table-column type="index">
			</el-table-column>
			<el-table-column prop="date" label="日期"></el-table-column>
			<el-table-column prop="name" label="姓名"></el-table-column>
			<el-table-column prop="phone" label="手机号"></el-table-column>
			  <el-table-column prop="zip" label="邮编"></el-table-column>
			<el-table-column prop="address" label="地址"></el-table-column>

			<el-table-column label="操作">
				<template slot-scope="scope">
					<el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
					<el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
				</template>
			</el-table-column>

		</el-table>

	<el-pagination
	
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="page"
      :page-sizes="[10, 20, 30, 40]"
      :page-size="size"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>
	
	<el-dialog title="提示" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
			<el-form ref="client"  :model="editData" label-width="80px">
				<el-form-item label="姓名" >
					<el-input v-model="editData.name" placeholder="请输入姓名"></el-input>
				</el-form-item>
				<el-form-item label="联系方式" prop='phone'>
					<el-input v-model="editData.phone" placeholder="请输入手机号"></el-input>
				</el-form-item>
				<el-form-item label="日期" >
					<el-input v-model="editData.date" placeholder="请输入姓名"></el-input>
				</el-form-item>
				<el-form-item label="地址" prop='address'>
					<el-input v-model="editData.address" placeholder="请输入手机号"></el-input>
				</el-form-item>
			
			</el-form>
			<span slot="footer" class="dialog-footer">
			    <el-button @click="dialogVisible = false">取 消</el-button>
			    <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
			</span>
		</el-dialog>

  </div>
</template>

<script>
export default {
  data() {
    return {
      student: [
        {
          date: "2019.5.28",
          name: "小明",
          phone: "18017005465",
          address: "上海徐家汇汇银广场",
          zip: "222999"
        },
        {
          date: "2019.5.28",
          name: "小明",
          phone: "18017005465",
          address: "上海徐家汇汇银广场",
          zip: "222999"
        },
        {
          date: "2019.5.28",
          name: "小明",
          phone: "18017005465",
          address: "上海徐家汇汇银广场",
          zip: "222999"
        },
        {
          date: "2019.5.28",
          name: "小明",
          phone: "18017005465",
          address: "上海徐家汇汇银广场",
          zip: "222999"
        },
        {
          date: "2019.5.28",
          name: "小明",
          phone: "18017005465",
          address: "上海徐家汇汇银广场",
          zip: "222999"
        }
      ]
    };
  },
  methods: {
    tableRowClassName({ row, rowIndex }) {
      if (rowIndex === 1) {
        return "warning-row";
      } else if (rowIndex === 3) {
        return "success-row";
      }
      return "";
    },
    handleClick(row) {
      console.log(row);
	},
	 deleteRow(index, rows) {
        rows.splice(index, 1);
	  },
	  handleDelete(index,row){
		  this.$confirm('此操作将永久删除该学生信息, 是否删除?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
	  },
	  handleEdit(index,row){
			this.dialogVisible = true;
			this.editData = {...this.student[index]};
	  },
	  handleClose(){
				this.dialogVisible = false;
	},
	handleSizeChange(val){
		
	}

  }
};
</script>

<style>
.warning-row {
  background: oldlace;
}

.success-row {
  background: #f0f9eb;
}
</style>