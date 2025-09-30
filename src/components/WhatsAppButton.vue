<template>
  <button 
    class="whatsapp-button" 
    @click="openWhatsApp"
    aria-label="Fale conosco no WhatsApp"
    title="Fale conosco no WhatsApp"
  >
    <span class="whatsapp-icon">💬</span>
  </button>
</template>

<script setup lang="ts">
const openWhatsApp = () => {
  const phoneNumber = '5511987654321'
  const message = encodeURIComponent('Olá! 👋 Gostaria de saber mais informações sobre a Super Promoção 2025. Poderia me ajudar?')
  const whatsappUrl = `https://wa.me/${phoneNumber}?text=${message}`
  window.open(whatsappUrl, '_blank')
}
</script>

<style scoped>
.whatsapp-button {
  position: fixed;
  bottom: 90px;
  right: 2rem;
  width: 60px;
  height: 60px;
  background: #25D366;
  color: white;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  z-index: 999;
  box-shadow: 0 10px 30px rgba(37, 211, 102, 0.4);
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.8rem;
  backdrop-filter: blur(10px);
}

.whatsapp-button:hover {
  transform: translateY(-5px) scale(1.1);
  box-shadow: 0 15px 40px rgba(37, 211, 102, 0.4);
  background: #128C7E;
}

.whatsapp-button:active {
  transform: translateY(-2px) scale(1.05);
}

.whatsapp-icon {
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-3px);
  }
  60% {
    transform: translateY(-1px);
  }
}

@media (max-width: 768px) {
  .whatsapp-button {
    bottom: 1rem;
    right: 1rem;
    width: 50px;
    height: 50px;
    font-size: 1.4rem;
  }
}
</style>