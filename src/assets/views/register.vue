<template>
    <div class="wrapper-login">
        <text class="text">注册账户</text>
        <input type="tel" autofocus="true" placeholder="手机号" class="input-style" v-model="phone">
        <div class="validate">
            <input type="text" placeholder="验证码" class="input-style validate-input" v-model="code">
            <text class="validate-btn" @click="validate()">获取验证码</text>
        </div>
        <input type="password" placeholder="密码" class="input-style" v-model="password">
        
        <text class="login-btn" @click="signIn()">注册</text>
    </div>
</template>
<style scoped>
    .wrapper-login{
        margin-top: 50px;
        margin-left: 125px;
        width: 500px;
        align-items: center;
    }
    .text{
        font-size: 36px;
        color: #333333;
    }
    .input-style{
        width: 400px;
        height: 60px;
        font-size: 34px;
        color: #666666;
        line-height: 60px;
        border-width: 1px;
        border-color: #cccccc;
        padding-top: 10px;
        padding-bottom: 10px;
        padding-left: 20px;
        padding-right: 20px;
        margin-top: 40px;
    }
    .validate{
        flex-direction: row;
        align-items: center;
    }
    .validate-input{
        width:250px;
    }
    .validate-btn{
        width: 150px;
        border-width: 1px;
        border-left-width: 0px;
        border-color: #cccccc;
        font-size: 28px;
        color: #333333;
        text-align: center;
        height: 60px;
        line-height: 60px;
        margin-top:40px;
    }
    .login-btn{
        width: 400px;
        text-align: center;
        font-size: 34px;
        color: #ffffff;
        background-color: green;
        height: 60px;
        line-height: 60px;
        margin-top:40px;
        border-radius: 10px;
    }
    .forget-psw{
        margin-top: 40px;
        color: #666666;
        font-size: 32px;
        width: 400px;
        text-align: center;
    }
</style>

<script>
    import util from '../util';
    var modal = weex.requireModule('modal');
    var stream = weex.requireModule('stream');
    var storage = weex.requireModule('storage');
    export default {
        data () {
            return {
                phone: '',
                code: '',
                password: ''
            }
        },
        methods: {
            signIn(){
                var _self = this;
                var ph = this.phone,
                    cd = this.code,
                    pw = this.password;
                if(!ph.length){
                    modal.toast({
                        message: "请输入手机号码",
                        duration: 1
                    });
                    return false;
                }
                if(!cd.length){
                    modal.toast({
                        message: "请输入验证码",
                        duration: 1
                    })
                    return false;
                }
                if(!pw.length){
                    modal.toast({
                        message: "请输入密码",
                        duration: 1
                    })
                    return false;
                }
                stream.fetch({
                    method: 'POST',
                    type: 'json',
                    headers:{
                        "Content-Type":"application/json"
                    },
                    body:JSON.stringify({"phone":ph,"code":cd,"password":pw}),
                    url: 'http://www.imbawin.com/app/register'
                }, function(res){
                    if(res.data.code == 200){
                        let result = res.data.result;
                        modal.toast({
                            message: res.data.message,
                            duration: 3
                        })
                    }else{
                        modal.toast({
                            message: res.data.message,
                            duration: 3
                        })
                    }
                    
                })
            },
            validate(){
                var ph = this.phone;
                if(!ph.length){
                    modal.toast({
                        message: "请输入手机号码",
                        duration: 1
                    });
                    return false;
                }
                stream.fetch({
                    method: 'POST',
                    type: 'json',
                    url: 'http://www.imbawin.com/app/sendCode?phone='+ph
                }, function(res){
                    if(res.data.code == 200){
                        let result = res.data.result;
                        modal.toast({
                            message: res.data.message,
                            duration: 3
                        })
                    }else{
                        modal.toast({
                            message: res.data.message,
                            duration: 3
                        })
                    }
                    
                })
            }
        }
    }
</script>