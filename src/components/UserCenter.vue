<template>
    <movie-header-backup></movie-header-backup>
    <div id="intro-area">
        <div id="intro-left">
            <div id="avatar">{{ user.first_letter }}</div>
            <div @click="logout()" id="logout-btn">登出</div>
        </div>
            <div id="intro-right">
                <div class="intro-block">
                <span>用户名</span>
                <span id="username">{{ user.name }}</span>
            </div>
            <div class="intro-block">
                <span>性别</span>
                <span id="gender">{{ user.gender }}</span>
            </div>
            <div class="intro-block">
                <span>年龄</span>
                <span id="age">{{ user.age }}</span>
            </div>
        </div>

    </div>
    <div id="order-area">
        <span>订单</span>
        <div v-for="order in orders.arr">
            <order-card v-bind="order"></order-card>
        </div>
    </div>  
    <!-- <span id="username">fd</span> -->
    <!-- <span>d</span> -->
</template>

<script setup>
    import MovieHeaderBackup from './MovieHeaderBackup.vue';
    import OrderCard from './OrderCard.vue';
    import { reactive } from 'vue';
    import axios from 'axios';
    import router from '..';

    const user = reactive({
        id: 0,
        name: '',
        gender: 'M',
        age: 0,
        first_letter: ''
    });

    const orders = reactive({
        arr: []
    });

    async function fetchUser() {
        try {
            const response = await axios.post("/user/getUserInfo");
            if (response.data.code == 200) {
                const data = response.data.data;
                user.id = data.userId;
                user.name = data.userName;
                user.age = data.age;
                if (data.gender == 'M') {
                    user.gender = '男';
                } else {
                    user.gender = '女';
                }
                user.first_letter = user.name[0].toUpperCase();
                fetchOrder();
            } else {
                // 判定为没有登录，转到登录界面
                router.push("/login");
            }
        } catch (error) {
            console.log(error);
        }
    }

    async function fetchOrder() {
        try {
            const response = await axios.get(`/orders/${user.id}`);
            if (response.data.code == 200) {
                orders.arr = response.data.data;
            }
        } catch(error) {
            console.log(error);
        }
    }

    function logout() {
        const token = localStorage.getItem("jwt");
        try {
            // 登出
            axios.post("/doLogout", token);
            localStorage.setItem("jwt", "");
            // 跳到主页
            router.push("/")
        } catch (error) {
            console.log(error);
        }
    }

    fetchUser();
</script>

<style scoped>
    #intro-area {
        display: flex;
        width: 70%;
        margin: 100px auto;
        justify-content: center;
    }

    #intro-left {
        width: 40%;
    }

    #avatar {
        width: 80px;
        height: 80px;
        font-size: 4em;
        background: lightpink;
        padding: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 40px;
        font-weight: bold;
    }

    #username {
        font-size: 2em;
    }

    .intro-block {
        width: 100%;
    }

    .intro-block span {
        margin-right: 30px;
    }

    #logout-btn {
        width: fit-content;
    }

    #logout-btn:hover {
        cursor: pointer;
    }

    #order-area {
        width: 70%;
        margin: 10vh auto;
    }
</style>