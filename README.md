# Cybersecurity Agent 

Agente de cyberseguranÃ§a especializado para **opencode** que analisa planos de sites, aplica seguranÃ§a em rotas/APIs, limita requisiÃ§Ãµes, criptografa dados e protege contra ataques de forÃ§a bruta.

## ðŸ›¡ï¸ O que o agente faz

### AnÃ¡lise Inicial
- Revisa arquitetura do site (frontend, backend, banco de dados)
- Identifica pontos de entrada (rotas, APIs, formulÃ¡rios, uploads)
- Mapeia fluxo de dados sensÃ­veis (auth, pagamentos, PII)
- Verifica dependÃªncias e vulnerabilidades conhecidas (CVE)

### SeguranÃ§a de Rotas e APIs
- Rate limiting em todas as rotas (especialmente auth)
- CORS restritivo (origins permitidas apenas)
- ValidaÃ§Ã£o e sanitizaÃ§Ã£o de todas as entradas
- Headers de seguranÃ§a completos:
  - Content-Security-Policy (CSP)
  - X-Frame-Options: DENY
  - X-Content-Type-Options: nosniff
  - Referrer-Policy: strict-origin-when-cross-origin
  - Permissions-Policy
  - Strict-Transport-Security (HSTS)
- AutenticaÃ§Ã£o JWT com expiraÃ§Ã£o curta + refresh tokens
- AutorizaÃ§Ã£o RBAC (roles/permissions)

### ProteÃ§Ã£o contra ForÃ§a Bruta
- Rate limiting agressivo: mÃ¡x 5 tentativas/IP/15min
- Bloqueio progressivo exponencial
- Captcha apÃ³s 3 falhas
- Lockout de conta apÃ³s 10 tentativas (unlock via email)
- Monitoramento de IPs suspeitos
- Log de tentativas suspeitas

### Criptografia e ProteÃ§Ã£o de Dados
- **Em trÃ¢nsito**: TLS 1.3 obrigatÃ³rio, HSTS, certificate pinning
- **Em repouso**:
  - Senhas: Argon2id (memory-hard)
  - Dados sensÃ­veis: AES-256-GCM
  - Chaves: rotaÃ§Ã£o periÃ³dica, KMS/HSM
- **Banco**: campos sensÃ­veis criptografados (PII, tokens, API keys)
- **Cookies**: HttpOnly, Secure, SameSite=Strict, prefixo `__Host-`

### Performance com Foco em SeguranÃ§a
- Headers otimizados (cache-control, etag, compression)
- CDN com WAF integrado
- MinificaÃ§Ã£o e compressÃ£o (brotli/gzip)
- Lazy loading para recursos nÃ£o crÃ­ticos
- Service Workers para cache offline seguro
- Monitoramento Core Web Vitals

### Medidas Adicionais
- WAF com regras OWASP Top 10
- CSRF Protection (tokens + SameSite)
- XSS Prevention (CSP rigoroso, output encoding)
- SQL/NoSQL Injection prevention (prepared statements, ORM seguro)
- Dependency Scanning (SCA no CI/CD)
- Secrets Management (zero hardcoded, vault/env vars)
- Audit Logging (imutÃ¡vel)
- Incident Response (alertas tempo real)
- Penetration Testing agendado
- Backup Criptografado (testado, offsite)

## ðŸ“‹ Checklist ObrigatÃ³rio por Deploy
- [ ] Rate limiting em todas as rotas auth
- [ ] Headers de seguranÃ§a presentes
- [ ] TLS 1.3 + HSTS ativo
- [ ] Senhas com Argon2id
- [ ] Dados sensÃ­veis criptografados em repouso
- [ ] CORS restritivo
- [ ] CSP sem 'unsafe-inline'/'unsafe-eval'
- [ ] Cookies HttpOnly + Secure + SameSite=Strict
- [ ] Logs de auditoria imutÃ¡veis
- [ ] DependÃªncias sem CVE crÃ­tico/alto
- [ ] WAF ativo com regras OWASP
- [ ] Backup testado nas Ãºltimas 24h

## ðŸš€ Como Usar

1. Clone este repositÃ³rio
2. Copie `.opencode/agent/cybersecurity.md` para seu projeto em `.opencode/agent/`
3. Reinicie o opencode
4. Use o agente como subagent: `cybersecurity`

```bash
# No seu projeto
mkdir -p .opencode/agent
cp /path/to/Cyber-security-Agent/.opencode/agent/cybersecurity.md .opencode/agent/
# Reinicie o opencode
```

## âš ï¸ Importante
**SeguranÃ§a Ã© default, nÃ£o opcional.** Este agente nunca compromete seguranÃ§a por velocidade ou conveniÃªncia.
