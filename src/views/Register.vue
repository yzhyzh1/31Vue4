<template>
    <div class="register">
        <h3>用户注册</h3>
        <p>
            <Input prefix="md-book" v-model="username" placeholder="请输入用户名"
            @on-blur="validateUser()"/>
            <span>{{errormsg}}</span>
        </p>
        <p>
        <Input prefix="md-book" v-model="password" placeholder="请输入密码"
               @on-blur="validatePass()"/>
            <span>{{errmsg2}}</span>
        </p>
        <p>
            <Input prefix="md-book" v-model="passagain" placeholder="请确认密码"
                   @on-blur="validatePass2()"/>
            <span>{{errmsg3}}</span>
        </p>
        <p>
            <button type="primary" class="btn" @click="doRegister()">注册</button>
            <button type="primary" class="btn">重置</button>
        </p>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "Register",
        data() {
            return {
                username: '',
                errormsg: '',
                password: '',
                passagain: '',
                errmsg2: '',
                errmsg3: '',

            }
        },
        methods: {
            validateUser() {
                let valid = true;
                if (this.username == '') {
                    this.errormsg = '用户名不能为空';
                    valid = false;
                } else {
                    axios.get('http://localhost:8080/api/isexist', {params: {username: this.username}})
                        .then((res) => {
                            if (res.data) {

                                this.errormsg = '用户名已存在'
                                valid = false;
                            }
                        })
                }
                if (valid == true)
                    this.errormsg = '';
                return valid;
            },
            validatePass() {
                let valid = true;
                if (this.password == '') {
                    this.errmsg2 = '密码不能为空';
                    valid = false;
                } else {
                    var reg = new RegExp(/^\w{6,16}$/)
                    valid = reg.test(this.password);
                    if (!valid)
                        this.errmsg2 = '密码需为6-16位'
                }
                if (valid)
                    this.errmsg2 = '';
                return valid;
            },
            validatePass2() {
                let valid = true;
                if (this.passagain == "") {
                    valid = false;
                    this.errmsg3 = '请输入确认密码';
                } else {
                    if (this.passagain != this.password) {
                        valid = false;
                        this.errmsg3 = '两次密码不一致'
                    }
                }
                if (valid)
                    this.errmsg3 = '';
                return valid;
            },
            validate() {
                let valedate1 = this.validateUser();
                let valedate2 = this.validatePass();
                let valedate3 = this.validatePass2();
                return validate1 && valedate2 && valedate3;
            },

            doRegister() {
                axios.post('http://localhost:8080/api/register',
                    {username: this.username, password: this.password})
                    .then((res) => {
                        if (res.data) {
                            alert("注册成功，跳转登录界面")
                            this.$router.push("/Login");
                        }
                    })
            }
        }
        }
</script>

<style scoped>
    .register{width:480px; margin:30px auto; border:1px solid #2d8cf0; padding:20px 5px 10px 30px; border-radius:5px; text-align:left;}
    .register .btn{width:100px; margin-right:20px;}
    .register p,.register h3{margin-bottom:15px;}
</style>