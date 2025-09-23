# API Backend para Sistema de Promoções

Uma API REST moderna e robusta para gerenciamento de promoções, desenvolvida em Node.js com TypeScript, Express e MongoDB.

## 🚀 Funcionalidades

- ✅ CRUD completo para promoções
- ✅ Validações avançadas de dados
- ✅ Diferentes tipos de promoção (percentual, fixo, frete grátis)
- ✅ Sistema de códigos promocionais
- ✅ Controle de limite de uso
- ✅ Filtros e paginação
- ✅ Cálculo automático de descontos
- ✅ Middleware de segurança (Helmet, CORS, Rate Limiting)
- ✅ Tratamento robusto de erros
- ✅ Logs estruturados
- ✅ **Integração pronta com Frontend React/Vue/Angular**
- ✅ **CORS configurado para desenvolvimento**

## 🛠️ Tecnologias

- **Node.js** - Runtime JavaScript
- **TypeScript** - Tipagem estática
- **Express** - Framework web
- **MongoDB** - Banco de dados NoSQL
- **Mongoose** - ODM para MongoDB
- **Express Validator** - Validação de dados
- **Helmet** - Segurança HTTP
- **Morgan** - Logs de requisições
- **CORS** - Cross-Origin Resource Sharing
- **Rate Limiting** - Controle de taxa de requisições

## 📦 Instalação

### Pré-requisitos

- Node.js (versão 16 ou superior)
- MongoDB (local ou MongoDB Atlas)
- npm ou yarn

### Passos

1. **Clone o repositório**
   ```bash
   git clone <url-do-repositorio>
   cd promocao-backend-api
   ```

2. **Instale as dependências**
   ```bash
   npm install
   ```

3. **Configure as variáveis de ambiente**
   ```bash
   cp .env.example .env
   ```
   
   Edite o arquivo `.env` com suas configurações:
   ```env
   PORT=3001
   NODE_ENV=development
   DATABASE_URL=mongodb://localhost:27017/promocoes
   JWT_SECRET=seu-secret-key-aqui
   CORS_ORIGIN=http://localhost:3000,http://localhost:5173,http://localhost:5174
   ```
   
   > **Nota**: Backend roda na porta **3001** para não conflitar com o frontend na porta 3000.

4. **Inicie o MongoDB**
   ```bash
   # Se estiver usando MongoDB local
   mongod
   ```

5. **Execute a aplicação**
   ```bash
   # Desenvolvimento (com hot reload)
   npm run dev
   
   # Produção
   npm run build
   npm start
   ```

## � Integração com Frontend

### URLs da API
- **Backend**: http://localhost:3001/api/v1
- **Health Check**: http://localhost:3001/api/v1/health

### Configuração Rápida para Vue 3
```javascript
// No seu projeto Vue 3, crie src/services/promocaoService.js
import axios from 'axios'

const API_BASE_URL = 'http://localhost:3001/api/v1'

const api = axios.create({
  baseURL: API_BASE_URL,
  headers: {
    'Content-Type': 'application/json'
  }
})

export const promocaoService = {
  async listarPromocoes(filtros = {}) {
    const response = await api.get('/promocoes', { params: filtros })
    return response.data
  },
  
  async criarPromocao(dados) {
    const response = await api.post('/promocoes', dados)
    return response.data
  },

  async aplicarPromocao(codigoPromocional) {
    const response = await api.post('/promocoes/aplicar', { codigoPromocional })
    return response.data
  }
}
```

> 📄 **Integração Vue 3**: Ver [VUE3-INTEGRATION.md](VUE3-INTEGRATION.md)  
> 📄 **Serviço Vue 3**: Ver [vue3-promocao-service.js](vue3-promocao-service.js)  
> 📄 **Composable Vue 3**: Ver [vue3-use-promocoes.js](vue3-use-promocoes.js)

## �📖 Documentação da API

### Base URL
```
http://localhost:3001/api/v1
```

### Endpoints Principais

#### 🏥 Health Check
```http
GET /api/v1/health
```

**Resposta:**
```json
{
  "success": true,
  "message": "API de Promoções funcionando!",
  "timestamp": "2024-09-22T10:00:00.000Z",
  "version": "1.0.0"
}
```

---

#### 📋 Listar Promoções
```http
GET /api/v1/promocoes
```

**Query Parameters:**
- `page` (number): Página (padrão: 1)
- `limit` (number): Itens por página (padrão: 10, máx: 100)
- `sortBy` (string): Campo para ordenação (padrão: 'createdAt')
- `sortOrder` (string): Ordem ('asc' | 'desc', padrão: 'desc')
- `ativo` (boolean): Filtrar por status ativo
- `tipo` (string): Filtrar por tipo de promoção
- `categoria` (string): Filtrar por categoria
- `codigoPromocional` (string): Buscar por código promocional

**Exemplo:**
```http
GET /api/v1/promocoes?page=1&limit=10&ativo=true&tipo=desconto_percentual
```

**Resposta:**
```json
{
  "success": true,
  "message": "Promoções listadas com sucesso",
  "data": [
    {
      "_id": "64f1234567890abcdef12345",
      "nome": "Black Friday 2024",
      "descricao": "Desconto especial para Black Friday",
      "tipo": "desconto_percentual",
      "valor": 50,
      "produtos": ["prod1", "prod2"],
      "dataInicio": "2024-11-25T00:00:00.000Z",
      "dataFim": "2024-11-29T23:59:59.000Z",
      "codigoPromocional": "BLACKFRIDAY50",
      "limiteUso": 1000,
      "usosAtuais": 245,
      "ativo": true,
      "createdAt": "2024-09-22T10:00:00.000Z",
      "updatedAt": "2024-09-22T10:00:00.000Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 50,
    "totalPages": 5
  }
}
```

---

#### 📝 Criar Promoção
```http
POST /api/v1/promocoes
```

**Body:**
```json
{
  "nome": "Promoção de Verão",
  "descricao": "Desconto especial para produtos de verão",
  "tipo": "desconto_percentual",
  "valor": 30,
  "produtos": ["produto1_id", "produto2_id"],
  "categorias": ["categoria1_id"],
  "dataInicio": "2024-12-01T00:00:00.000Z",
  "dataFim": "2024-12-31T23:59:59.000Z",
  "codigoPromocional": "VERAO30",
  "limiteUso": 500
}
```

**Resposta:**
```json
{
  "success": true,
  "message": "Promoção criada com sucesso",
  "data": {
    "_id": "64f1234567890abcdef12346",
    "nome": "Promoção de Verão",
    // ... outros campos
    "createdAt": "2024-09-22T10:00:00.000Z",
    "updatedAt": "2024-09-22T10:00:00.000Z"
  }
}
```

---

#### 🔍 Buscar Promoção por ID
```http
GET /api/v1/promocoes/:id
```

**Resposta:**
```json
{
  "success": true,
  "message": "Promoção encontrada",
  "data": {
    "_id": "64f1234567890abcdef12345",
    "nome": "Black Friday 2024",
    // ... todos os campos da promoção
  }
}
```

---

#### ✏️ Atualizar Promoção
```http
PUT /api/v1/promocoes/:id
```

**Body (campos opcionais):**
```json
{
  "nome": "Novo nome da promoção",
  "valor": 40,
  "ativo": false
}
```

---

#### 🗑️ Deletar Promoção
```http
DELETE /api/v1/promocoes/:id
```

**Resposta:**
```json
{
  "success": true,
  "message": "Promoção deletada com sucesso"
}
```

---

#### 🎯 Buscar Promoções por Produto
```http
GET /api/v1/promocoes/produto/:produtoId
```

---

#### 🏷️ Buscar Promoções por Categoria
```http
GET /api/v1/promocoes/categoria/:categoriaId
```

---

#### 💳 Aplicar Código Promocional
```http
POST /api/v1/promocoes/aplicar
```

**Body:**
```json
{
  "codigoPromocional": "BLACKFRIDAY50"
}
```

**Resposta:**
```json
{
  "success": true,
  "message": "Promoção aplicada com sucesso",
  "data": {
    "_id": "64f1234567890abcdef12345",
    "nome": "Black Friday 2024",
    "tipo": "desconto_percentual",
    "valor": 50,
    "usosAtuais": 246
  }
}
```

---

#### 💰 Calcular Desconto
```http
POST /api/v1/promocoes/calcular-desconto
```

**Body:**
```json
{
  "promocaoId": "64f1234567890abcdef12345",
  "valorOriginal": 100.00
}
```

**Resposta:**
```json
{
  "success": true,
  "message": "Desconto calculado com sucesso",
  "data": {
    "promocao": {
      "id": "64f1234567890abcdef12345",
      "nome": "Black Friday 2024",
      "tipo": "desconto_percentual"
    },
    "valorOriginal": 100.00,
    "valorDesconto": 50.00,
    "valorFinal": 50.00,
    "tipoDesconto": "desconto_percentual"
  }
}
```

## 📊 Tipos de Promoção

- **`desconto_percentual`**: Desconto em porcentagem (ex: 30% off)
- **`desconto_fixo`**: Desconto em valor fixo (ex: R$ 10 off)
- **`frete_gratis`**: Frete grátis
- **`leve_pague`**: Promoção do tipo "Leve 3, Pague 2"

## 🔒 Validações

### Promoção
- Nome: 3-100 caracteres
- Descrição: 10-500 caracteres  
- Valor: Número positivo
- Data início: Não pode ser no passado
- Data fim: Posterior à data de início
- Código promocional: 3-20 caracteres, apenas A-Z e 0-9
- Limite de uso: Número inteiro positivo (opcional)

### Validações de Negócio
- Data de fim deve ser posterior à data de início
- Código promocional deve ser único
- Promoção percentual: valor entre 0-100%
- Promoção de frete grátis: valor deve ser 0
- Limite de uso não pode ser excedido

## 🚨 Tratamento de Erros

### Códigos de Status
- `200` - Sucesso
- `201` - Criado com sucesso
- `400` - Dados inválidos
- `404` - Recurso não encontrado
- `409` - Conflito (ex: código promocional já existe)
- `429` - Muitas tentativas
- `500` - Erro interno do servidor

### Formato de Erro
```json
{
  "success": false,
  "message": "Mensagem do erro",
  "errors": [
    {
      "field": "campo_com_erro",
      "message": "Descrição do erro",
      "value": "valor_invalido"
    }
  ]
}
```

## 🔐 Segurança

- **Helmet**: Proteção contra vulnerabilidades HTTP
- **CORS**: Configuração de domínios permitidos
- **Rate Limiting**: Máximo 100 requests por IP a cada 15 minutos
- **Validação rigorosa**: Todos os inputs são validados
- **Error Handling**: Não exposição de informações sensíveis

## 📈 Performance

- **Indexação**: Índices no MongoDB para queries otimizadas
- **Paginação**: Limitação de resultados para evitar sobrecarga
- **Caching**: Headers apropriados de cache
- **Compressão**: Compressão gzip habilitada

## 🧪 Testes Manuais

### Exemplos com cURL

1. **Criar uma promoção:**
```bash
curl -X POST http://localhost:3000/api/v1/promocoes \\
  -H "Content-Type: application/json" \\
  -d '{
    "nome": "Teste API",
    "descricao": "Promoção de teste da API",
    "tipo": "desconto_percentual",
    "valor": 25,
    "produtos": ["prod123"],
    "dataInicio": "2024-12-01T00:00:00.000Z",
    "dataFim": "2024-12-31T23:59:59.000Z",
    "codigoPromocional": "TESTE25"
  }'
```

2. **Listar promoções:**
```bash
curl http://localhost:3000/api/v1/promocoes?page=1&limit=5
```

3. **Aplicar código promocional:**
```bash
curl -X POST http://localhost:3000/api/v1/promocoes/aplicar \\
  -H "Content-Type: application/json" \\
  -d '{"codigoPromocional": "TESTE25"}'
```

## 🚀 Deploy

### Usando Docker (opcional)
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY dist ./dist
EXPOSE 3000
CMD ["npm", "start"]
```

### Variáveis de Ambiente para Produção
```env
NODE_ENV=production
PORT=3000
DATABASE_URL=mongodb+srv://user:pass@cluster.mongodb.net/promocoes
JWT_SECRET=super-secret-key-production
CORS_ORIGIN=https://seu-frontend.com
```

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -am 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 📞 Suporte

Para dúvidas ou suporte:
- Abra uma [issue](https://github.com/seu-usuario/promocao-backend-api/issues)
- Email: [seu-email@exemplo.com](mailto:seu-email@exemplo.com)

---

**Desenvolvido com ❤️ em TypeScript**