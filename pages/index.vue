<template>
  <div style="margin-left: 20px">
    <el-input
      v-model="input"
      placeholder="请输入手机号"
      maxlength="11"
      clearable
      style="width: 200px"
    />
    <el-select v-model="value" placeholder="请选择">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      />
    </el-select>
    <el-button
      type="primary"
      round
      style="margin: 20px"
      @click="query_verification"
    >
      确定
    </el-button>
    <div>
      {{ verificatio_code }}
    </div>
  </div>
</template>

<script>
// import { ElMessage } from 'element-plus'
export default {
  data () {
    return {
      input: '',
      verificatio_code: '',
      options: [{
        value: 'VerificationCode',
        label: '验证码'
      }, {
        value: 'Uid',
        label: 'uid'
      }],
      value: 'VerificationCode'
    }
  },
  methods: {
    query_verification () {
      if (this.value === 'VerificationCode') {
        this.$axios.get('/phone2verificationCode/' + this.input)
          .catch((error) => {
            if (error.response) {
              this.$message({
                showClose: true,
                message: '请输入正确的手机号',
                type: 'error'
              })
            } else {
              this.$message({
                showClose: true,
                message: '未知错误',
                type: 'error'
              })
            }
          })
          .then((res) => {
            if (res !== null) {
              this.verificatio_code = res
            } else {
              this.verificatio_code = '未查询到验证码!'
            }
          })
      } else {
        this.$axios.get('/phone2uid/' + this.input)
          .catch((error) => {
            if (error.response) {
              this.$message({
                showClose: true,
                message: '请输入正确的手机号',
                type: 'error'
              })
            } else {
              this.$message({
                showClose: true,
                message: '未知错误',
                type: 'error'
              })
            }
          })
          .then((res) => {
            if (res !== null) {
              this.verificatio_code = res
            } else {
              this.verificatio_code = '未查询到uid!'
            }
          })
      }
    }
  }
}
</script>

<style>
</style>
