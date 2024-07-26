<!-- TicketModal.vue -->
<template>
    <div class="modal-overlay">
      <div class="modal-content">
        <h2>确认订单信息</h2>
        <div>
          <label for="ticket-count">票数:</label>
          <button @click="decreaseCount">-</button>
          <input id="ticket-count" type="number" v-model="order.ticketCount" readonly />
          <button @click="increaseCount">+</button>
        </div>
        <div>
          <label for="hall-number">影厅号:</label>
          <select v-model="order.hallNumber">
            <option value="A">A</option>
            <option value="B">B</option>
          </select>
        </div>
        <div>
          <label for="seat-number">座位号:</label>
          <select v-model="order.seatNumber">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
          </select>
        </div>
        <div>
          <label for="show-time">放映时间:</label>
          <input id="show-time" type="datetime-local" v-model="order.showTime" />
        </div>
        <div>
          <!-- <label for="ticket-price">票价:</label>
          <input id="ticket-price" type="number" v-model="ticketPrice" /> -->
          <span>票价：{{ order.ticketPrice }}</span>
        </div>
        <button @click="confirmPurchase">确认购买</button>
        <button @click="closeModal">取消</button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { reactive, defineProps, defineEmits } from 'vue';
  import axios from 'axios';

  const props = defineProps({
    modelValue: {
      type: Boolean,
      default: false
    },
    cinemaId: {
      type: String,
      required: true
    },
    movieId: {
      type: String,
      required: true
    },
    userId: {
      type: String,
      required: true
    },
    ticketPrice: {
      type: Number,
      required: true
    }
  });
  
  const emit = defineEmits(['update:modelValue']);

  const singleTicketPrice = props.ticketPrice;

  const order = reactive({
    ticketCount: 1,
    hallNumber: '',
    seatNumber: '',
    showTime: '',
    userId: props.userId,
    movieId: props.movieId,
    cinemaId: props.cinemaId,
    ticketPrice: singleTicketPrice
  })
  
  function increaseCount() {
    order.ticketCount++;
    order.ticketPrice = order.ticketCount * singleTicketPrice;
  }
  
  function decreaseCount() {
    if (order.ticketCount > 1) {
      order.ticketCount--;
      order.ticketPrice = order.ticketCount * singleTicketPrice;
    }
  }
  
  function closeModal() {
    emit('update:modelValue', false);
  }
  
  async function confirmPurchase() {
    if (order.hallNumber == '' || order.seatNumber == '' || order.showTime == '') {
      alert("请填完必要信息！");
      return;
    }
    order.showTime = new Date(order.showTime).toISOString(); // 将日期转换为字符串类型
    order.seatNumber = order.hallNumber + order.seatNumber;
    order.hallNumber = "Hall " + order.hallNumber;
    console.log(order);
  
    // try {
    //   const response = await axios.post('/orders', { params: order });
    //   console.log('Order created:', response.data);
    //   closeModal();
    // } catch (error) {
    //   console.error('Error creating order:', error);
    // }
  }
  </script>
  

  <style scoped>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .modal-content {
    background: white;
    padding: 20px;
    border-radius: 5px;
  }
  </style>
  