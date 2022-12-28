<template>
  <u-container-layout>
    <div>
      <!-- 头部查询from -->
      <el-form :inline="true" :model="formInline">
        <el-form-item label="姓名">
          <el-input v-model="formInline.user" placeholder="姓名"></el-input>
        </el-form-item>
        <el-form-item label="姓别">
          <el-select v-model="formInline.usertype" placeholder="性别">
            <el-option label="男" value="1" />
            <el-option label="女" value="2" />
          </el-select>
        </el-form-item>
        <el-form-item label="价格" v-show="sta">
          <el-input v-model="formInline.zero" placeholder="价格"></el-input>
        </el-form-item>
        <el-form-item label="账号" v-show="sta">
          <el-input v-model="formInline.userCell" placeholder="账号"></el-input>
        </el-form-item>
        <el-form-item label="地址" v-show="sta">
          <el-input v-model="formInline.usert" placeholder="地址"></el-input>
        </el-form-item>
        <el-form-item label="日期" v-show="sta">
          <el-input v-model="formInline.usertime" placeholder="日期"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="onclear">重置</el-button>
          <el-button type="primary" @click="onSubmit">查询</el-button>
          <el-button text type="" @click="sta = !sta">{{ sta ? '展开∧' : '合并∨' }}</el-button>
        </el-form-item>
      </el-form>

      <!-- 按钮 -->
      <el-row justify="end">
        <el-button type="primary" @click="AddOk"
          ><el-icon><Plus /></el-icon>添加</el-button
        >
        <el-button type="danger" @click="AddCel"
          ><el-icon><Delete /></el-icon>删除</el-button
        >
      </el-row>

      <!-- 表格 -->
      <el-row>
        <el-table :data="filterTableData" style="width: 100%">
          <el-table-column type="selection" width="55" align="center" />
          <el-table-column type="index" label="id" width="50" align="center" />
          <el-table-column label="姓名" prop="date" align="center" />
          <el-table-column label="年龄" prop="name" align="center" />
          <el-table-column label="操作" align="center">
            <template #default="scope">
              <el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>
      </el-row>

      <!-- 添加-dalos -->
      <el-dialog v-model="dialogFormVisible" title="新增">
        <el-form ref="dialogForm" :model="form" :rules="rules">
          <el-form-item label="活动名称" :label-width="150" prop="name">
            <el-input v-model="form.name" autocomplete="off" />
          </el-form-item>
          <el-form-item label="价格" :label-width="150" prop="region">
            <el-input v-model="form.region" autocomplete="off" />
          </el-form-item>
        </el-form>
        <template #footer>
          <span class="dialog-footer">
            <el-button @click="dialogFormVisibleCel(dialogForm)">取消</el-button>
            <el-button type="primary" @click="dialogFormVisibleOk(dialogForm)"> 确定 </el-button>
          </span>
        </template>
      </el-dialog>
    </div>
  </u-container-layout>
</template>

<script lang="ts" setup>
  import { reactive, ref } from 'vue'
  import type { FormInstance, FormRules } from 'element-plus'
  let formInline = reactive({
    user: '',
    usertype: '',
    zero: '',
    userCell: '',
    usert: '',
    usertime: '',
  })
  type user = {
    date: string
    name: string
  }
  let form = reactive({
    name: '',
    region: '',
  })
  let filterTableData: user[] = [
    { date: 'x', name: 'xxx' },
    { date: 'x', name: 'xxx' },
  ]
  const handleEdit = (index: number, row: user) => {
    console.log(index, row, '2')
  }
  const handleDelete = (index: number, row: user) => {
    console.log(index, row, '3')
  }
  let dialogForm = ref<FormInstance>()
  let sta = ref(true)
  let dialogFormVisible = ref(false)
  const rules = reactive<FormRules>({
    name: [{ required: true, message: '请填写数据名称', trigger: 'blur' }],
    region: [{ required: true, message: '请填写价格', trigger: 'blur' }],
  })
  let onSubmit = () => {
    console.log(formInline)
  }
  let onclear = () => {
    formInline.user = ''
    formInline.usertype = ''
    formInline.zero = ''
    formInline.userCell = ''
    formInline.usert = ''
    formInline.usertime = ''
  }
  const AddOk = () => {
    dialogFormVisible.value = true
  }
  const dialogFormVisibleOk = async (formEl: FormInstance | undefined) => {
    if (!formEl) return
    await formEl.validate((valid) => {
      if (valid) {
        console.log('成功')
        dialogFormVisible.value = false
      } else {
        console.log('失败')
      }
    })
  }
  const dialogFormVisibleCel = (formEl: FormInstance | undefined) => {
    dialogFormVisible.value = false
    if (!formEl) return
    formEl.resetFields()
  }
</script>

<style lang="scss" scoped>
  ::v-deep(.el-divider--horizontal) {
    margin: 10px 0;
  }
</style>
