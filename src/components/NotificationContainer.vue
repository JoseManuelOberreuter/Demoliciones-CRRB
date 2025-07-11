<template>
  <Teleport to="body">
    <div class="notification-container">
      <TransitionGroup name="notification" tag="div">
        <div 
          v-for="notification in notifications" 
          :key="notification.id"
          :class="['notification', `notification--${notification.type}`]"
          @click="removeNotification(notification.id)"
        >
          <div class="notification__icon">
            <span v-if="notification.type === 'success'">✅</span>
            <span v-else-if="notification.type === 'error'">❌</span>
            <span v-else-if="notification.type === 'warning'">⚠️</span>
            <span v-else-if="notification.type === 'info'">ℹ️</span>
          </div>
          
          <div class="notification__content">
            <p class="notification__message">{{ notification.message }}</p>
          </div>
          
          <button 
            class="notification__close"
            @click.stop="removeNotification(notification.id)"
            aria-label="Cerrar notificación"
          >
            ×
          </button>
        </div>
      </TransitionGroup>
    </div>
  </Teleport>
</template>

<script setup>
import { storeToRefs } from 'pinia'
import { useNotificationStore } from '../stores/notifications'

const notificationStore = useNotificationStore()
const { notifications } = storeToRefs(notificationStore)
const { removeNotification } = notificationStore
</script>

<style scoped>
.notification-container {
  position: fixed;
  top: 80px; /* Debajo del header */
  right: 20px;
  z-index: 9999;
  pointer-events: none;
}

.notification {
  display: flex;
  align-items: flex-start;
  gap: var(--spacing-md);
  background: var(--color-white);
  border-radius: var(--border-radius-lg);
  padding: var(--spacing-lg);
  margin-bottom: var(--spacing-md);
  box-shadow: var(--shadow-lg);
  border-left: 4px solid;
  min-width: 320px;
  max-width: 420px;
  pointer-events: auto;
  cursor: pointer;
  transition: all var(--transition-normal);
}

.notification:hover {
  transform: translateX(-5px);
  box-shadow: var(--shadow-xl);
}

/* Tipos de notificación */
.notification--success {
  border-left-color: #10b981;
  background: linear-gradient(135deg, #f0fdf4 0%, #ffffff 100%);
}

.notification--error {
  border-left-color: #ef4444;
  background: linear-gradient(135deg, #fef2f2 0%, #ffffff 100%);
}

.notification--warning {
  border-left-color: #f59e0b;
  background: linear-gradient(135deg, #fffbeb 0%, #ffffff 100%);
}

.notification--info {
  border-left-color: #3b82f6;
  background: linear-gradient(135deg, #eff6ff 0%, #ffffff 100%);
}

.notification__icon {
  font-size: var(--font-size-lg);
  flex-shrink: 0;
}

.notification__content {
  flex: 1;
}

.notification__message {
  margin: 0;
  color: var(--color-gray-800);
  font-size: var(--font-size-sm);
  line-height: var(--line-height-relaxed);
  white-space: pre-wrap;
}

.notification__close {
  background: none;
  border: none;
  font-size: var(--font-size-xl);
  color: var(--color-gray-400);
  cursor: pointer;
  padding: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: var(--border-radius-full);
  transition: all var(--transition-normal);
  flex-shrink: 0;
}

.notification__close:hover {
  background: var(--color-gray-100);
  color: var(--color-gray-600);
}

/* Animaciones */
.notification-enter-active {
  transition: all 0.3s ease-out;
}

.notification-leave-active {
  transition: all 0.3s ease-in;
}

.notification-enter-from {
  opacity: 0;
  transform: translateX(100%);
}

.notification-leave-to {
  opacity: 0;
  transform: translateX(100%);
}

.notification-move {
  transition: transform 0.3s ease;
}

/* Responsive */
@media (max-width: 768px) {
  .notification-container {
    top: 70px;
    right: 10px;
    left: 10px;
  }

  .notification {
    min-width: auto;
    max-width: none;
  }
}
</style> 