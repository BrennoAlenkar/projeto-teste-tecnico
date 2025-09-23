# Configuração para Integração Frontend + Backend

Este documento explica como conectar o backend da API de Promoções com o frontend React.

## 🔧 Configuração do Backend (API)

### Porta e CORS
- **Backend roda na porta**: `3001`
- **Frontend pode rodar nas portas**: `3000`, `5173`, `5174`
- **CORS configurado** para aceitar requisições do frontend

### URLs da API
```javascript
// Base URL para o frontend consumir
const API_BASE_URL = 'http://localhost:3001/api/v1';

// Endpoints principais
const ENDPOINTS = {
  health: `${API_BASE_URL}/health`,
  promocoes: `${API_BASE_URL}/promocoes`,
  aplicarPromocao: `${API_BASE_URL}/promocoes/aplicar`,
  calcularDesconto: `${API_BASE_URL}/promocoes/calcular-desconto`
};
```

## 📱 Configuração no Frontend React

### 1. Instalar cliente HTTP (se não tiver)
```bash
npm install axios
# ou
npm install fetch
```

### 2. Criar serviço da API
Crie `src/services/promocaoService.js`:

```javascript
import axios from 'axios';

const API_BASE_URL = 'http://localhost:3001/api/v1';

const api = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
  withCredentials: true
});

export const promocaoService = {
  // Listar promoções
  async listarPromocoes(filtros = {}) {
    const params = new URLSearchParams(filtros).toString();
    const response = await api.get(`/promocoes?${params}`);
    return response.data;
  },

  // Criar promoção
  async criarPromocao(dadosPromocao) {
    const response = await api.post('/promocoes', dadosPromocao);
    return response.data;
  },

  // Buscar promoção por ID
  async buscarPromocao(id) {
    const response = await api.get(`/promocoes/${id}`);
    return response.data;
  },

  // Atualizar promoção
  async atualizarPromocao(id, dados) {
    const response = await api.put(`/promocoes/${id}`, dados);
    return response.data;
  },

  // Deletar promoção
  async deletarPromocao(id) {
    await api.delete(`/promocoes/${id}`);
  },

  // Aplicar código promocional
  async aplicarPromocao(codigoPromocional) {
    const response = await api.post('/promocoes/aplicar', { codigoPromocional });
    return response.data;
  },

  // Calcular desconto
  async calcularDesconto(promocaoId, valorOriginal) {
    const response = await api.post('/promocoes/calcular-desconto', {
      promocaoId,
      valorOriginal
    });
    return response.data;
  }
};
```

### 3. Exemplo de uso em um componente React

```jsx
import React, { useState, useEffect } from 'react';
import { promocaoService } from '../services/promocaoService';

function ListaPromocoes() {
  const [promocoes, setPromocoes] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    carregarPromocoes();
  }, []);

  const carregarPromocoes = async () => {
    try {
      setLoading(true);
      const response = await promocaoService.listarPromocoes({
        ativo: true,
        page: 1,
        limit: 10
      });
      setPromocoes(response.data);
    } catch (err) {
      setError('Erro ao carregar promoções');
      console.error(err);
    } finally {
      setLoading(false);
    }
  };

  const aplicarCodigo = async (codigo) => {
    try {
      const response = await promocaoService.aplicarPromocao(codigo);
      alert(`Promoção aplicada: ${response.data.nome}`);
      carregarPromocoes(); // Recarregar lista
    } catch (err) {
      alert('Erro ao aplicar código promocional');
    }
  };

  if (loading) return <div>Carregando...</div>;
  if (error) return <div>Erro: {error}</div>;

  return (
    <div>
      <h2>Promoções Ativas</h2>
      {promocoes.map(promocao => (
        <div key={promocao._id} className="promocao-card">
          <h3>{promocao.nome}</h3>
          <p>{promocao.descricao}</p>
          <p>Desconto: {promocao.valor}%</p>
          <p>Código: {promocao.codigoPromocional}</p>
          <button onClick={() => aplicarCodigo(promocao.codigoPromocional)}>
            Aplicar Promoção
          </button>
        </div>
      ))}
    </div>
  );
}

export default ListaPromocoes;
```

## 🚀 Como Rodar Frontend + Backend

### 1. Terminal 1 - Backend (API)
```bash
cd promocao-backend-api
npm run dev
# Rodará na porta 3001
```

### 2. Terminal 2 - Frontend (React)
```bash
cd [pasta-do-frontend]
npm run dev
# Rodará na porta 3000, 5173 ou 5174
```

### 3. Testear Conexão
- Frontend: http://localhost:3000 (ou 5173/5174)
- Backend API: http://localhost:3001/api/v1/health

## 🔍 Testando a Integração

### Teste simples no console do browser:
```javascript
// Testar se API está acessível
fetch('http://localhost:3001/api/v1/health')
  .then(res => res.json())
  .then(data => console.log(data));

// Testar listagem de promoções  
fetch('http://localhost:3001/api/v1/promocoes')
  .then(res => res.json())
  .then(data => console.log(data));
```

## ⚠️ Possíveis Problemas e Soluções

### 1. Erro de CORS
```
Access to fetch at 'http://localhost:3001' from origin 'http://localhost:3000' has been blocked by CORS policy
```
**Solução**: Verificar se o backend está rodando e as origens estão configuradas

### 2. Connection Refused
```
Failed to fetch
```  
**Solução**: Verificar se o backend está rodando na porta 3001

### 3. MongoDB não conectado
```
MongooseServerSelectionError: connect ECONNREFUSED
```
**Solução**: Instalar e iniciar MongoDB local ou configurar MongoDB Atlas

## 📋 Checklist de Integração

- [ ] Backend rodando na porta 3001
- [ ] Frontend rodando na porta 3000/5173/5174  
- [ ] MongoDB conectado e funcionando
- [ ] CORS configurado corretamente
- [ ] Serviço de API criado no frontend
- [ ] Componentes consumindo a API
- [ ] Tratamento de erros implementado

## 🎯 Próximos Passos

1. **Configurar MongoDB** (local ou Atlas)
2. **Testar endpoints** individualmente
3. **Implementar componentes** no frontend  
4. **Adicionar tratamento de loading/erro**
5. **Implementar formulários** para CRUD
6. **Adicionar autenticação** (se necessário)