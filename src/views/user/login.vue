<template>
    <table cellspacing="0" cellpadding="0" :height = "tableHeight" :width = "tableWidth">
    <tr>
        <td class="aside" grid></td>
        <td class="main gird" align="center">
          <div class = "form-div">
            <el-form :model="loginForm" :rules="rules" ref="loginForm" label-width="100px">
            <el-form-item label="邮箱" prop="email">
              <el-input v-model="loginForm.email"></el-input>
            </el-form-item>
            <el-form-item label="密码" prop="password">
              <el-input type="password" v-model="loginForm.password"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="login">登陆</el-button>
              <el-button type="primary" @click="toRegister">注册</el-button>
            </el-form-item>
            </el-form>
          </div>
        </td>
        <td class="aside grid" ></td>
    </tr>
    </table> 
</template>

<script>
  export default {
    name: "UserLogin",
    data() {
      return {
        loginForm: {
          email: '',
          password: '',
        },
        rules: {
          email: [
            { required: true, message: '请输入注册时的邮箱', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' }
          ]
        }
      };
    },
    computed: {
    },
    created() {
    },
    methods: {
      login() {
        this.$axios({
          method: 'post',
          url:'/login',
          data: JSON.stringify({
            email: this.loginForm.email,
            password: this.loginForm.password
          })
        }).then((res)=>{
          console.log(res.data)
          if (res.code == 1000) {
            localStorage.setItem("loginResult", JSON.stringify(res.data));
            this.$store.commit("login", res.data);
            const h = this.$createElement;
            this.$notify({
              title: '登陆成功',
              message: h('i', { style: 'color: teal'}, '欢迎使用')
            });
            this.$router.push({path: this.redirect || '/' })
          } else {
            const h = this.$createElement;
            this.$notify({
            title: '验证错误',
            message: h('i', { style: 'color: teal'}, '请检查密码和网络')
            });
            console.log(res.msg)
          }
        }).catch((error)=>{
          console.log(error)
        })
      },
      toRegister() {
         this.$router.push("/user-register") 
      }
    },
    computed: {
      tableHeight: function() {
        return (window.innerHeight) + 'px';
      },
      tableWidth: function() {
        return (window.innerWidth) + 'px';
      }
    }
    
  }
  
</script>

<style>
.form-div {
  padding-left: 30%;
  padding-right: 30%;
}
.gird {
    border: 2px  solid #000;
}
</style>