# Integração Backend API com Vue 3

Guia completo para integrar a API de Promoções com seu projeto Vue 3.

## 🔧 Configuração do Backend

### Portas Configuradas
- **Backend API**: `http://localhost:3001`  
- **Frontend Vue 3**: `http://localhost:5173` (Vite) ou `http://localhost:3000`
- **CORS**: Configurado para aceitar requisições do Vue

## 📱 Configuração no Vue 3

### 1. Instalar Axios (se não tiver)
```bash
npm install axios
```

### 2. Criar Serviço da API
Crie o arquivo `src/services/promocaoService.js`:

```javascript
import axios from 'axios'

const API_BASE_URL = 'http://localhost:3001/api/v1'

// Configuração do axios
const api = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json'
  },
  timeout: 10000
})

// Interceptor para logs de desenvolvimento
if (import.meta.env.DEV) {
  api.interceptors.request.use(
    (config) => {
      console.log(`🚀 ${config.method?.toUpperCase()} ${config.url}`, config.data)
      return config
    }
  )

  api.interceptors.response.use(
    (response) => {
      console.log(`✅ ${response.status} ${response.config.url}`, response.data)
      return response
    },
    (error) => {
      console.error('❌ Erro na API:', error.response?.data || error.message)
      return Promise.reject(error)
    }
  )
}

export const promocaoService = {
  // Health check
  async healthCheck() {
    const response = await api.get('/health')
    return response.data
  },

  // Listar promoções
  async listarPromocoes(filtros = {}) {
    const response = await api.get('/promocoes', { params: filtros })
    return response.data
  },

  // Criar promoção
  async criarPromocao(dadosPromocao) {
    const response = await api.post('/promocoes', dadosPromocao)
    return response.data
  },

  // Buscar promoção por ID
  async buscarPromocao(id) {
    const response = await api.get(`/promocoes/${id}`)
    return response.data
  },

  // Atualizar promoção
  async atualizarPromocao(id, dados) {
    const response = await api.put(`/promocoes/${id}`, dados)
    return response.data
  },

  // Deletar promoção
  async deletarPromocao(id) {
    await api.delete(`/promocoes/${id}`)
    return { success: true }
  },

  // Aplicar código promocional
  async aplicarPromocao(codigoPromocional) {
    const response = await api.post('/promocoes/aplicar', { codigoPromocional })
    return response.data
  },

  // Calcular desconto
  async calcularDesconto(promocaoId, valorOriginal) {
    const response = await api.post('/promocoes/calcular-desconto', {
      promocaoId,
      valorOriginal
    })
    return response.data
  },

  // Buscar promoções por produto
  async buscarPromocoesPorProduto(produtoId) {
    const response = await api.get(`/promocoes/produto/${produtoId}`)
    return response.data
  }
}

export default promocaoService
```

### 3. Criar Composable para Vue 3
Crie `src/composables/usePromocoes.js`:

```javascript
import { ref, reactive } from 'vue'
import { promocaoService } from '@/services/promocaoService'

export function usePromocoes() {
  const promocoes = ref([])
  const loading = ref(false)
  const error = ref(null)

  const state = reactive({
    promocoes: [],
    loading: false,
    error: null,
    filtros: {
      ativo: true,
      page: 1,
      limit: 10
    }
  })

  const carregarPromocoes = async (filtros = {}) => {
    try {
      state.loading = true
      state.error = null
      
      const response = await promocaoService.listarPromocoes({
        ...state.filtros,
        ...filtros
      })
      
      state.promocoes = response.data
      return response
    } catch (err) {
      state.error = err.message
      throw err
    } finally {
      state.loading = false
    }
  }

  const criarPromocao = async (dadosPromocao) => {
    try {
      const response = await promocaoService.criarPromocao(dadosPromocao)
      await carregarPromocoes() // Recarregar lista
      return response
    } catch (err) {
      state.error = err.message
      throw err
    }
  }

  const aplicarCodigo = async (codigoPromocional) => {
    try {
      const response = await promocaoService.aplicarPromocao(codigoPromocional)
      await carregarPromocoes() // Recarregar para atualizar contadores
      return response
    } catch (err) {
      state.error = err.message
      throw err
    }
  }

  const deletarPromocao = async (id) => {
    try {
      await promocaoService.deletarPromocao(id)
      await carregarPromocoes() // Recarregar lista
    } catch (err) {
      state.error = err.message
      throw err
    }
  }

  return {
    // Estado reativo
    state,
    
    // Estados individuais (compatibilidade)
    promocoes: state.promocoes,
    loading: state.loading,
    error: state.error,
    
    // Métodos
    carregarPromocoes,
    criarPromocao,
    aplicarCodigo,
    deletarPromocao
  }
}
```

### 4. Exemplo de Componente Vue
Crie `src/components/ListaPromocoes.vue`:

```vue
<template>
  <div class="lista-promocoes">
    <div class="header">
      <h2>Promoções Ativas</h2>
      <button @click="carregarPromocoes" :disabled="loading">
        {{ loading ? 'Carregando...' : 'Atualizar' }}
      </button>
    </div>

    <!-- Loading -->
    <div v-if="loading" class="loading">
      Carregando promoções...
    </div>

    <!-- Erro -->
    <div v-else-if="error" class="error">
      <p>❌ {{ error }}</p>
      <button @click="carregarPromocoes">Tentar novamente</button>
    </div>

    <!-- Lista de promoções -->
    <div v-else class="promocoes-grid">
      <div
        v-for="promocao in promocoes"
        :key="promocao._id"
        class="promocao-card"
      >
        <h3>{{ promocao.nome }}</h3>
        <p class="descricao">{{ promocao.descricao }}</p>
        
        <div class="detalhes">
          <span class="tipo">{{ formatarTipo(promocao.tipo) }}</span>
          <span class="valor">{{ formatarValor(promocao.tipo, promocao.valor) }}</span>
        </div>

        <div class="periodo">
          <small>
            {{ formatarData(promocao.dataInicio) }} até {{ formatarData(promocao.dataFim) }}
          </small>
        </div>

        <div v-if="promocao.codigoPromocional" class="codigo">
          <strong>Código: {{ promocao.codigoPromocional }}</strong>
          <button @click="aplicarCodigo(promocao.codigoPromocional)">
            Aplicar
          </button>
        </div>

        <div class="stats">
          <span v-if="promocao.limiteUso">
            {{ promocao.usosAtuais }}/{{ promocao.limiteUso }} usos
          </span>
          <span :class="{ ativo: promocao.ativo, inativo: !promocao.ativo }">
            {{ promocao.ativo ? 'Ativa' : 'Inativa' }}
          </span>
        </div>
      </div>
    </div>

    <!-- Paginação -->
    <div v-if="promocoes.length > 0" class="paginacao">
      <button @click="paginaAnterior" :disabled="filtros.page <= 1">
        Anterior
      </button>
      <span>Página {{ filtros.page }}</span>
      <button @click="proximaPagina">
        Próxima
      </button>
    </div>
  </div>
</template>

<script setup>
import { onMounted, reactive } from 'vue'
import { usePromocoes } from '@/composables/usePromocoes'

const { promocoes, loading, error, carregarPromocoes, aplicarCodigo } = usePromocoes()

const filtros = reactive({
  ativo: true,
  page: 1,
  limit: 10
})

onMounted(() => {
  carregarPromocoes(filtros)
})

const formatarTipo = (tipo) => {
  const tipos = {
    'desconto_percentual': 'Desconto %',
    'desconto_fixo': 'Desconto Fixo',
    'frete_gratis': 'Frete Grátis',
    'leve_pague': 'Leve e Pague'
  }
  return tipos[tipo] || tipo
}

const formatarValor = (tipo, valor) => {
  if (tipo === 'desconto_percentual') return `${valor}% OFF`
  if (tipo === 'desconto_fixo') return `R$ ${valor} OFF`
  if (tipo === 'frete_gratis') return 'GRÁTIS'
  return valor
}

const formatarData = (data) => {
  return new Date(data).toLocaleDateString('pt-BR')
}

const paginaAnterior = () => {
  if (filtros.page > 1) {
    filtros.page--
    carregarPromocoes(filtros)
  }
}

const proximaPagina = () => {
  filtros.page++
  carregarPromocoes(filtros)
}
</script>

<style scoped>
.lista-promocoes {
  padding: 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.promocoes-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.promocao-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  background: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.promocao-card h3 {
  margin: 0 0 8px 0;
  color: #333;
}

.descricao {
  color: #666;
  margin-bottom: 12px;
}

.detalhes {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
}

.tipo {
  background: #f0f0f0;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.8em;
}

.valor {
  font-weight: bold;
  color: #e74c3c;
}

.codigo {
  background: #f8f9fa;
  padding: 8px;
  border-radius: 4px;
  margin: 8px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.stats {
  display: flex;
  justify-content: space-between;
  font-size: 0.8em;
  margin-top: 8px;
}

.ativo { color: #27ae60; }
.inativo { color: #e74c3c; }

.paginacao {
  display: flex;
  justify-content: center;
  gap: 16px;
  align-items: center;
}

.loading, .error {
  text-align: center;
  padding: 40px;
}

.error {
  color: #e74c3c;
}

button {
  background: #3498db;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background: #2980b9;
}

button:disabled {
  background: #bdc3c7;
  cursor: not-allowed;
}
</style>
```

## 🚀 Como Usar

### 1. Rodar Backend
```bash
cd promocao-backend-api
npm run dev
# Backend roda na porta 3001
```

### 2. Rodar Frontend Vue 3
```bash
cd [seu-projeto-vue]
npm run dev
# Vue roda na porta 5173 (Vite) ou 3000
```

### 3. Testar Integração
```javascript
// No console do browser
fetch('http://localhost:3001/api/v1/health')
  .then(res => res.json())
  .then(data => console.log(data))
```

## 🔍 Estrutura de Arquivos no Vue

```
src/
├── services/
│   └── promocaoService.js     # Serviço da API
├── composables/
│   └── usePromocoes.js        # Composable Vue 3
├── components/
│   └── ListaPromocoes.vue     # Componente exemplo
└── views/
    └── PromocoesView.vue      # View principal
```

## 📋 Próximos Passos

1. ✅ Backend configurado (porta 3001)
2. ✅ Serviços Vue 3 criados
3. ✅ Composables implementados
4. ⚠️ **Configurar MongoDB** para o backend funcionar
5. 🔄 Integrar componentes ao seu projeto Vue

## 🎯 URLs de Teste

- **Health**: http://localhost:3001/api/v1/health
- **Promoções**: http://localhost:3001/api/v1/promocoes
- **Frontend**: http://localhost:5173 (ou 3000)