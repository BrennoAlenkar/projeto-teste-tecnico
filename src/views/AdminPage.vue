<template>
  <div class="admin-page">
    <!-- Header da página administrativa -->
    <div class="admin-header">
      <div class="container">
        <h1 class="admin-title">🛠️ Painel Administrativo</h1>
        <p class="admin-subtitle">Gestão de usuários e configurações da promoção</p>
        
        <div class="admin-actions">
          <router-link to="/" class="btn-back">
            ← Voltar para o site
          </router-link>
          <div class="user-info" v-if="currentUser">
            <span class="welcome-text">Olá, {{ currentUser.nome }}!</span>
            <button @click="handleLogout" class="btn-logout">
              Sair
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Conteúdo administrativo -->
    <div class="admin-content">
      <UsuariosSection />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import UsuariosSection from '../components/UsuariosSection.vue'

const router = useRouter()
const currentUser = ref<any>(null)

const handleLogout = () => {
  localStorage.removeItem('currentUser')
  currentUser.value = null
  router.push('/')
}

onMounted(() => {
  // Verificar se há usuário logado
  const savedUser = localStorage.getItem('currentUser')
  if (savedUser) {
    try {
      currentUser.value = JSON.parse(savedUser)
    } catch (error) {
      console.warn('Erro ao carregar usuário atual:', error)
    }
  }
})
</script>

<style scoped>
.admin-page {
  min-height: 100vh;
  background: #f8fafc;
}

.admin-header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem 0;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
}

.admin-header .container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.admin-title {
  font-size: 2rem;
  font-weight: 700;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.admin-subtitle {
  margin: 0.5rem 0 0 0;
  opacity: 0.9;
  font-size: 1.1rem;
}

.admin-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.btn-back {
  background: rgba(255, 255, 255, 0.2);
  color: white;
  text-decoration: none;
  padding: 0.75rem 1.5rem;
  border-radius: 8px;
  font-weight: 600;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
}

.btn-back:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-2px);
}

.user-info {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.welcome-text {
  font-weight: 600;
  opacity: 0.9;
}

.btn-logout {
  background: rgba(239, 68, 68, 0.2);
  color: white;
  border: 1px solid rgba(239, 68, 68, 0.4);
  padding: 0.5rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.3s ease;
}

.btn-logout:hover {
  background: rgba(239, 68, 68, 0.3);
  border-color: rgba(239, 68, 68, 0.6);
}

.admin-content {
  padding: 2rem 0;
}

@media (max-width: 768px) {
  .admin-header .container {
    flex-direction: column;
    text-align: center;
  }
  
  .admin-title {
    font-size: 1.5rem;
  }
  
  .admin-actions {
    flex-direction: column;
    width: 100%;
  }
  
  .user-info {
    flex-direction: column;
    gap: 0.5rem;
  }
}
</style>