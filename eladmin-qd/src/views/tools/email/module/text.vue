<template>
  <el-form ref="form" :model="form" :rules="rules" style="margin-top: 6px;" size="small" label-width="100px">
    <el-form-item label="发件人邮箱" prop="fromUser">
      <el-input v-model="form.fromUser" style="width: 40%"/>
      <span style="color: #C0C0C0;margin-left: 10px;">Sender mailbox</span>
    </el-form-item>
    <el-button :loading="loading" style="margin-left:5%;" size="medium" type="success" @click="doSubmit">保存配置</el-button>
  </el-form>
</template>

<script>
import { get, msg } from '@/api/email'
import qs from 'Qs'
export default {
  name: 'Config',
  data() {
    return {
      loading: false, form: { id: 1, fromUser: '' },
      rules: {
        fromUser: [
          { required: true, message: '请输入发件人邮箱', trigger: 'blur' },
          { type: 'email', message: '请输入正确的邮箱地址', trigger: 'blur' }
        ]
      }
    }
  },
  created() {
    this.init()
  },
  methods: {
    init() {
      get().then(res => {
        this.form = res
        this.form.fromUser = 'ccit.com@163.com'
      })
    },
    doSubmit() {
      this.$refs['form'].validate((valid) => {
        if (valid) {
          // this.loading = true
          let data = new FormData()
    	  data.append('fromUser', this.form.fromUser)
    	  console.log(data)
    	  let data1 = qs.stringify({
			"fromUser": this.form.fromUse
		  });
          msg(data1).then(res => {
            this.$notify({
              title: '修改成功',
              type: 'success',
              duration: 2500
            })
            this.loading = false
          }).catch(err => {
            this.loading = false
            console.log(err.response.data.message)
          })
        } else {
          return false
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
