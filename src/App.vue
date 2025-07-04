<template>
  <div id="app">
    <!-- Header Component -->
    <Header 
      @open-login="openLoginModal"
      @open-register="openRegisterModal"
    />

    <!-- Auth Modal Component -->
    <AuthModal
      :show-login-modal="showLoginModal"
      :show-register-modal="showRegisterModal"
      :show-forgot-password-modal="showForgotPasswordModal"
      @close-modals="closeModals"
      @switch-to-register="switchToRegister"
      @switch-to-login="switchToLogin"
      @switch-to-forgot-password="switchToForgotPassword"
    />

    <!-- Main Content - Router View -->
    <main>
      <router-view />
    </main>

    <!-- Cart Sidebar Component -->
    <CartSidebar />

    <!-- Footer Component -->
    <Footer />

    <!-- Notification Container -->
    <NotificationContainer />
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import Header from './components/Header.vue'
import CartSidebar from './components/CartSidebar.vue'
import AuthModal from './components/AuthModal.vue'
import Footer from './components/Footer.vue'
import NotificationContainer from './components/NotificationContainer.vue'
import { useAuthStore } from './stores/auth'
import { useCartStore } from './stores/cart'

const authStore = useAuthStore()
const cartStore = useCartStore()

// Auth modals state
const showLoginModal = ref(false)
const showRegisterModal = ref(false)
const showForgotPasswordModal = ref(false)

// Inicializar carrito cuando el usuario esté autenticado
watch(() => authStore.isAuthenticated, async (isAuthenticated) => {
  if (isAuthenticated) {
    await cartStore.initializeCart()
  }
}, { immediate: true })

// Modal functions
const openLoginModal = () => {
  showLoginModal.value = true
  showRegisterModal.value = false
}

const openRegisterModal = () => {
  showRegisterModal.value = true
  showLoginModal.value = false
}

const closeModals = () => {
  showLoginModal.value = false
  showRegisterModal.value = false
  showForgotPasswordModal.value = false
}

const switchToRegister = () => {
  showLoginModal.value = false
  showRegisterModal.value = true
}

const switchToLogin = () => {
  showRegisterModal.value = false
  showForgotPasswordModal.value = false
  showLoginModal.value = true
}

const switchToForgotPassword = () => {
  showLoginModal.value = false
  showRegisterModal.value = false
  showForgotPasswordModal.value = true
}
</script>

<style>
/* Reset y estilos base */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: var(--font-family-primary);
  line-height: var(--line-height-normal);
  color: var(--color-gray-700);
  background-color: var(--color-white);
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1;
}
</style>