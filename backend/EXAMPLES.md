# Exemplos de Uso da API de Promoções

Este arquivo contém exemplos práticos de como usar a API.

## 1. Iniciar a aplicação

```bash
# Instalar dependências
npm install

# Executar em modo desenvolvimento
npm run dev
```

## 2. Exemplos de cURL

### Health Check
```bash
curl http://localhost:3000/api/v1/health
```

### Criar uma promoção
```bash
curl -X POST http://localhost:3000/api/v1/promocoes \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Black Friday 2024",
    "descricao": "Desconto especial para Black Friday",
    "tipo": "desconto_percentual",
    "valor": 50,
    "produtos": ["produto1", "produto2"],
    "dataInicio": "2024-11-25T00:00:00.000Z",
    "dataFim": "2024-11-29T23:59:59.000Z",
    "codigoPromocional": "BLACKFRIDAY50",
    "limiteUso": 1000
  }'
```

### Listar promoções
```bash
curl "http://localhost:3000/api/v1/promocoes?page=1&limit=10&ativo=true"
```

### Aplicar código promocional
```bash
curl -X POST http://localhost:3000/api/v1/promocoes/aplicar \
  -H "Content-Type: application/json" \
  -d '{"codigoPromocional": "BLACKFRIDAY50"}'
```

### Calcular desconto
```bash
curl -X POST http://localhost:3000/api/v1/promocoes/calcular-desconto \
  -H "Content-Type: application/json" \
  -d '{
    "promocaoId": "ID_DA_PROMOCAO_AQUI",
    "valorOriginal": 100.00
  }'
```

## 3. Tipos de Promoção Suportados

- `desconto_percentual`: Desconto em %
- `desconto_fixo`: Desconto em valor fixo
- `frete_gratis`: Frete grátis
- `leve_pague`: Promoções especiais

## 4. Estrutura de Resposta

Todas as respostas seguem o padrão:

```json
{
  "success": true,
  "message": "Mensagem descritiva",
  "data": { /* dados da resposta */ }
}
```