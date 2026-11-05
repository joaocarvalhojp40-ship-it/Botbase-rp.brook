# CNV Bot Base — Bot de Roleplay para Discord

Base de bot de Discord feita em **JavaScript (discord.js v14)** para comunidades de roleplay de **Brookhaven (Roblox)**. Todas as respostas usam **Components V2** com visual minimalista e tudo é configurável em `config/`.

> ⚠️ **Leia os [Termos de uso](#termos-de-uso-leia-antes-de-usar) antes de usar.** Este projeto é público, mas **não é MIT**: exige **créditos** e **revisão manual** do autor.

## Recursos

- **Whitelist** por modais (9 perguntas de RP + 1 situação) e **Allowhitelist** (criação de personagem, idade mínima 18).
- **Passaporte** automático com Nome, Idade, Nacionalidade e SSN (3 números), com a **foto do avatar do Roblox** (API pública) do jogador.
- Cargos por etapa (o cargo da etapa anterior é removido) e apelido **"Nome - SSN"** ao aprovar.
- `/aprove` e `/reprove` (permissão *Ver Registros do Servidor*) que detectam a etapa do jogador e avisam por **DM**.
- **Banco** (apenas no canal de banco): `/saldo`, `/enviar`, `/sacar`, `/comprovante` e, para banqueiros, `/transfer`, `/wage`, `/remove`. Toda movimentação gera um **ID de comprovante**.
- **Polícia**: `/fine`, `/carrying-a-weapon`, `/prison` com log no canal da polícia.
- **Tickets**: painel `$$up`, 1 ticket por usuário, cooldown, **criação de call de voz privada** e **log em PNG** (avatar, nome, imagens, emojis e figurinhas).
- `$$rp`: mensagem **Roleplay ON** com o link do servidor privado do Brookhaven.
- Status "Jogando …" com **convite permanente** gerado pelo próprio bot.

## Como usar

1. Faça um **fork** (ou clone) deste repositório.
2. `npm install`
3. Copie `.env.example` para `.env` e preencha `TOKEN` e `GUILD_ID` (ou use `config/secrets.js`).
4. Preencha **seus IDs** em `config/ids.js`, o nome do seu RP em `config/settings.js`, e seus banners/emojis em `config/visual.js` / `config/emojis.js`.
5. **Solicite a revisão manual** (veja abaixo) e aguarde a aprovação.
6. Hospede — veja o guia completo em **[DEPLOY.md](DEPLOY.md)** (Discloud, Termux e local).

## Comandos

| Comando | Quem usa | Onde |
| --- | --- | --- |
| `/saldo [usuario]` | Jogadores (banqueiro vê de qualquer um) | Canal de banco |
| `/enviar`, `/sacar`, `/comprovante` | Jogadores | Canal de banco |
| `/transfer`, `/wage`, `/remove` | Banqueiros | Canal de banco |
| `/fine`, `/carrying-a-weapon`, `/prison` | Policiais | Qualquer canal (o log vai ao canal da polícia) |
| `/aprove`, `/reprove` | Ver Registros do Servidor | Qualquer canal |
| `$$up`, `$$painel` | Moderador/Admin | Canal desejado |
| `$$rp` | Gerenciar Servidor | Canal desejado |

## Estrutura do projeto

```
index.js                 entrada
config/
  settings.js            nome do RP, prefixo, limites, link do Brookhaven, status
  ids.js                 IDs de cargos, canais e categorias
  emojis.js              seus emojis (vazio = sem emoji)
  visual.js              cores e SEUS banners
  questions.js           perguntas da whitelist, raças, tipos de porte
  credits.js             ⚠️ créditos (protegido)
  secrets.example.js     modelo do config/secrets.js (alternativa ao .env)
src/
  commands/{bank,police,staff}   slash commands (um arquivo por comando)
  events/                        ready, interactionCreate, messageCreate
  interactions/                  botões, modais e selects (whitelist, character, ticket)
  prefix/                        $$up, $$painel, $$rp
  services/                      db, bank, roblox, invite, review, police, transcript, credits, checkConfig
  utils/                         ui (Components V2), perms, format, options
assets/fonts/                    fontes do log PNG (Inter e DejaVu Sans)
```

## Desenvolvimento

- **Carregamento automático** (`src/loader.js`): tudo em `commands/`, `interactions/`, `prefix/` e `events/` é registrado sozinho.
- **Novo slash command** — crie `src/commands/<grupo>/nome.js`:

  ```js
  const { SlashCommandBuilder } = require('discord.js');
  const { card, out } = require('../../utils/ui');

  module.exports = {
    group: 'bank',            // 'bank' (só no canal do banco) | 'police' (exige cargo) | 'staff'
    // banker: true,          // exige banqueiro (apenas no grupo 'bank')
    data: new SlashCommandBuilder().setName('ping').setDescription('Teste'),
    run: (i) => i.reply(out(card({ title: 'Pong', text: 'Funcionando.' }))),
  };
  ```
- **Botões/modais/selects** — exporte `{ id: 'meu_custom_id', run(i) {} }` (ou uma lista) em `src/interactions/`.
- **Comandos de prefixo** — exporte `{ name, allow(member), run(message) }` em `src/prefix/`.
- **Visual** — use sempre `card()` e `out()` (`src/utils/ui.js`): geram Components V2 (Container + texto + banner + botões). Mensagens V2 não aceitam `content`/`embeds`.
- **Dados** — `db.json` (criado automaticamente): `users` (etapa, saldo, personagem, SSN), `tx` (comprovantes), `calls`, `cooldowns`, `invite`. Etapas: `none → wl_pending → wl_ok → al_pending → done`.
- **Logs de erro** — problemas de configuração aparecem no console como `❌ BUG/CONFIG: …` com a causa e a correção.
- **Requisitos** — Node.js 18+ (recomendado 20+). O log em PNG usa `@napi-rs/canvas`; se ele não estiver disponível na sua hospedagem, o log do ticket sai em `.txt`.

## Termos de uso (leia antes de usar)

Este projeto é público, mas **não é MIT**. Ele usa a licença do arquivo [`LICENSE`](LICENSE), que exige:

- **Créditos no código e na biografia do bot.** Não remova nem altere `config/credits.js`, o cabeçalho dos arquivos, o `LICENSE` nem este README. O bot adiciona o crédito à biografia automaticamente — não desative.
- **Revisão manual do autor antes do uso.** Envie um e-mail para **joaocarvalhojp40@gmail.com** com o link do seu fork e a lista de alterações. O uso em produção só é permitido após aprovação por escrito.
- **Modifique apenas os itens necessários** (arquivos de `config/` e segredos). Alterar a lógica em `src/` exige nova aprovação.
- **Banners, emojis, nome "Cidade Nova Roleplay", "CNV", logo e textos são de uso exclusivo da Hyper Squad** e não podem ser usados. Veja [`BRAND.md`](BRAND.md).
- **Não é permitido** vender, redistribuir como produto, alegar autoria ou contornar as proteções do repositório.
- **Contribuições** (pull requests) exigem aprovação manual do autor.

O descumprimento encerra automaticamente a licença.

## Segurança

Veja [`SECURITY.md`](SECURITY.md). **Nunca** envie o token do bot para o GitHub — `.env` e `config/secrets.js` já estão no `.gitignore`.

## Código de conduta

Veja [`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md).

## Contribuir, suporte e avisos

- Contribuições: [`CONTRIBUTING.md`](CONTRIBUTING.md)
- Suporte: [`SUPPORT.md`](SUPPORT.md)
- Histórico de versões: [`CHANGELOG.md`](CHANGELOG.md)
- Licenças de terceiros (fontes, bibliotecas, Twemoji): [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)

## Autor e contato

Feito por **eljoaogg** · **Hyper Squad** — [GitHub](https://github.com/joaocarvalhojp40-ship-it) · joaocarvalhojp40@gmail.com
