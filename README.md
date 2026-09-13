# Cybersecurity Agent for opencode

Agente de cybersegurança especializado para **opencode** que analisa planos de sites, aplica segurança em rotas/APIs, limita requisições, criptografa dados e protege contra ataques de força bruta.

## 🛡️ O que o agente faz

### Análise Inicial
- Revisa arquitetura do site (frontend, backend, banco de dados)
- Identifica pontos de entrada (rotas, APIs, formulários, uploads)
- Mapeia fluxo de dados sensíveis (auth, pagamentos, PII)
- Verifica dependências e vulnerabilidades conhecidas (CVE)

### Segurança de Rotas e APIs
- Rate limiting em todas as rotas (especialmente auth)
- CORS restritivo (origins permitidas apenas)
- Validação e sanitização de todas as entradas
- Headers de segurança completos:
  - Content-Security-Policy (CSP)
  - X-Frame-Options: DENY
  - X-Content-Type-Options: nosniff
  - Referrer-Policy: strict-origin-when-cross-origin
  - Permissions-Policy
  - Strict-Transport-Security (HSTS)
- Autenticação JWT com expiração curta + refresh tokens
- Autorização RBAC (roles/permissions)

### Proteção contra Força Bruta
- Rate limiting agressivo: máx 5 tentativas/IP/15min
- Bloqueio progressivo exponencial
- Captcha após 3 falhas
- Lockout de conta após 10 tentativas (unlock via email)
- Monitoramento de IPs suspeitos
- Log de tentativas suspeitas

### Criptografia e Proteção de Dados
- **Em trânsito**: TLS 1.3 obrigatório, HSTS, certificate pinning
- **Em repouso**:
  - Senhas: Argon2id (memory-hard)
  - Dados sensíveis: AES-256-GCM
  - Chaves: rotação periódica, KMS/HSM
- **Banco**: campos sensíveis criptografados (PII, tokens, API keys)
- **Cookies**: HttpOnly, Secure, SameSite=Strict, prefixo `__Host-`

### Performance com Foco em Segurança
- Headers otimizados (cache-control, etag, compression)
- CDN com WAF integrado
- Minificação e compressão (brotli/gzip)
- Lazy loading para recursos não críticos
- Service Workers para cache offline seguro
- Monitoramento Core Web Vitals

### Medidas Adicionais
- WAF com regras OWASP Top 10
- CSRF Protection (tokens + SameSite)
- XSS Prevention (CSP rigoroso, output encoding)
- SQL/NoSQL Injection prevention (prepared statements, ORM seguro)
- Dependency Scanning (SCA no CI/CD)
- Secrets Management (zero hardcoded, vault/env vars)
- Audit Logging (imutável)
- Incident Response (alertas tempo real)
- Penetration Testing agendado
- Backup Criptografado (testado, offsite)

## 📋 Checklist Obrigatório por Deploy
- [ ] Rate limiting em todas as rotas auth
- [ ] Headers de segurança presentes
- [ ] TLS 1.3 + HSTS ativo
- [ ] Senhas com Argon2id
- [ ] Dados sensíveis criptografados em repouso
- [ ] CORS restritivo
- [ ] CSP sem 'unsafe-inline'/'unsafe-eval'
- [ ] Cookies HttpOnly + Secure + SameSite=Strict
- [ ] Logs de auditoria imutáveis
- [ ] Dependências sem CVE crítico/alto
- [ ] WAF ativo com regras OWASP
- [ ] Backup testado nas últimas 24h

## 🚀 Como Usar

1. Clone este repositório
2. Copie `.opencode/agent/cybersecurity.md` para seu projeto em `.opencode/agent/`
3. Reinicie o opencode
4. Use o agente como subagent: `cybersecurity`

```bash
# No seu projeto
mkdir -p .opencode/agent
cp /path/to/Cyber-security-Agent/.opencode/agent/cybersecurity.md .opencode/agent/
# Reinicie o opencode
```

## ⚠️ Importante
**Segurança é default, não opcional.** Este agente nunca compromete segurança por velocidade ou conveniência.
