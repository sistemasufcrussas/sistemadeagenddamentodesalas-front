# 🔐 Análise de Segurança: JWT em vitrine-lar vs sistema-de-agendamento

## 📊 Comparação de Segurança

### ✅ **sistema-de-agendamento (MAIS SEGURO)**

#### **Pontos Positivos:**
1. **JWT_SECRET sem fallback** ✅
   ```javascript
   // backend/lib/auth.js
   jwt.sign(payload, process.env.JWT_SECRET, { expiresIn: '24h' })
   ```
   - **Seguro**: Se `JWT_SECRET` não estiver definido, o sistema falha (comportamento correto)
   - **Força configuração**: Não permite deploy sem secret configurado

2. **Expiração curta** ✅
   - **24 horas**: Tokens expiram rapidamente, reduzindo janela de ataque
   - **Menor risco**: Se token for comprometido, expira em 1 dia

3. **Bcrypt com salt adequado** ✅
   ```javascript
   const saltRounds = 12; // Bom!
   ```

4. **Validação de token consistente** ✅
   - Middleware de autenticação bem implementado
   - Tratamento de erros adequado

---

### ⚠️ **vitrine-lar (TEM PROBLEMA DE SEGURANÇA)**

#### **Problema Crítico:**
1. **JWT_SECRET com fallback inseguro** ❌
   ```javascript
   // backend/config/index.js
   export const JWT_SECRET = process.env.JWT_SECRET || 'fallback-secret-key';
   ```
   - **RISCO**: Se `JWT_SECRET` não estiver configurado, usa chave padrão conhecida
   - **Vulnerabilidade**: Qualquer pessoa que conheça o código pode gerar tokens válidos
   - **Crítico em produção**: Nunca deve ter fallback para JWT_SECRET

#### **Pontos Positivos:**
1. **Bcrypt com salt adequado** ✅
   ```javascript
   const saltRounds = 12; // Bom!
   ```

2. **Validação de token** ✅
   - Middleware de autenticação implementado
   - Verificação em múltiplos pontos

#### **Pontos de Atenção:**
1. **Expiração longa** ⚠️
   - **7 dias**: Tokens ficam válidos por muito tempo
   - **Risco**: Se token for comprometido, fica válido por uma semana
   - **Recomendação**: Reduzir para 24h ou menos

---

## 🚨 Problema Crítico no vitrine-lar

### **O que está errado:**
```javascript
// ❌ PERIGOSO
export const JWT_SECRET = process.env.JWT_SECRET || 'fallback-secret-key';
```

### **Por que é perigoso:**
1. **Chave conhecida**: Qualquer pessoa com acesso ao código conhece a chave padrão
2. **Geração de tokens falsos**: Atacantes podem criar tokens válidos
3. **Acesso não autorizado**: Tokens falsos podem ser aceitos pelo sistema
4. **Violação de segurança**: Compromete toda a autenticação

### **Como corrigir:**
```javascript
// ✅ SEGURO
export const JWT_SECRET = process.env.JWT_SECRET;

if (!JWT_SECRET) {
  throw new Error('JWT_SECRET não está configurado! Configure a variável de ambiente.');
}
```

Ou ainda melhor:
```javascript
// ✅ MUITO SEGURO
export const JWT_SECRET = process.env.JWT_SECRET;

if (!JWT_SECRET) {
  console.error('❌ ERRO CRÍTICO: JWT_SECRET não configurado!');
  process.exit(1); // Para o servidor se não tiver secret
}
```

---

## 📋 Recomendações de Segurança

### **Para vitrine-lar:**
1. **URGENTE**: Remover fallback do `JWT_SECRET`
2. **Recomendado**: Reduzir expiração de 7 dias para 24h
3. **Recomendado**: Adicionar validação na inicialização do servidor

### **Para sistema-de-agendamento:**
1. ✅ Já está seguro (sem fallback)
2. ✅ Expiração adequada (24h)
3. **Opcional**: Considerar refresh tokens para melhor UX

---

## 🔒 Boas Práticas de Segurança JWT

### **1. JWT_SECRET:**
- ✅ **NUNCA** usar fallback ou valor padrão
- ✅ **SEMPRE** usar variável de ambiente
- ✅ **GERAR** chave forte (mínimo 32 caracteres aleatórios)
- ✅ **VALIDAR** na inicialização do servidor

### **2. Expiração:**
- ✅ **Curta**: 15 minutos a 1 hora para tokens de acesso
- ✅ **Refresh tokens**: Para sessões longas (7-30 dias)
- ⚠️ **Evitar**: Tokens de acesso com expiração > 24h

### **3. Payload:**
- ✅ **Mínimo necessário**: Apenas dados essenciais (userId, email, role)
- ❌ **NUNCA** incluir senhas ou dados sensíveis
- ✅ **Validação**: Sempre validar dados do payload

### **4. Transmissão:**
- ✅ **HTTPS**: Sempre usar em produção
- ✅ **Headers**: Usar `Authorization: Bearer <token>`
- ❌ **NUNCA** enviar em query strings ou URLs

### **5. Validação:**
- ✅ **Verificar assinatura**: Sempre validar com `jwt.verify()`
- ✅ **Verificar expiração**: Não aceitar tokens expirados
- ✅ **Verificar issuer/audience**: Se aplicável ao seu caso

---

## 🎯 Conclusão

### **sistema-de-agendamento:**
- ✅ **Seguro**: Implementação correta de JWT
- ✅ **Sem vulnerabilidades críticas**
- ✅ **Pronto para produção** (após configurar JWT_SECRET)

### **vitrine-lar:**
- ⚠️ **Vulnerável**: Fallback no JWT_SECRET
- ⚠️ **Necessita correção urgente**
- ⚠️ **Não seguro para produção** até corrigir

### **Recomendação:**
1. **Corrigir vitrine-lar** removendo o fallback
2. **Validar** que JWT_SECRET está configurado na Vercel
3. **Testar** que sistema falha se JWT_SECRET não estiver definido
4. **Considerar** reduzir expiração de tokens

---

## 🔧 Como Corrigir vitrine-lar

### **Passo 1: Atualizar `backend/config/index.js`**
```javascript
import dotenv from 'dotenv';
import { OAuth2Client } from 'google-auth-library';

dotenv.config();

// JWT Secret - SEM FALLBACK (segurança crítica)
export const JWT_SECRET = process.env.JWT_SECRET;

if (!JWT_SECRET) {
  console.error('❌ ERRO CRÍTICO: JWT_SECRET não está configurado!');
  console.error('Configure a variável de ambiente JWT_SECRET antes de iniciar o servidor.');
  process.exit(1);
}

// Google OAuth Client
export const googleClient = new OAuth2Client(process.env.GOOGLE_CLIENT_ID);

// Server Port
export const PORT = process.env.PORT || 3001;
```

### **Passo 2: Verificar na Vercel**
1. Acesse configurações do projeto na Vercel
2. Verifique se `JWT_SECRET` está configurado
3. Se não estiver, adicione uma chave forte (32+ caracteres aleatórios)

### **Passo 3: Testar**
1. Remova temporariamente `JWT_SECRET` do `.env` local
2. Tente iniciar o servidor
3. Deve falhar com erro claro (comportamento correto)

---

**🔐 Segurança é prioridade! Corrija o vitrine-lar antes de usar em produção.**

