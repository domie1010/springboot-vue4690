<template>
    <div>
        <div class="container loginIn">

            <div :class="2 == 1 ? 'left' : 2 == 2 ? 'left center' : 'left right'">
                <el-form class="login-form" label-position="left" :label-width="1 == 3 || 1 == 2 ? '30px': '0px'">
                    <div class="title-container"><h3 class="title">同城上门喂遛宠物预约系统登录</h3></div>
                    <el-form-item :style='{"padding":"0","boxShadow":"0px 2px 4px rgba(0, 0, 0, 0.16)","margin":"0 0 12px 60px","borderColor":"rgba(0,0,0,0)","backgroundColor":"rgba(255, 255, 255, 1)","borderRadius":"8px","borderWidth":"0","width":"35%","borderStyle":"solid","height":"auto"}' :label="1 == 3 ? '用户名' : ''" :class="'style'+1">
            <span v-if="1 != 3" class="svg-container" style="
			color:#333;
			line-height:30px;
			font-size:14px;
			width:30px;
			padding:0;
			margin:0;
			radius:0;
			border-width:0;
			border-style:solid;
			border-color:rgba(0,0,0,0);
			background-color:rgba(0,0,0,0);
			box-shadow:0 0 6px rgba(0,0,0,0);
			}"><svg-icon icon-class="user" /></span>
                        <el-input placeholder="请输入用户名" name="username" type="text" v-model="rulesForm.username" />
                    </el-form-item>
                    <el-form-item :style='{"padding":"0","boxShadow":"0px 2px 4px rgba(0, 0, 0, 0.16)","margin":"0 0 12px 60px","borderColor":"rgba(0,0,0,0)","backgroundColor":"rgba(255, 255, 255, 1)","borderRadius":"8px","borderWidth":"0","width":"35%","borderStyle":"solid","height":"auto"}' :label="1 == 3 ? '密码' : ''" :class="'style'+1">
            <span v-if="1 != 3" class="svg-container" style="color:#333;
			line-height:30px;
			font-size:14px;
			width:30px;
			padding:0;
			margin:0;
			radius:0;
			border-width:0;
			border-style:solid;
			border-color:rgba(0,0,0,0);
			background-color:rgba(0,0,0,0);
			box-shadow:0 0 6px rgba(0,0,0,0);"><svg-icon icon-class="password" /></span>
                        <el-input placeholder="请输入密码" name="password" type="password" v-model="rulesForm.password" />
                    </el-form-item>
                    <el-form-item v-if="roleOptions.length>1" label="角色" prop="loginInRole" class="role" style="display: flex;align-items: center;">
                        <el-radio
                                @change="menuChange"
                                v-for="item in roleOptions"
                                v-bind:key="item.value"
                                v-model="rulesForm.role"
                                :label="item.value"
                        >{{item.key}}</el-radio>
                    </el-form-item>
                    <el-form-item v-if="roleOptions.length==1" label=" " prop="loginInRole" class="role" style="display: flex;align-items: center;">
                    </el-form-item>
                    <el-button type="primary" @click="login()" class="loginInBt" style="background-color: var(--publicMainColor)">{{'1' == '1' ? '登录' : 'login'}}</el-button>          <el-form-item class="setting">
<!--            <div class="register" @click="register('yonghu')">用户注册</div>-->
          </el-form-item>
        </el-form>
      </div>
<!--
                    <el-form-item v-if="0 == '1'" class="code" :label="3 == 3 ? '验证码' : ''" :class="'style'+3">
                        <span v-if="3 != 3" class="svg-container" style="color:rgba(136, 154, 164, 1);line-height:46px"><svg-icon icon-class="code" /></span>
                        <el-input placeholder="请输入验证码" name="code" type="text" v-model="rulesForm.code" />
                        <div class="getCodeBt" @click="getRandCode(4)" style="height:46px;line-height:46px">
                            <span v-for="(item, index) in codes" :key="index" :style="{color:item.color,transform:item.rotate,fontSize:item.size}">{{ item.num }}</span>
                        </div>
                    </el-form-item>

-->

    </div>
  </div>
</template>
<script>

    import menu from "@/utils/menu";

    export default {
        data() {
            return {
                rulesForm: {
                    username: "",
                    password: "",
                    role: "",
                    code: '',
                },
                menus: [],
                roleOptions: [],
                tableName: "",
                codes: [{
                    num: 1,
                    color: '#000',
                    rotate: '10deg',
                    size: '16px'
                },{
                    num: 2,
                    color: '#000',
                    rotate: '10deg',
                    size: '16px'
                },{
                    num: 3,
                    color: '#000',
                    rotate: '10deg',
                    size: '16px'
                },{
                    num: 4,
                    color: '#000',
                    rotate: '10deg',
                    size: '16px'
                }],
            };
        },
        mounted() {
            let menus = menu.list();
            this.menus = menus;
            for (let i = 0; i < this.menus.length; i++) {
                if (this.menus[i].hasBackLogin=='是') {
                    let menuItem={};
                    menuItem["key"]=this.menus[i].roleName;
                    menuItem["value"]=this.menus[i].tableName;
                    this.roleOptions.push(menuItem);
                }
            }
        },
        created() {
            this.getRandCode()

        },
        methods: {
            menuChange(role){
            },
            register(tableName){
                this.$storage.set("loginTable", tableName);
                this.$router.push({path:'/register'})
            },
            // 登陆
            login() {
                let code = ''
                for(let i in this.codes) {
                    code += this.codes[i].num
                }
                if ('0' == '1' && !this.rulesForm.code) {
                    this.$message.error("请输入验证码");
                    return;
                }
                if ('0' == '1' && this.rulesForm.code.toLowerCase() != code.toLowerCase()) {
                    this.$message.error("验证码输入有误");
                    this.getRandCode()
                    return;
                }
                if (!this.rulesForm.username) {
                    this.$message.error("请输入用户名");
                    return;
                }
                if (!this.rulesForm.password) {
                    this.$message.error("请输入密码");
                    return;
                }
                if(this.roleOptions.length>1) {
                    console.log("1")
                    if (!this.rulesForm.role) {
                        this.$message.error("请选择角色");
                        return;
                    }
                    let menus = this.menus;
                    for (let i = 0; i < menus.length; i++) {
                        if (menus[i].tableName == this.rulesForm.role) {
                            this.tableName = menus[i].tableName;
                            this.rulesForm.role = menus[i].roleName;
                        }
                    }
                } else {
                    this.tableName = this.roleOptions[0].value;
                    this.rulesForm.role = this.roleOptions[0].key;
                }
                this.$http({
                    url: `${this.tableName}/login?username=${this.rulesForm.username}&password=${this.rulesForm.password}`,
                    method: "post"
                }).then(({ data }) => {
                    if (data && data.code === 0) {
                        this.$storage.set("Token", data.token);
                        this.$storage.set("userId", data.userId);
                        this.$storage.set("role", this.rulesForm.role);
                        this.$storage.set("sessionTable", this.tableName);
                        this.$storage.set("adminName", this.rulesForm.username);
                        this.$router.replace({ path: "/index/" });
                    } else {
                        this.$message.error(data.msg);
                    }
                });
            },
            getRandCode(len = 4){
                this.randomString(len)
            },
            randomString(len = 4) {
                let chars = [
                    "a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k",
                    "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v",
                    "w", "x", "y", "z", "A", "B", "C", "D", "E", "F", "G",
                    "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R",
                    "S", "T", "U", "V", "W", "X", "Y", "Z", "0", "1", "2",
                    "2", "4", "5", "6", "7", "8", "9"
                ]
                let colors = ["0", "1", "2","2", "4", "5", "6", "7", "8", "9", "a", "b", "c", "d", "e", "f"]
                let sizes = ['14', '15', '16', '17', '18']

                let output = [];
                for (let i = 0; i < len; i++) {
                    // 随机验证码
                    let key = Math.floor(Math.random()*chars.length)
                    this.codes[i].num = chars[key]
                    // 随机验证码颜色
                    let code = '#'
                    for (let j = 0; j < 6; j++) {
                        let key = Math.floor(Math.random()*colors.length)
                        code += colors[key]
                    }
                    this.codes[i].color = code
                    // 随机验证码方向
                    let rotate = Math.floor(Math.random()*60)
                    let plus = Math.floor(Math.random()*2)
                    if(plus == 1) rotate = '-'+rotate
                    this.codes[i].rotate = 'rotate('+rotate+'deg)'
                    // 随机验证码字体大小
                    let size = Math.floor(Math.random()*sizes.length)
                    this.codes[i].size = sizes[size]+'px'
                }
            },
        }
    };
</script>
<style lang="scss" scoped>
    .loginIn {
        min-height: 100vh;
        position: relative;
        background-repeat: no-repeat;
        background-position: center center;
        background-size: cover;
        background-image: url(/tongchengshangmenweiliuxitong/img/back-img-bg.jpg);

    .loginInBt {
        width: 200px;
        height: 200px;
        line-height: 200px;
        margin: -150px 0 0 62%;
        padding: 0;
        color: rgba(255, 255, 255, 1);
        font-size: 40px;
        border-radius: 50%;
        border-width: 0;
        border-style: solid;
        border-color: rgba(64, 158, 255, 1);
        background-color: rgba(0, 73, 142, 1);
        box-shadow: 0px 0px 0px 30px rgba(255,255,255,1);
    }
    .register {
        width: auto;
        height: 24px;
        line-height: 24px;
        margin: 0 0 0 40px;
        padding: 0 10px;
        color: rgba(153, 153, 153, 1);
        font-size: 12px;
        border-radius: 0;
        border-width: 0;
        border-style: solid;
        border-color: rgba(64, 158, 255, 1);
        background-color: rgba(255, 255, 255, 0);
        box-shadow: 0 0 6px rgba(255,0,0,0);
        cursor: pointer;
    }
    .reset {
        width: auto;
        height: 24px;
        line-height: 24px;
        margin: 0;
        padding: 0;
        color: rgba(153, 153, 153, 1);
        font-size: 12px;
        border-radius: 0;
        border-width: 0;
        border-style: solid;
        border-color: rgba(64, 158, 255, 1);
        background-color: rgba(255, 255, 255, 0);
        box-shadow: 0 0 6px rgba(255,0,0,0);
    }


    .left {
        position: absolute;
        left: 0;
        top: 0;
        box-sizing: border-box;
        width: 700px;
        height: 400px;
        margin: 0 auto;
        padding: 0 12px;
        border-radius: 45px;
        border-width: 0;
        border-style: solid;
        border-color: rgba(0,0,0,.3);
        background-color: var(--publicSubColor);
        box-shadow: 0 0 0px rgba(30, 144, 255, .8);

    .login-form {
        background-color: transparent;
        width: 100%;
        right: inherit;
        padding: 0;
        box-sizing: border-box;
        display: flex;
        position: initial;
        justify-content: center;
        flex-direction: column;
    }

    .title-container {
        text-align: center;
        font-size: 24px;

    .title {
        width: 80%;
        line-height: 30px;
        margin: -134px auto 50px;
        padding: 15px 0;
        color: #fff;
        font-size: 24px;
        border-radius: 30px 30px 0 0;
        border-width: 0;
        border-style: solid;
        border-color: rgba(0,0,0,.3);
        background-color: var(--publicSubColor);
        box-shadow: 0 0 6px rgba(0,0,0,0);
    }
    }

    .el-form-item {
        position: relative;

    & /deep/ .el-form-item__content {
          line-height: initial;
      }

    & /deep/ .el-radio__label {
          width: auto;
          height: 14px;
          line-height: 14px;
          margin: 0;
          padding: 0 0 0 10px;
          color: #fff;
          font-size: 14px;
          border-radius: 0;
          border-width: 0;
          border-style: solid;
          border-color: rgba(255, 255, 255, 0);
          background-color: rgba(255, 255, 255, 0);
          box-shadow: 0 0 6px rgba(255,0,0,0);
          text-align: left;
      }
    & /deep/ .el-radio.is-checked .el-radio__label {
          width: auto;
          height: 14px;
          line-height: 14px;
          margin: 0;
          padding: 0 0 0 10px;
          color: #fff;
          font-size: 14px;
          border-radius: 0;
          border-width: 0;
          border-style: solid;
          border-color: rgba(255, 255, 255, 0);
          background-color: rgba(255, 255, 255, 0);
          box-shadow: 0 0 6px rgba(255,0,0,0);
          text-align: left;
      }
    & /deep/ .el-radio__inner {
          width: 14px;
          height: 14px;
          margin: 0;
          padding: 0;
          border-radius: 100%;
          border-width: 1px;
          border-style: solid;
          border-color: rgba(130, 130, 130, 1);
          background-color: rgba(255, 255, 255, 1);
          box-shadow: 0 0 6px rgba(255,0,0,0);
      }
    & /deep/ .el-radio.is-checked .el-radio__inner {
          width: 14px;
          height: 14px;
          margin: 0;
          padding: 0;
          border-radius: 100%;
          border-width: 1px;
          border-style: solid;
          border-color:var(--publicSubColor);
          background-color: rgba(0, 73, 142, 1);
          box-shadow: 0 0 6px rgba(255,0,0,0);
      }

    .svg-container {
        padding: 6px 5px 6px 15px;
        color: #889aa4;
        vertical-align: middle;
        display: inline-block;
        position: absolute;
        left: 0;
        top: 0;
        z-index: 1;
        padding: 0;
        line-height: 40px;
        width: 30px;
        text-align: center;
    }

    .el-input {
        display: inline-block;
    // height: 40px;
        width: 100%;

    & /deep/ input {
          background: transparent;
          border: 0px;
          -webkit-appearance: none;
          padding: 0 15px 0 30px;
          color: #fff;
          height: 40px;

          width: 100%;
          height: 30px;
          line-height: 30px;
          margin: 0;
          padding: 0 30px;
          color: rgba(0, 0, 0, 1);
          font-size: 16px;
          border-radius: 0;
          border-width: 0;
          border-style: solid;
          border-color: rgba(0,0,0,0);
          background-color: rgba(0,0,0,0);
          box-shadow: 0 0 6px rgba(255,0,0,0);
      }
    }

    }


    }

    .center {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate3d(-50%,-50%,0);
    }

    .right {
        position: absolute;
        left: inherit;
        right: 0;
        top: 0;
    }

    .code {
    .el-form-item__content {
        position: relative;

    .getCodeBt {
        position: absolute;
        right: 0;
        top: 50%;
        transform: translate3d(0, -50%, 0);
        line-height: 40px;
        width: 100px;
        background-color: rgba(51,51,51,0.4);
        color: #fff;
        text-align: center;
        border-radius: 0 4px 4px 0;
        height: 40px;
        overflow: hidden;
        padding: 0;
        margin: 0;
        width: 100px;
        height: 30px;
        line-height: 30px;
        border-radius: 0;
        border-width: 0;
        border-style: solid;
        border-color: rgba(64, 158, 255, 1);
        background-color: rgba(51, 51, 51, 0.4);
        box-shadow: 0 0 6px rgba(255,0,0,0);

    span {
        padding: 0 5px;
        display: inline-block;
        font-size: 16px;
        font-weight: 600;
    }
    }

    .el-input {
    & /deep/ input {
          padding: 0 130px 0 30px;
      }
    }
    }
    }

    .setting {
    & /deep/ .el-form-item__content {
      // padding: 0 15px;
          box-sizing: border-box;
          line-height: 32px;
          height: 32px;
          font-size: 14px;
          color: #fff;
          margin: 0 !important;
          display: flex;

    .register {
    // float: left;
    // width: 50%;
        text-align: center;
    }

    .reset {
        float: right;
        width: 50%;
        text-align: right;
    }
    }
    }

    .style2 {
        padding-left: 30px;

    .svg-container {
        left: -30px !important;
    }

    .el-input {
    & /deep/ input {
          padding: 0 15px !important;
      }
    }
    }

    .code.style2, .code.style3 {
    .el-input {
    & /deep/ input {
          padding: 0 115px 0 15px;
      }
    }
    }

    .style3 {
    & /deep/ .el-form-item__label {
          padding-right: 6px;
          height: 30px;
          line-height: 30px;
      }

    .el-input {
    & /deep/ input {
          padding: 0 15px !important;
      }
    }
    }

    & /deep/ .el-form-item__label {
          width: 30px;
          height: 30px;
          line-height: 30px;
          margin: 0;
          padding: 0;
          color: #333;
          font-size: 14px;
          border-radius: 0;
          border-width: 0;
          border-style: solid;
          border-color: rgba(0,0,0,0);
          background-color: rgba(0,0,0,0);
          box-shadow: 0 0 6px rgba(0,0,0,0);
      }

    .role {
    & /deep/ .el-form-item__label {
          width: 56px !important;
          height: 30px;
          line-height: 30px;
          margin: 0 0 12px 35px;
          padding: 0;
          color: #333;
          font-size: 14px;
          border-radius: 0;
          border-width: 0;
          border-style: solid;
          border-color: var(--publicSubColor);
          background-color: rgba(255, 255, 255, 0);
          box-shadow: 0 0 6px rgba(255,0,0,0);
          text-align: left;
      }

    & /deep/ .el-radio {
          margin-right: 12px;
        color: var(--publicMainColor);
      }
    }
    }
</style>
