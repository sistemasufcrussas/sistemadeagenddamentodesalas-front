# 🏗️ Arquitetura do Sistema SIRU

Este documento explica a arquitetura e a separação entre frontend e backend do Sistema de Agendamento de Espaços Universitários.

## 📦 Estrutura de Repositórios

O sistema está dividido em **dois repositórios separados** no GitHub:

### 1. Frontend (React + Vite)
- **Repositório**: `sistemadeagenddamentodesalas-front`
- **URL**: https://github.com/sistemasufcrussas/sistemadeagenddamentodesalas-front.git
- **Deploy**: **Vercel**
- **URL de Produção**: https://siruufc.vercel.app (ou similar)

### 2. Backend (Next.js API)
- **Repositório**: `sistemadeagenddamentodesalas`
- **URL**: https://github.com/sistemasufcrussas/sistemadeagenddamentodesalas.git
- **Deploy**: **Render**
- **URL de Produção**: https://siru-backend.onrender.com (ou similar)

## 🔐 Conta de Deploy

**Email**: sistemaufcrussas@gmail.com

Ambos os serviços (Vercel e Render) estão cadastrados com este email.

## 🚀 Deploy

### Frontend no Vercel

1. Conecte o repositório `sistemadeagenddamentodesalas-front` no Vercel
2. Configure:
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`
   - **Install Command**: `npm install`
3. Adicione variáveis de ambiente:
   - `VITE_API_URL` = URL do backend no Render

### Backend no Render

1. Conecte o repositório `sistemadeagenddamentodesalas` no Render
2. Configure:
   - **Environment**: Node
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`
   - **Root Directory**: (deixe vazio, o repositório já é só do backend)
3. Adicione variáveis de ambiente:
   - `DATABASE_URL` = Connection string do PostgreSQL
   - `JWT_SECRET` = Chave secreta para JWT
   - `ALLOWED_ORIGIN` = URL do frontend no Vercel
   - `NODE_ENV` = `production`

## 🔄 Comunicação Frontend ↔ Backend

O frontend faz requisições HTTP para o backend através da variável de ambiente `VITE_API_URL`.

### Exemplo de Requisição

```javascript
// Frontend (React)
const API_URL = import.meta.env.VITE_API_URL;

fetch(`${API_URL}/api/auth/login`, {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ email, password })
});
```

## 🗄️ Banco de Dados

- **Serviço**: Neon PostgreSQL
- **Configuração**: Definida no backend através da variável `DATABASE_URL`

## 🔒 CORS

O backend está configurado para aceitar requisições apenas do frontend definido em `ALLOWED_ORIGIN`. Isso garante segurança nas requisições entre os serviços.

## 📝 Vantagens desta Arquitetura

1. **Separação de Responsabilidades**: Frontend e backend podem evoluir independentemente
2. **Escalabilidade**: Cada serviço pode ser escalado separadamente
3. **Deploy Independente**: Mudanças em um não afetam o outro
4. **Manutenção**: Mais fácil de manter e debugar
5. **Performance**: Cada serviço otimizado para sua função específica

## 🔧 Desenvolvimento Local

### Frontend
```bash
cd frontend
npm install
npm run dev
# Roda em http://localhost:5173
```

### Backend
```bash
cd backend
npm install
npm run dev
# Roda em http://localhost:3001
```

Certifique-se de configurar `VITE_API_URL=http://localhost:3001` no frontend para desenvolvimento local.

## 📞 Suporte

Para questões sobre deploy ou configuração, consulte:
- Vercel: https://vercel.com/docs
- Render: https://render.com/docs
- Repositório do Backend: Veja `README.md` e `DEPLOY_RENDER.md`

---

**Sistemas UFC Russas**  
*Email: sistemaufcrussas@gmail.com*

