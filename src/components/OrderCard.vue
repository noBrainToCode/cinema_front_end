<template>
    <div class="card-container">
        <span>订单号：{{ props.orderId }}</span>
        <span>电影名：{{ props.movieName }}</span>
        <span>影厅号：{{ props.hallNumber }}</span>
        <span>座位号：{{ props.seatNumber }}</span>
        <span>订单数量：{{ props.ticketCount }}</span>
        <span>价格：{{ props.orderPrice }}</span>
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
    .card-container {
        border-radius: 10px;
        box-shadow: 0px 3px 5px 1px rgb(55, 55, 55);
        padding-top: 20px;
        padding-bottom: 20px;
    }

    .card-container span {
        margin-left: 15px;
        margin-right: 15px;
    }
</style>