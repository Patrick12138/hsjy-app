<template>
    <div>
        <div class="userinfo" @click="setting">
            <div class="left">
                <div class="avatar">
                    <!--<img :src="img_prefix_url + userInfo.imageUrl" alt="">-->
                    <van-image
                          round
                          width="1.2rem"
                          height="1.2rem"
                          :src="img_prefix_url + userInfo.imageUrl"
                    />
                </div>
                <div class="info">
                    <h2>{{ userInfo.name }}</h2>
                    <p>{{ userInfo.email }}</p>
                </div>
            </div>
            <div class="right">
                <div class="setting">
                    <van-icon name="setting-o"/>
                </div>
            </div>
        </div>
        <van-divider/>
        <van-cell title="我的考试" @click="showExam" icon="records" isLink/>
        <van-cell title="我的收藏" @click="showCollect" icon="like-o" isLink/>
        <van-cell title="我的评论" @click="showComment" icon="like-o" isLink/>
        <van-divider/>
        <van-cell title="退出" @click="logout" icon="close" isLink/>
    </div>
</template>
<script>
    import {img_prefix_url} from '../../config/site';
    import {mapState} from 'vuex'
    export default {
        name: "index",
        data() {
            return {
                img_prefix_url,
            }
        },
        computed: {
            ...mapState(['userInfo']),
        },
        methods: {
            showComment() {
                this.$router.push('/show/comments');
            },
            showCollect() {
                this.$router.push('/show/collects');
            },
            showExam() {
                this.$router.push('/show/myexams');
            },
            logout() {
                localStorage.removeItem('token');
                this.$store.commit('logout');
                this.$router.push('/');
            },
            setting() {
                this.$router.push('/setting');
            }
        }
    }
</script>
<style lang="less" scoped>
    .userinfo {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 1rem 0.2rem;
        background-color: #fff;
        .left {
            display: flex;
            justify-content: space-between;
            align-items: center;
            .avatar {
                width: 1.2rem;
                height: 1.2rem;
                margin-right: 0.2rem;
                img {
                    width: 100%;
                }
            }
            .info {
                > h2 {
                    font-weight: 700;
                    margin-bottom: 0.2rem;
                }
                > p {
                    color: #454545;
                }
            }
        }
        .right {
            .setting {
                font-size: 0.6rem;
            }
        }
        .van-cell {
            border-bottom: 2px solid #c6e2ff;
        }
    }
</style>
