# Changelog

Formato baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/).

## [2.0.0] — 2026-11-05

### Adicionado
- Estrutura modular: `config/`, `src/commands`, `src/events`, `src/interactions`, `src/prefix`, `src/services`, `src/utils`.
- Respostas em **Components V2**, visual minimalista e monocromático.
- Banco com **ID de comprovante** (`/comprovante`) e `/saldo` com seleção de usuário para banqueiros.
- Passaporte com foto do avatar do **Roblox** (API pública), cargos por etapa e apelido "Nome - SSN".
- Tickets com **log em PNG**, criação de **call de voz** privada e gerenciamento de acesso.
- `$$rp` (Roleplay ON), status com convite permanente gerado pelo bot.
- Créditos obrigatórios (código + biografia do bot), arquivos de proteção e documentação de hospedagem (Discloud, Termux e local).

### Alterado
- Token e ID do servidor agora vêm do `.env` ou de `config/secrets.js` (nunca versionados).
- Banners e emojis removidos do projeto (pertencem à Hyper Squad).
