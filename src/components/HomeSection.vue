<template>
  <section class="home-section">
    <div class="hero">
      <div class="hero-animation">
        <div class="floating-prize">🎁</div>
        <div class="floating-prize">💎</div>
        <div class="floating-prize">🏆</div>
      </div>
      
      <div class="hero-content">
        <h1 class="hero-title">
          <span class="highlight">SUPER PROMOÇÃO</span>
          <span class="year">2025</span>
        </h1>
        <p class="hero-subtitle">
          A maior campanha promocional do ano!<br>
          <strong>Participe e concorra a prêmios incríveis!</strong>
        </p>
        
        <div class="countdown-container">
          <h3>⏰ Promoção válida até:</h3>
          <div class="countdown">
            <div class="countdown-item">
              <span class="countdown-number">{{ countdown.days }}</span>
              <span class="countdown-label">Dias</span>
            </div>
            <div class="countdown-item">
              <span class="countdown-number">{{ countdown.hours }}</span>
              <span class="countdown-label">Horas</span>
            </div>
            <div class="countdown-item">
              <span class="countdown-number">{{ countdown.minutes }}</span>
              <span class="countdown-label">Min</span>
            </div>
            <div class="countdown-item">
              <span class="countdown-number">{{ countdown.seconds }}</span>
              <span class="countdown-label">Seg</span>
            </div>
          </div>
        </div>
        
        <div class="hero-actions">
          <button class="btn btn-primary pulse-animation">
            🎯 PARTICIPAR AGORA
          </button>
          <button class="btn btn-secondary">
            📋 Ver Regulamento
          </button>
        </div>
      </div>
    </div>
    
    <div class="campaign-highlights">
      <div class="highlight-grid">
        <div class="highlight-card">
          <div class="highlight-icon rotating">🎁</div>
          <h3>Mais de 1000</h3>
          <p>Prêmios disponíveis</p>
        </div>
        <div class="highlight-card">
          <div class="highlight-icon rotating">🏪</div>
          <h3>500+ Lojas</h3>
          <p>Participantes em todo Brasil</p>
        </div>
        <div class="highlight-card">
          <div class="highlight-icon rotating">🏆</div>
          <h3>R$ 1 Milhão</h3>
          <p>Em prêmios totais</p>
        </div>
        <div class="highlight-card">
          <div class="highlight-icon rotating">📱</div>
          <h3>Fácil de</h3>
          <p>Participar pelo app</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

interface CountdownTime {
  days: number
  hours: number
  minutes: number
  seconds: number
}

const countdown = ref<CountdownTime>({
  days: 0,
  hours: 0,
  minutes: 0,
  seconds: 0
})

let countdownInterval: number

const updateCountdown = () => {
  const endDate = new Date()
  endDate.setDate(endDate.getDate() + 30)
  endDate.setHours(23, 59, 59, 999)
  
  const now = new Date().getTime()
  const distance = endDate.getTime() - now
  
  if (distance > 0) {
    countdown.value = {
      days: Math.floor(distance / (1000 * 60 * 60 * 24)),
      hours: Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)),
      minutes: Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60)),
      seconds: Math.floor((distance % (1000 * 60)) / 1000)
    }
  } else {
    // promoção expirada
    countdown.value = { days: 0, hours: 0, minutes: 0, seconds: 0 }
  }
}

onMounted(() => {
  updateCountdown()
  countdownInterval = setInterval(updateCountdown, 1000)
})

onUnmounted(() => {
  if (countdownInterval) {
    clearInterval(countdownInterval)
  }
})
</script>

<style scoped>
.home-section {
  padding: 0;
  overflow: hidden;
}

.hero {
  position: relative;
  text-align: center;
  padding: 6rem 2rem 4rem;
  background: linear-gradient(135deg, #434343 0%, #000000 100%);
  background-size: 400% 400%;
  animation: gradientShift 8s ease-in-out infinite;
  color: white;
  min-height: 80vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.hero-animation {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  overflow: hidden;
}

.floating-prize {
  position: absolute;
  font-size: 3rem;
  animation: float 6s ease-in-out infinite;
  opacity: 0.7;
}

.floating-prize:nth-child(1) {
  top: 10%;
  left: 10%;
  animation-delay: 0s;
}

.floating-prize:nth-child(2) {
  top: 20%;
  right: 15%;
  animation-delay: 2s;
}

.floating-prize:nth-child(3) {
  bottom: 30%;
  left: 20%;
  animation-delay: 4s;
}

@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  33% { transform: translateY(-20px) rotate(5deg); }
  66% { transform: translateY(10px) rotate(-5deg); }
}

.hero-content {
  position: relative;
  z-index: 2;
  max-width: 800px;
}

.hero-title {
  font-size: 4rem;
  margin-bottom: 1.5rem;
  font-weight: 900;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.highlight {
  background: linear-gradient(45deg, var(--warning), #fff700);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: shimmer 3s ease-in-out infinite;
}

.year {
  font-size: 5rem;
  color: white;
  text-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
}

@keyframes shimmer {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.8; }
}

.hero-subtitle {
  font-size: 1.4rem;
  margin-bottom: 3rem;
  line-height: 1.6;
  text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
}

.countdown-container {
  margin: 3rem 0;
  padding: 2rem;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 1rem;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.countdown-container h3 {
  margin-bottom: 1.5rem;
  font-size: 1.2rem;
}

.countdown {
  display: flex;
  justify-content: center;
  gap: 2rem;
  flex-wrap: wrap;
}

.countdown-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 0.5rem;
  min-width: 80px;
}

.countdown-number {
  font-size: 2.5rem;
  font-weight: 900;
  line-height: 1;
  margin-bottom: 0.5rem;
}

.countdown-label {
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.hero-actions {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  flex-wrap: wrap;
  margin-top: 3rem;
}

.btn {
  padding: 1rem 2.5rem;
  border: none;
  border-radius: 50px;
  font-weight: 700;
  font-size: 1.1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.btn-primary {
  background: var(--gradient-secondary);
  color: white;
  box-shadow: 0 8px 30px rgba(116, 185, 255, 0.4);
}

.btn-secondary {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: 2px solid rgba(255, 255, 255, 0.5);
  backdrop-filter: blur(10px);
}

.btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.2);
}

.pulse-animation {
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% { box-shadow: 0 8px 30px rgba(255, 107, 107, 0.3); }
  50% { box-shadow: 0 8px 30px rgba(255, 107, 107, 0.6), 0 0 0 10px rgba(255, 107, 107, 0.1); }
  100% { box-shadow: 0 8px 30px rgba(255, 107, 107, 0.3); }
}

.campaign-highlights {
  padding: 4rem 2rem;
  background: white;
}

.highlight-grid {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.highlight-card {
  text-align: center;
  padding: 2.5rem 2rem;
  border-radius: 1.5rem;
  background: var(--gradient-accent);
  color: white;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.highlight-card::before {
  content: '';
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.1), transparent);
  transform: rotate(45deg);
  transition: all 0.6s ease;
  opacity: 0;
}

.highlight-card:hover::before {
  opacity: 1;
  transform: rotate(45deg) translate(50%, 50%);
}

.highlight-card:hover {
  transform: translateY(-10px) scale(1.02);
  box-shadow: 0 20px 50px rgba(102, 126, 234, 0.3);
}

.highlight-icon {
  font-size: 3.5rem;
  margin-bottom: 1.5rem;
  display: inline-block;
}

.rotating {
  animation: rotate 4s linear infinite;
}

@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

.highlight-card h3 {
  font-size: 2rem;
  margin-bottom: 0.5rem;
  font-weight: 900;
}

.highlight-card p {
  font-size: 1.1rem;
  opacity: 0.9;
}

@media (max-width: 768px) {
  .hero-title {
    font-size: 2.5rem;
  }
  
  .year {
    font-size: 3.5rem;
  }
  
  .hero-subtitle {
    font-size: 1.2rem;
  }
  
  .countdown {
    gap: 1rem;
  }
  
  .countdown-number {
    font-size: 2rem;
  }
  
  .btn {
    padding: 0.8rem 2rem;
    font-size: 1rem;
  }
}
</style>