<template>
    <div class="login-container">
        <el-card class="login-box">
            <img src="../../assets/images/logo_index.png" alt="">
            <el-form ref="loginForm" :status-icon="true" :model="loginForm" :rules="loginRules">
                <el-form-item prop="mobile">
                    <el-input v-model="loginForm.mobile" placeholder="请输入手机号"></el-input>
                </el-form-item>
                <el-form-item prop="code">
                    <el-input v-model="loginForm.code" placeholder="请输入验证码" style="width: 240px"></el-input>
                    <el-button style="float: right">发送验证码</el-button>
                </el-form-item>
                <el-form-item>
                    <el-checkbox :value="true">我已阅读并同意用户协议和隐私条款</el-checkbox>
                </el-form-item>
                <el-form-item>
                    <el-button style="width: 100%" type="primary" @click="login()">登 录</el-button>
                </el-form-item>
            </el-form>
        </el-card>
    </div>
</template>

<script>
export default {
  data () {
    const checkMobile = (rule, value, callback) => {
      // 校验逻辑， 把value拿出来进行手机号的格式校验
      if (/^1[3-9]\d{9}/.test(value)) {
        callback()
      } else {
        callback(new Error('手机号格式不正确'))
      }
    }
    return {
      loginForm: {
        mobile: '13911111111',
        code: '246810'
      },
      loginRules: {
        mobile: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          { validator: checkMobile, trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          { len: 6, message: '请输入6位数字', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      // 对整个表单进行校验
      // this.$refs.loginForm.validate(valid => {
      //   if (valid) {
      //     // 提交登录请求   axios是基于primise封装的 post() 返回值一个promise对象
      //     this.$http.post('http://ttapi.research.itcast.cn/mp/v1_0/authorizations', this.loginForm)
      //       .then(res => {
      //         this.$router.push('/')
      //         window.sessionStorage.setItem('xm2-toutiao', JSON.stringify(res.data.data))
      //       })
      //       .catch(() => {
      //         this.$message.error('手机号或验证码输入错误')
      //       })
      //   }
      // })

      this.$refs.loginForm.validate(async valid => {
        if (valid) {
          // 发请 promise对象 给你发请求
          // try { 业务逻辑 } catch(err) { 处理错误 }
          try {
            const res = await this.$http.post('authorizations', this.loginForm)
            window.sessionStorage.setItem('xm2-toutiao', JSON.stringify(res.data.data))
            this.$router.push('/')
          } catch (err) {
            this.$message.error('手机号或验证码错误')
          }
        }
      })
    }
  }
}
</script>

<style lang="less">
.login-container {
    width: 100%;
    height: 100%;
    position: absolute;
    left: 0;
    top: 0;
    background: url(../../assets/images/login_bg.jpg) no-repeat center / cover;
    .login-box {
        width: 400px;
        height: 350px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        img {
            display: block;
            width: 200px;
            margin: 10px auto;
        }
    }
}
</style>
