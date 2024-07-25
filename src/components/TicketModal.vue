<!-- TicketModal.vue -->
<template>
    <div class="modal-overlay">
      <div class="modal-content">
        <h2>确认订单信息</h2>
        <div>
          <label for="ticket-count">票数:</label>
          <button @click="decreaseCount">-</button>
          <input id="ticket-count" type="number" v-model="ticketCount" readonly />
          <button @click="increaseCount">+</button>
        </div>
        <div>
          <label for="hall-number">影厅号:</label>
          <input id="hall-number" v-model="hallNumber" />
        </div>
        <div>
          <label for="seat-number">座位号:</label>
          <input id="seat-number" v-model="seatNumber" />
        </div>
        <div>
          <label for="show-time">放映时间:</label>
          <input id="show-time" type="datetime-local" v-model="showTime" />
        </div>
        <div>
          <!-- <label for="ticket-price">票价:</label>
          <input id="ticket-price" type="number" v-model="ticketPrice" /> -->
          <span>票价：{{ props.ticketPrice }}</span>
        </div>
        <button @click="confirmPurchase">确认购买</button>
        <button @click="closeModal">取消</button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, defineProps, defineEmits } from 'vue';
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

  const ticketCount = ref(1);
  const hallNumber = ref('');
  const seatNumber = ref('');
  const showTime = ref('');
  const ticketPrice = ref(0);
  
  function increaseCount() {
    ticketCount.value++;
  }
  
  function decreaseCount() {
    if (ticketCount.value > 1) {
      ticketCount.value--;
    }
  }
  
  function closeModal() {
    emit('update:modelValue', false);
  }
  
  async function confirmPurchase() {
    const orderData = {
      userId: props.userId,
      movieId: props.movieId,
      cinemaId: props.cinemaId,
      hallNumber: hallNumber.value,
      seatNumber: seatNumber.value,
      showTime: new Date(showTime.value).toISOString(), // 将日期转换为字符串类型
      ticketCount: ticketCount.value,
      ticketPrice: ticketPrice.value
    };
  
    try {
      const response = await axios.post('/orders', { params: orderData });
      console.log('Order created:', response.data);
      closeModal();
    } catch (error) {
      console.error('Error creating order:', error);
    }
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
  