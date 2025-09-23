<template>
  <header class="app-header" :class="{ scrolled: isScrolled }">
    <nav class="navbar">
      <div class="nav-brand">
        <h1>🎉 SUPER PROMOÇÃO 2025</h1>
      </div>
      
      <ul class="nav-menu" :class="{ 'nav-menu-open': mobileMenuOpen }">
        <li class="nav-item">
          <a href="#home" class="nav-link" 
             :class="{ active: activeSection === 'home' }"
             @click="navigateToSection('home')">Home</a>
        </li>
        <li class="nav-item">
          <a href="#como-participar" class="nav-link" 
             :class="{ active: activeSection === 'como-participar' }"
             @click="navigateToSection('como-participar')">Como Participar</a>
        </li>
        <li class="nav-item">
          <a href="#premios" class="nav-link" 
             :class="{ active: activeSection === 'premios' }"
             @click="navigateToSection('premios')">Prêmios</a>
        </li>
        <li class="nav-item">
          <a href="#faq" class="nav-link" 
             :class="{ active: activeSection === 'faq' }"
             @click="navigateToSection('faq')">FAQ</a>
        </li>
        <li class="nav-item">
          <a href="#ganhadores" class="nav-link" 
             :class="{ active: activeSection === 'ganhadores' }"
             @click="navigateToSection('ganhadores')">Ganhadores</a>
        </li>
        <li class="nav-item">
          <a href="#lojas" class="nav-link" 
             :class="{ active: activeSection === 'lojas' }"
             @click="navigateToSection('lojas')">Lojas</a>
        </li>
        <li class="nav-item" v-if="isAuthenticated">
          <router-link to="/admin" class="nav-link nav-link-admin">
            👥 Usuários
          </router-link>
        </li>
        
        <li class="nav-item nav-cta" v-if="!isAuthenticated">
          <button class="btn-cadastro" @click="$emit('show-auth')">
            👤 Entrar
          </button>
        </li>
        
        <li class="nav-item nav-cta" v-else>
          <div class="user-menu">
            <span class="user-name">Olá, {{ user?.nome?.split(' ')[0] }}!</span>
            <button class="btn-logout" @click="$emit('logout')">
              🚪 Sair
            </button>
          </div>
        </li>
      </ul>
      
      <button class="hamburger-btn" 
              :class="{ 'hamburger-open': mobileMenuOpen }"
              @click="toggleMobileMenu">
        <span class="hamburger-line"></span>
        <span class="hamburger-line"></span>
        <span class="hamburger-line"></span>
      </button>
    </nav>
  </header>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

interface Props {
  activeSection: string
  isAuthenticated: boolean
  user: any
}

interface Emits {
  (e: 'navigate', section: string): void
  (e: 'show-auth'): void
  (e: 'logout'): void
}

defineProps<Props>()
const emit = defineEmits<Emits>()

const isScrolled = ref(false)
const mobileMenuOpen = ref(false)

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

const toggleMobileMenu = () => {
  mobileMenuOpen.value = !mobileMenuOpen.value
}

const navigateToSection = (section: string) => {
  emit('navigate', section)
  mobileMenuOpen.value = false
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style scoped>
.app-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1001;
  background: var(--gradient-primary);
  backdrop-filter: blur(10px);
  transition: all 0.3s ease;
  box-shadow: 0 2px 20px rgba(30, 58, 138, 0.1);
}

.app-header.scrolled {
  background: rgba(30, 58, 138, 0.95);
  backdrop-filter: blur(15px);
  box-shadow: 0 2px 30px rgba(30, 58, 138, 0.2);
}

.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.nav-brand h1 {
  font-size: 1.5rem;
  font-weight: 800;
  color: white;
  margin: 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.nav-menu {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
  align-items: center;
  gap: 1.2rem;
  flex-wrap: nowrap;
}

.nav-item {
  position: relative;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-weight: 600;
  padding: 0.5rem 0.8rem;
  border-radius: 8px;
  transition: all 0.3s ease;
  font-size: 0.95rem;
  white-space: nowrap;
}

.nav-link:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
}

.nav-link.active,
.nav-link.router-link-active {
  background: rgba(255, 255, 255, 0.2);
  box-shadow: 0 4px 15px rgba(255, 255, 255, 0.1);
}

.nav-link-admin {
  color: #fbbf24 !important;
  font-weight: 700;
}

.nav-link-admin:hover {
  background: rgba(251, 191, 36, 0.1);
}

.nav-cta .btn-cadastro {
  background: var(--gradient-accent);
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 25px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(249, 115, 22, 0.3);
}

.nav-cta .btn-cadastro:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 25px rgba(249, 115, 22, 0.4);
}

.btn-users {
  background: rgba(255, 255, 255, 0.1) !important;
  border: 1px solid rgba(255, 255, 255, 0.2);
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-users:hover {
  background: rgba(255, 255, 255, 0.2) !important;
  transform: translateY(-2px);
}

.user-menu {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.user-name {
  color: white;
  font-weight: 600;
  font-size: 0.9rem;
}

.btn-logout {
  background: rgba(239, 68, 68, 0.8);
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.85rem;
}

.btn-logout:hover {
  background: rgba(239, 68, 68, 1);
  transform: translateY(-1px);
}

.hamburger-btn {
  display: none;
  flex-direction: column;
  gap: 4px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem;
}

.hamburger-line {
  width: 25px;
  height: 3px;
  background: white;
  border-radius: 2px;
  transition: all 0.3s ease;
}

.hamburger-btn.hamburger-open .hamburger-line:nth-child(1) {
  transform: rotate(45deg) translate(6px, 6px);
}

.hamburger-btn.hamburger-open .hamburger-line:nth-child(2) {
  opacity: 0;
}

.hamburger-btn.hamburger-open .hamburger-line:nth-child(3) {
  transform: rotate(-45deg) translate(6px, -6px);
}

@media (max-width: 1200px) {
  .nav-menu {
    gap: 0.8rem;
  }
  
  .nav-link {
    padding: 0.5rem 0.6rem;
    font-size: 0.9rem;
  }
  
  .nav-brand h1 {
    font-size: 1.3rem;
  }
}

@media (max-width: 768px) {
  .navbar {
    padding: 1rem;
  }
  
  .nav-brand h1 {
    font-size: 1.2rem;
  }
  
  .hamburger-btn {
    display: flex;
  }
  
  .nav-menu {
    position: fixed;
    top: 80px;
    left: 0;
    right: 0;
    flex-direction: column;
    background: var(--gradient-primary);
    padding: 2rem;
    gap: 1rem;
    transform: translateX(-100%);
    transition: transform 0.3s ease;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
    z-index: 1000;
    display: flex;
  }
  
  .nav-menu.nav-menu-open {
    transform: translateX(0);
  }
  
  .user-menu {
    flex-direction: column;
    gap: 0.5rem;
    align-items: flex-start;
  }
}

@media (max-width: 480px) {
  .nav-brand h1 {
    font-size: 1rem;
  }
  
  .navbar {
    padding: 0.75rem;
  }
  
  .nav-menu {
    top: 70px;
    padding: 1.5rem;
  }
}
</style>