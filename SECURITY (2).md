# Política de segurança

## Reportar uma vulnerabilidade

**Não abra uma issue pública** para falhas de segurança. Envie um e-mail para **joaocarvalhojp40@gmail.com** com:

- descrição do problema e o impacto;
- passos para reproduzir;
- versão/commit afetado;
- (opcional) sugestão de correção.

Responderemos o mais rápido possível. Pedimos que você aguarde a correção antes de divulgar o problema publicamente.

## Versões suportadas

| Versão | Suporte |
| --- | --- |
| 2.x (branch `main`) | ✅ |
| 1.x e anteriores | ❌ |

## Cuidados ao usar o bot

- **Nunca** envie o token do bot para o GitHub. O projeto ignora `.env` e `config/secrets.js` no `.gitignore` — não remova essas regras.
- Se o token vazar, vá em *Discord Developer Portal → Bot → Reset Token* **imediatamente** e atualize o `.env`.
- Não compartilhe prints ou logs com o token.
- Dê ao bot apenas as permissões necessárias e mantenha o cargo dele logo acima dos cargos que ele gerencia.
- Ative a verificação em duas etapas na conta dona do bot e do repositório.
- O repositório possui um workflow (`protect-signature.yml`) que bloqueia commits contendo tokens do Discord.

## O que NÃO é vulnerabilidade

- Uso do código sem créditos ou sem autorização → isso é violação de licença; reporte em **joaocarvalhojp40@gmail.com** (veja [BRAND.md](BRAND.md)).
