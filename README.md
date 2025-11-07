# 🏛️ Sistema de Agendamento de Espaços Universitários (SIRU)

Sistema completo de gerenciamento de reservas de espaços universitários desenvolvido em React (frontend) e Next.js (backend).

## 🚀 Arquitetura

Este projeto está dividido em dois repositórios separados:

- **Frontend (React + Vite)**: Deploy no **Vercel**
  - Repositório: `sistemadeagenddamentodesalas-front`
  - URL: https://github.com/sistemasufcrussas/sistemadeagenddamentodesalas-front.git

- **Backend (Next.js API)**: Deploy no **Render**
  - Repositório: `sistemadeagenddamentodesalas`
  - URL: https://github.com/sistemasufcrussas/sistemadeagenddamentodesalas.git

**Conta de deploy**: sistemaufcrussas@gmail.com

## 📋 Funcionalidades

- ✅ Sistema de autenticação com JWT
- ✅ Gerenciamento de usuários (Admin, Professor, Coordenador, Aluno, Portaria)
- ✅ Reserva de salas e espaços
- ✅ Sistema de aprovação de reservas
- ✅ Gerenciamento de projetos e alunos
- ✅ Notificações e logs de auditoria
- ✅ Dashboard personalizado por tipo de usuário

## 🛠️ Tecnologias

### Frontend
- React 18
- React Router DOM
- Tailwind CSS
- Vite
- Radix UI
- Framer Motion

### Backend
- Next.js 14
- PostgreSQL (Neon)
- JWT Authentication
- Node.js

## 📦 Instalação

### Frontend

```bash
npm install
npm run dev
```

### Backend

Veja o README no repositório do backend para instruções completas.

## 🔧 Configuração

### Variáveis de Ambiente

#### Frontend (.env)
```env
VITE_API_URL=https://seu-backend.onrender.com
```

#### Backend (.env.local)
```env
DATABASE_URL=sua-connection-string-postgresql
JWT_SECRET=seu-jwt-secret
ALLOWED_ORIGIN=https://seu-frontend.vercel.app
NODE_ENV=production
```

## 📡 Endpoints da API

### Autenticação
- `POST /api/auth/login` - Login
- `POST /api/auth/register` - Registro
- `GET /api/auth/verify` - Verificar token

### Usuários
- `GET /api/users` - Listar usuários
- `GET /api/users/pending` - Usuários pendentes
- `POST /api/users/approve` - Aprovar usuário

### Salas
- `GET /api/rooms` - Listar salas
- `GET /api/rooms/[id]` - Detalhes da sala

### Reservas
- `GET /api/reservations` - Listar reservas
- `POST /api/reservations` - Criar reserva
- `GET /api/reservations/pending` - Reservas pendentes
- `POST /api/reservations/approve` - Aprovar reserva

## 👥 Tipos de Usuário

- **Admin**: Acesso total ao sistema
- **Coordenador**: Aprovação de reservas e usuários
- **Professor**: Criação de projetos e reservas
- **Aluno**: Visualização e participação em projetos
- **Portaria**: Visualização de reservas aprovadas

## 📝 Scripts

```bash
npm run dev      # Desenvolvimento
npm run build    # Build para produção
npm run preview  # Preview do build
npm run lint     # Linter
```

## 🔐 Segurança

- Senhas criptografadas com bcrypt
- Tokens JWT com expiração
- CORS configurado
- Validação de entrada
- Logs de auditoria

## 📚 Documentação

Para mais detalhes sobre a arquitetura e deploy, consulte:
- `ARQUITETURA.md` - Detalhes sobre a separação frontend/backend

## 🤝 Contribuição

Este projeto foi desenvolvido para o Sistema de Reservas Universitário (SIRU).

---

**Desenvolvido para Sistemas UFC Russas**  
*Email: sistemaufcrussas@gmail.com*
