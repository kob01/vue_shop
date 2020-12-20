<template>
  <div>
    <!-- 面包屑导航区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>商品管理</el-breadcrumb-item>
      <el-breadcrumb-item>商品分类</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 卡片试图区域 -->
    <el-card>
      <el-row>
        <el-col>
          <el-button type="primary" class="add_cate" @click="showAddCateDialog">添加分类</el-button>
        </el-col>
      </el-row>

      <!-- 表格区域 -->
      <tree-table
        :data="catelist"
        :columns="columns"
        :expand-type="false"
        :selection-type="false"
        :show-index="true"
        index-text="序号"
        border
        :show-row-hover="false"
        style="min-width: 600px"
        align="center"
      >
        <template slot="isok" slot-scope="scope">
          <i class="el-icon-success" v-if="scope.row.cat_deleted === false" style="color:lightgreen"></i>
          <i class="el-icon-eror" v-else style="color:red"></i>
        </template>
        <template slot="order" slot-scope="scope">
          <el-tag v-if="scope.row.cat_level === 0" size="small">标签一</el-tag>
          <el-tag type="success" v-else-if="scope.row.cat_level === 1" size="small">标签二</el-tag>
          <el-tag type="warning" v-else size="small">标签三</el-tag>
        </template>
        <template slot="handle">
          <el-button type="primary" icon="el-icon-edit" size="mini">编辑</el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini">删除</el-button>
        </template>
      </tree-table>
      <!-- 分页区域,handleCurrentChange少不了,因为页码变化是通过修改pagenum的 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.num"
        :page-sizes="[3, 5, 10, 15]"
        :page-size="queryInfo.pagesize"
        layout="total,sizes,prev,pager,next,jumper"
        :total="total"
      >
      </el-pagination>
    </el-card>
    <!-- 添加分类区域 -->
    <el-dialog title="添加分类" :visible.sync="addCateDialogVisible" width="50%" @close="addCateDialogClosed">
      <!-- 添加分类的表单 -->
      <el-form :model="addCateForm" :rules="addCateFormRules" ref="addCateFormRef" label-width="100px">
        <el-form-item label="分类名称" prop="cat_name">
          <el-input v-model="addCateForm.cat_name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="父级分类">
          <el-cascader v-model="selectedKeys" :options="parentCateList" :props="cascaderProps" @change="parentCateChanged" clearable></el-cascader>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="addCateDialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addCate">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 查询条件
      queryInfo: {
        type: 3,
        pagenum: 1,
        pagesize: 5
      },
      // 商品数据列表，默认空
      catelist: [],
      total: 0,
      // 为table指定列的定义
      columns: [
        {
          label: '分类名称',
          minWidth: '150px',
          prop: 'cat_name'
        },
        {
          label: '是否有效',
          type: 'template',
          // 表示当前使用模板名称
          template: 'isok',
          headerAlign: 'center'
        },
        {
          label: '排序',
          type: 'template',
          template: 'order',
          headerAlign: 'center'
        },
        {
          label: '操作',
          type: 'template',
          minWidth: '200px',
          template: 'handle',
          headerAlign: 'center'
        }
      ],
      addCateDialogVisible: false,
      // 添加分类的表单数据对象
      addCateForm: {
        cat_name: '', // 将要添加的分类名称
        cat_pid: 0, // 父分类,如果要添加1级分类，则父分类Id应该设置为  `0`
        cat_level: 0 // `0`表示一级分类；`1`表示二级分类；`2`表示三级分类
      },
      // 添加分类表单的验证规则对象
      addCateFormRules: {
        cat_name: [{ required: true, message: '请输入分类名称', trigger: 'blur' }],
        cat_pid: [{ required: true }],
        cat_level: [{ required: true }]
      },
      parentCateList: [], // 父级分类的列表
      cascaderProps: {
        expandTrigger: 'hover',
        value: 'cat_id',
        label: 'cat_name',
        children: 'children',
        checkStrictly: true
      },
      selectedKeys: []
    }
  },
  created() {
    this.getCatelist()
  },
  methods: {
    // 获取商品数据
    async getCatelist() {
      const { data: res } = await this.$http.get('categories', {
        params: this.queryInfo // 记忆点
      })
      if (res.meta.status !== 200) {
        return this.$message.error('获取商品分类失败！')
      }
      this.catelist = res.data.result
      this.total = res.data.total
    },
    handleSizeChange(val) {
      this.queryInfo.pagesize = val
      this.getCatelist()
    },
    handleCurrentChange(val) {
      this.queryInfo.pagenum = val
      this.getCatelist()
    },
    // 点击按钮展开对话框
    showAddCateDialog() {
      this.getParentCateList()
      this.addCateDialogVisible = true
    },
    // 获取父级分类列表
    async getParentCateList() {
      const { data: res } = await this.$http.get('categories', {
        params: { type: 2 } // 记忆点
      })
      if (res.meta.status !== 200) {
        return this.$message.error('获取商品分类失败！')
      }
      this.parentCateList = res.data
    },
    parentCateChanged() {
      // selectedKeys.length>0,有选中分类
      if (this.selectedKeys.length > 0) {
        // 父级分类ID
        this.addCateForm.cat_pid = this.selectedKeys[this.selectedKeys.length - 1]
        this.addCateForm.cat_level = this.selectedKeys.length
      } else {
        this.addCateForm.cat_pid = 0
        this.addCateForm.cat_level = 0
      }
    },
    // 点击添加新分类
    addCate() {
      this.$refs.addCateFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('categories', this.addCateForm)
        if (res.meta.status !== 201) {
          return this.$message.error('添加分类失败！')
        }
        this.$message.success('添加分类成功！')
        this.getCatelist()
        this.addCateDialogVisible = false
      })
    },
    // 监听对话框关闭事件
    addCateDialogClosed() {
      this.$refs.addCateFormRef.resetFields()
      this.selectedKeys = []
      this.addCateForm.cat_pid = 0
      this.addCateForm.cat_level = 0
    }
  }
}
</script>

<style lang="less" scoped>
.add_cate {
  float: left;
}
.el-cascader {
  width: 100%;
}
</style>
