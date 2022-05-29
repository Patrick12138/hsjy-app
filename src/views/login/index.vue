<template>
    <div class="login">
        <div class="logo">
            <van-image
                    round
                    width="1rem"
                    height="1rem"
                    src="/imgs/hsjy_logo.png"
            />
        </div>
        <van-form @submit="onSubmit">
            <van-field
                    v-model="username"
                    name="username"
                    label="用户名"
                    autocomplete="off"
                    placeholder="请输入用户名或邮箱"
                    :rules="[{ required: true, message: '' }]"
            />
            <van-field
                    v-model="password"
                    name="password"
                    label="密码"
                    autocomplete="new-password"
                    type="password"
                    placeholder="请输入密码"
                    :rules="[{ required: true, message: '' }]"
            />
            <div class="subBtns">
                <van-button round block type="danger" native-type="submit">登录</van-button>
                <van-button @click="goRegister" class="no-border" round block type="primary" plain>注册</van-button>
            </div>
        </van-form>
    </div>
</template>

<script>
    import {login} from "../../api/user";
    import {mapState} from 'vuex'
    export default {
        name: 'login',
        data() {
            return {
                username: '',
                password: '',
            };
        },
        computed: {
            ...mapState(['userInfo', 'isLogin'])
        },
        watch: {
            '$store.state.isLogin': function (newVal, oldVal) {
                if (newVal) {
                    this.$router.push('/profile');
                }
            }
        },
        methods: {
            goRegister(e) {
                e.preventDefault();
                this.$router.push('/register');
            },
            async onSubmit(values) {
                let info = await login(values);
                if (info.data.statusCode == 200) {
                    this.$notify({type: 'success', message: '登录成功', duration: 500});
                    localStorage.setItem('token', info.data.data.token);
                    this.$store.commit('updateUserInfo', info.data.data.userInfo);
                    await this.$router.push('/profile');
                } else {
                    this.$notify({type: 'danger', message: '登录失败-' + info.data.message, duration: 500});
                }
            },
            onClickLeft() {
                this.$router.back();
            },
        },
    };
</script>

<style lang="less" scoped>
    .login {
        height: 100vh;
        background-color: #fff;
        .logo {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 3rem;
        }
        .subBtns {
            margin: 0.32rem 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
          .no-border {
                border: none;
                color: #999;
            }
            .van-button {
                width: 5rem;
                margin-bottom: 0.1rem;
            }
        }
    }
</style>
