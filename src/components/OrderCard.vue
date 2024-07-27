<template>
    <div class="container">
        <span>订单号{{ props.orderId }}</span>
        <span>电影名{{ props.movieName }}</span>
    </div>
</template>

<script setup>
    const props = defineProps({
        orderId: Number,
        movieName: String,
        cinemaName: String,
        hallNumber: String,
        seatNumber: String,
        ticketCount: Number,
        orderPrice: Number,
        showTime: String,
        orderTime: String,
        ticketPrice: Number
    })
</script>

<style scoped>
    .container {
        border-radius: 10px;
        box-shadow: 0px 3px 10px 3px black;
    }
</style>