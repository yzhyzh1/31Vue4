<template>
    <div>
        <router-link to="/login">登录</router-link>
        <router-link to="/register">注册</router-link>
        <h1>欢迎进入我们的MALL</h1>



    </div>
</template>

<script>
    export default {
        name: "Index",
        mounted() {
            if (this.$cookies.isKey('username')){
                let loginuser=this.$cookies.get('username')
                this.$store.commit('setLoginUser',loginuser);
                this.$router.push('/ProductList')
            }
        }
    }
</script>

<style scoped>

</style>