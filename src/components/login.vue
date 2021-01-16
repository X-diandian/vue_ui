<template>
    <div class="login_container">
        <div class="login_box">
            <!-- 头像区域 -->
            <div class="avatar_box">
                <img src="../assets/logo.png" />
            </div>
            <!-- 表单区域 -->
            <el-form ref="loginFromRef" :rules="rules" label-width="0" :model="loginFrom" class="login_from">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input
                        v-model="loginFrom.username"
                        prefix-icon="iconfont icon-user"
                    ></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input
                        v-model="loginFrom.password"
                        type="password"
                        prefix-icon="iconfont icon-3702mima"
                    ></el-input>
                </el-form-item>
                <!-- 按钮区域 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登陆</el-button>
                    <el-button type="info" @click="resetLoginFrom">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      // 这是登陆表单的数据绑定对象
      loginFrom: {
        username: 'admin',
        password: '123456'
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginFrom() {
      this.$refs.loginFromRef.resetFields()
      console.log('111')
    },
    login() {
      this.$refs.loginFromRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginFrom)
        if (res.meta.status !== 200) return this.$message.error('错了哦，这是一条错误消息')
        this.$message({
          message: '登陆成功',
          type: 'success'
        })
        // console.log('登陆成功')
        console.log(res)
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login_container {
    height: 100%;
    background-color: #2b4b6b;
}
.login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -250px;
    margin-top: -150px;
    .avatar_box {
        width: 100px;
        height: 100px;
        border: 1px solid #eee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #ddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: white;
        img {
            width: 100%;
            height: 100px;
            border-radius: 50%;
            background-color: #eee;
        }
    }
}
.btns {
    display: flex;
    justify-content: flex-end;
}
.login_from {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
}
</style>
