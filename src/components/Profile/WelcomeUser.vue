<script setup>
import { ref, onMounted } from 'vue'
import { getCurrentUser } from '@/services/apiAuth.js'
import UserSettings from '@/components/Profile/UserSettings.vue'
import OrdersHistory from '@/components/Profile/OrdersHistory.vue'

const user = ref(null)
const loading = ref(true)

const showComponent = ref('welcome')

const goToSettings = () => {
  showComponent.value = 'settings'
}

const goToOrderHistory = () => {
  showComponent.value = 'orderHistory'
}

const goToWelcome = () => {
  showComponent.value = 'welcome'
}

onMounted(async () => {
  user.value = await getCurrentUser()
  loading.value = false
})
</script>

<template>
  <div v-if="showComponent === 'welcome'">
    <div v-if="loading" class="text-2xl"></div>
    <div v-else-if="user" class="animate-slide-in text-2xl">
      <button
        v-hoverable
        v-motion="{
          enter: { scale: 1, x: 0 },
          hovered: { scale: 1.2, x: -20 }
        }"
        @click="goToOrderHistory"
        class="mr-10"
      >
        Order History <span class="text-4xl">←</span>
      </button>
      Welcome back, {{ user.email }}!
      <button
        v-hoverable
        v-motion="{
          enter: { scale: 1, x: 0 },
          hovered: { scale: 1.2, x: 20 }
        }"
        @click="goToSettings"
        class="ml-10"
      >
        <span class="text-4xl">→</span> Settings
      </button>
    </div>
  </div>
  <UserSettings v-else-if="showComponent === 'settings'" @back="goToWelcome" />
  <OrdersHistory v-else-if="showComponent === 'orderHistory'" @back="goToWelcome" />
</template>

<style scoped>
@keyframes slide-in {
  0% {
    transform: translateX(-100%);
    opacity: 0;
  }
  100% {
    transform: translateX(0);
    opacity: 1;
  }
}

.animate-slide-in {
  animation: slide-in 1s ease-out;
  animation-fill-mode: forwards;
}
</style>
