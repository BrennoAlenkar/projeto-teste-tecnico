<template>
  <section class="contact-section">
    <div class="container">
      <h2>Entre em Contato</h2>
      <p class="section-subtitle">
        Estamos prontos para ajudar você. Entre em contato conosco!
      </p>
      
      <div class="contact-content">
        <div class="contact-info">
          <h3>Informações de Contato</h3>
          <div class="contact-item">
            <div class="contact-icon">📧</div>
            <div>
              <h4>Email</h4>
              <p>contato@empresa.com</p>
            </div>
          </div>
          
          <div class="contact-item">
            <div class="contact-icon">📱</div>
            <div>
              <h4>Telefone</h4>
              <p>(11) 99999-9999</p>
            </div>
          </div>
          
          <div class="contact-item">
            <div class="contact-icon">📍</div>
            <div>
              <h4>Endereço</h4>
              <p>São Paulo, SP<br>Brasil</p>
            </div>
          </div>
          
          <div class="social-links">
            <h4>Redes Sociais</h4>
            <div class="social-icons">
              <a href="#" class="social-link">🔗 LinkedIn</a>
              <a href="#" class="social-link">📘 Facebook</a>
              <a href="#" class="social-link">📸 Instagram</a>
            </div>
          </div>
        </div>
        
        <div class="contact-form">
          <h3>Envie uma Mensagem</h3>
          <form @submit.prevent="submitForm">
            <div class="form-group">
              <label for="name">Nome *</label>
              <input 
                type="text" 
                id="name" 
                v-model="form.name" 
                required
                placeholder="Seu nome completo"
              >
            </div>
            
            <div class="form-group">
              <label for="email">Email *</label>
              <input 
                type="email" 
                id="email" 
                v-model="form.email" 
                required
                placeholder="seu@email.com"
              >
            </div>
            
            <div class="form-group">
              <label for="phone">Telefone</label>
              <input 
                type="tel" 
                id="phone" 
                v-model="form.phone"
                placeholder="(11) 99999-9999"
              >
            </div>
            
            <div class="form-group">
              <label for="subject">Assunto *</label>
              <select id="subject" v-model="form.subject" required>
                <option value="">Selecione um assunto</option>
                <option value="desenvolvimento">Desenvolvimento Web</option>
                <option value="consultoria">Consultoria</option>
                <option value="suporte">Suporte</option>
                <option value="outros">Outros</option>
              </select>
            </div>
            
            <div class="form-group">
              <label for="message">Mensagem *</label>
              <textarea 
                id="message" 
                v-model="form.message" 
                required
                rows="5"
                placeholder="Descreva como podemos ajudar você..."
              ></textarea>
            </div>
            
            <button type="submit" class="btn-submit" :disabled="isSubmitting">
              {{ isSubmitting ? 'Enviando...' : 'Enviar Mensagem' }}
            </button>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue'

const form = reactive({
  name: '',
  email: '',
  phone: '',
  subject: '',
  message: ''
})

const isSubmitting = ref(false)

const submitForm = async () => {
  isSubmitting.value = true
  
  // Simular envio do formulário
  try {
    await new Promise(resolve => setTimeout(resolve, 2000))
    alert('Mensagem enviada com sucesso!')
    
    // Resetar formulário
    Object.assign(form, {
      name: '',
      email: '',
      phone: '',
      subject: '',
      message: ''
    })
  } catch (error) {
    alert('Erro ao enviar mensagem. Tente novamente.')
  } finally {
    isSubmitting.value = false
  }
}
</script>

<style scoped>
.contact-section {
  padding: 4rem 0;
  background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
  text-align: center;
}

.contact-section h2 {
  font-size: 2.5rem;
  margin-bottom: 1rem;
  color: #333;
}

.section-subtitle {
  font-size: 1.1rem;
  color: #666;
  margin-bottom: 3rem;
}

.contact-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  text-align: left;
  margin-top: 3rem;
}

.contact-info h3,
.contact-form h3 {
  font-size: 1.5rem;
  margin-bottom: 2rem;
  color: #333;
}

.contact-item {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
  margin-bottom: 2rem;
}

.contact-icon {
  font-size: 1.5rem;
  margin-top: 0.2rem;
}

.contact-item h4 {
  margin: 0 0 0.5rem 0;
  color: #333;
}

.contact-item p {
  margin: 0;
  color: #666;
  line-height: 1.5;
}

.social-links {
  margin-top: 2rem;
}

.social-links h4 {
  margin-bottom: 1rem;
  color: #333;
}

.social-icons {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.social-link {
  color: #667eea;
  text-decoration: none;
  transition: color 0.3s ease;
  display: inline-block;
}

.social-link:hover {
  color: #764ba2;
}

.contact-form {
  background: white;
  padding: 2rem;
  border-radius: 1rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  color: #333;
  font-weight: 500;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #e9ecef;
  border-radius: 0.5rem;
  font-size: 1rem;
  transition: border-color 0.3s ease;
  box-sizing: border-box;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #667eea;
}

.form-group textarea {
  resize: vertical;
  min-height: 120px;
}

.btn-submit {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  padding: 1rem 2rem;
  border-radius: 0.5rem;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  width: 100%;
}

.btn-submit:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(102, 126, 234, 0.3);
}

.btn-submit:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

@media (max-width: 768px) {
  .contact-content {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .contact-section h2 {
    font-size: 2rem;
  }
  
  .contact-form {
    padding: 1.5rem;
  }
}
</style>