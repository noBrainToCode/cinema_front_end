<template>
    <div id="content-area">
        <div id="avatar">{{ user.first_letter }}</div>
        <div @click="logout()">登出</div>
    </div>
    <!-- <span id="username">fd</span> -->
    <!-- <span>d</span> -->
</template>

<script setup>
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

    async function fetchUser() {
        try {
            const response = await axios.post("/user/getUserInfo");
            if (response.data.code == 200) {
                const data = response.data.data;
                user.id = data.userId;
                user.name = data.userName;
                user.age = data.age;
                user.gender = data.gender;
                user.first_letter = user.name[0].toUpperCase();
            } else {
                // 判定为没有登录，转到登录界面
                router.push("/login");
            }
        } catch (error) {
            console.log(error);
        }
    }

    function logout() {
        const token = localStorage.getItem("jwt");
        try {
            // 登出
            axios.post("/doLogout", token);
            // 跳到主页
            router.push("/")
        } catch (error) {
            console.log(error);
        }
    }

    fetchUser();
</script>

<style scoped>
    #content-area {
        display: flex;
        width: 70%;
        margin: 100px auto;
        background-color: aqua;
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
        border-radius: 25px;
        font-weight: bold;
    }
</style>