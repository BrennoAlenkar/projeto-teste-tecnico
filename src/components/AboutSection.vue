<template>
  <section class="about-section">
    <div class="container">
      <div class="about-content">
        <div class="about-text">
          <h2>Sobre Nós</h2>
          <p class="lead">
            Somos uma empresa dedicada a criar soluções inovadoras e eficientes
            para nossos clientes.
          </p>
          <p>
            Com anos de experiência no mercado, nossa equipe combina conhecimento
            técnico avançado com uma abordagem centrada no cliente. Utilizamos as
            mais modernas tecnologias para entregar produtos de alta qualidade.
          </p>
          <div class="stats">
            <div class="stat-item">
              <h3>100+</h3>
              <p>Projetos Concluídos</p>
            </div>
            <div class="stat-item">
              <h3>50+</h3>
              <p>Clientes Satisfeitos</p>
            </div>
            <div class="stat-item">
              <h3>5+</h3>
              <p>Anos de Experiência</p>
            </div>
          </div>
        </div>
        <div class="about-image">
          <div class="image-placeholder">
            <span>📊</span>
            <p>Imagem da empresa</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
// Lógica do componente Sobre
</script>

<style scoped>
.about-section {
  padding: 4rem 0;
  background-color: #f8f9fa;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.about-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem;
  align-items: center;
}

.about-text h2 {
  font-size: 2.5rem;
  margin-bottom: 1.5rem;
  color: #333;
}

.lead {
  font-size: 1.2rem;
  font-weight: 600;
  color: #667eea;
  margin-bottom: 1.5rem;
}

.about-text p {
  line-height: 1.8;
  color: #666;
  margin-bottom: 1.5rem;
}

.stats {
  display: flex;
  gap: 2rem;
  margin-top: 2rem;
}

.stat-item {
  text-align: center;
}

.stat-item h3 {
  font-size: 2rem;
  color: #667eea;
  margin-bottom: 0.5rem;
}

.stat-item p {
  color: #666;
  font-size: 0.9rem;
  margin: 0;
}

.about-image {
  display: flex;
  justify-content: center;
  align-items: center;
}

.image-placeholder {
  width: 100%;
  height: 400px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: white;
}

.image-placeholder span {
  font-size: 4rem;
  margin-bottom: 1rem;
}

@media (max-width: 768px) {
  .about-content {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .stats {
    justify-content: center;
  }
  
  .about-text h2 {
    font-size: 2rem;
  }
}
</style>