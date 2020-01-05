<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="menu-tab">
                <li v-for="(item,index) in menuTab" :key="item.index" :class="{'current': index==currentIndex }" @click="toggleMenu(index)">{{ item.txt }}</li>
            </ul>
            <!-- form -->
            <el-form :model="ruleForm"  :rules="rules" ref="ruleForm"  class="loginForm" size="medium">
                <el-form-item prop="username" class="item-form">
                    <label>邮箱</label>
                    <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item prop="password" class="item-form">
                    <label>密码</label>
                    <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item prop="code" class="item-form">
                    <label>验证码</label>
                    <el-row :gutter="20">
                        <el-col :span="14">
                            <el-input v-model.number="ruleForm.code"></el-input>
                        </el-col>
                        <el-col :span="10">
                            <el-button type="success" class="block code-btn">获取验证码</el-button>
                        </el-col>
                    </el-row>
                </el-form-item>
                <el-form-item class="item-form">
                    <el-button type="danger" @click="submitForm('ruleForm')" class="block login-btn">提交</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    export default {
        name: "login",
        data(){
            var checkCode = (rule, value, callback) => {
                if (!value) {
                    return callback(new Error('验证码不能为空'));
                }
                setTimeout(() => {
                    if (!Number.isInteger(value)) {
                        callback(new Error('请输入数字值'));
                    } else {
                        if (value < 18) {
                            callback(new Error('必须年满18岁'));
                        } else {
                            callback();
                        }
                    }
                }, 1000);
            };
            var validateUsername = (rule, value, callback) => {
                let reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
                if (value === '') {
                    callback(new Error('请输入用户名'));
                } else {
                    if (!reg.test(value)) {
                        callback(new Error("用户名格式错误"));
                    }
                    callback();
                }
            };
            var validatePassword = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入密码'));
                } else {
                    callback();
                }
            };
            return {
                ruleForm: {
                    password: '',
                    username: '',
                    code: ''
                },
                rules: {
                    password: [
                        { validator: validatePassword, trigger: 'blur' },
                        { min: 6,trigger: 'blur', message: "密码必须大于6"}
                    ],
                    username: [
                        { validator: validateUsername, trigger: 'blur' }
                    ],
                    code: [
                        { validator: checkCode, trigger: 'blur' }
                    ]
                },
                menuTab: [
                    {txt: "登录"},
                    {txt: "注册"}
                ],
                currentIndex: 0
            }
        },
        created(){

        },
        mounted(){

        },
        methods:{
            toggleMenu(index){
               this.currentIndex = index;
            },
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
        }
    };
</script>
<style scoped lang="scss">
#login{
    height: 100vh;
    background: #344a5f;
}
.login-wrap{
    width: 330px;
    margin: auto;
}
.menu-tab {
    text-align: center;

    li {
        display: inline-block;
        width: 80px;
        line-height: 36px;
        font-size: 14px;
        color: #fff;
        border-radius: 2px;
        cursor: pointer;
    }

    .current {
        background-color: rgba(0, 0, 0, .1);
    }
}
.loginForm {
    margin-top: 29px;
    label {
        margin-bottom: 3px;
        display: block;
        font-size: 14px;
        color: #fff;
    }
    .item-form {
        margin-bottom: 13px;
    }
    .block {
        display: block;
        width: 100%;
    }
    .login-btn {
        margin-top: 29px;
    }
}


</style>
