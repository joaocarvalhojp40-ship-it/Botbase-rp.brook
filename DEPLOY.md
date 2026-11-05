# Como hospedar o bot

Guia para rodar o **CNV Bot Base** na **Discloud**, no **Termux** (Android) ou **localmente** (Windows, Linux e macOS).

> ⚠️ Antes de colocar o bot em produção, leia os [Termos de uso](README.md#termos-de-uso-leia-antes-de-usar): é obrigatório manter os **créditos** e ter a **revisão manual** do autor (joaocarvalhojp40@gmail.com).

## 1. Criar o bot no Discord (vale para todas as hospedagens)

1. Acesse o [Discord Developer Portal](https://discord.com/developers/applications) → **New Application**.
2. Aba **Bot** → **Reset Token** → copie o token (guarde em segredo!).
3. Na mesma aba, em **Privileged Gateway Intents**, ligue **MESSAGE CONTENT INTENT** (necessário para `$$up`, `$$painel` e `$$rp`).
4. Aba **OAuth2 → General**: copie o **Client ID**.
5. Convide o bot para o seu servidor (troque `SEU_CLIENT_ID`):

   ```
   https://discord.com/oauth2/authorize?client_id=SEU_CLIENT_ID&scope=bot%20applications.commands&permissions=8
   ```

   `permissions=8` (Administrador) é o jeito mais simples. Se preferir permissões mínimas, marque: *Ver canais, Enviar mensagens, Ler histórico, Anexar arquivos, Gerenciar canais, Gerenciar cargos, Gerenciar apelidos, Criar convite e Mencionar @everyone/@here*.
6. No servidor, ative o **Modo Desenvolvedor** (Configurações → Avançado) para copiar IDs com *botão direito → Copiar ID*.
7. Em **Configurações do servidor → Cargos**, arraste o cargo do bot para **acima** dos cargos Whitelist, Personagem e Aprovado (senão ele não consegue dá-los/removê-los nem mudar apelidos).

## 2. Configurar o projeto

1. **Segredos** — escolha **uma** opção:
   - **A) `.env`**: copie `.env.example` para `.env` e preencha `TOKEN` e `GUILD_ID` (ID do servidor).
   - **B) `config/secrets.js`**: copie `config/secrets.example.js` para `config/secrets.js` e preencha `token` e `guildId`.
2. **IDs** — preencha todos os campos de `config/ids.js` (cargos, canais e categoria).
3. **Seu roleplay** — em `config/settings.js`: `serverName`, `presence.text`, `rp.privateServerLink` (link do servidor privado do Brookhaven) e demais opções.
4. **Visual** — `config/visual.js` (**seus** banners) e `config/emojis.js` (**seus** emojis). Os recursos da Hyper Squad não podem ser usados ([BRAND.md](BRAND.md)).

> Nunca envie `.env` nem `config/secrets.js` para o GitHub (já estão no `.gitignore`).

## 3. Discloud

1. Crie sua conta em [discloud.com](https://discloud.com) e tenha um plano com RAM suficiente (o `discloud.config` pede **256 MB**, por causa do log em PNG).
2. Abra o `discloud.config` e troque `NAME` pelo nome do seu bot:

   ```
   NAME=MeuBotRP
   TYPE=bot
   MAIN=index.js
   RAM=256
   VERSION=latest
   AUTORESTART=true
   ```
3. Configure os segredos. **Na Discloud prefira a opção B (`config/secrets.js`)**: arquivos que começam com ponto (`.env`) costumam ficar de fora do `.zip` no Windows/macOS.
4. Crie um `.zip` **com o conteúdo da pasta** — `index.js`, `package.json` e `discloud.config` precisam ficar na **raiz** do zip (não dentro de uma subpasta). **Não inclua** `node_modules`.
5. No painel da Discloud, faça o **upload do .zip** (ver [docs.discloud.com](https://docs.discloud.com) para os demais métodos de envio). A Discloud instala as dependências sozinha.
6. Abra os **logs** do app. Deve aparecer, nesta ordem: `📦 CNV Bot Base`, `© CNV Bot Base — …`, `🔑 Token carregado`, `✅ … online.`, `✅ 12 comandos registrados.`
7. **Atualizar o bot**: envie o novo `.zip` pelo painel. **Faça backup do `db.json`** antes (ele guarda saldos, personagens e comprovantes) e inclua-o no novo zip.

## 4. Local (Windows, Linux e macOS)

1. Instale o [Node.js](https://nodejs.org) **LTS** (18 ou superior; recomendado 20+) e o [Git](https://git-scm.com).
2. Baixe o projeto e instale as dependências:

   ```bash
   git clone https://github.com/SEU_USUARIO/SEU_FORK.git
   cd SEU_FORK
   npm install
   ```
3. Configure os segredos (`.env` ou `config/secrets.js`) e os IDs (seção 2).
4. Inicie:

   ```bash
   npm start
   ```
5. **Manter ligado** (opcional, com [pm2](https://pm2.keymetrics.io)):

   ```bash
   npm install -g pm2
   pm2 start index.js --name meu-bot-rp
   pm2 save
   pm2 logs meu-bot-rp
   ```

   No Windows, o bot fica ligado enquanto o computador estiver ligado; para 24h, use uma VPS ou a Discloud.

## 5. Termux (Android)

1. Instale o Termux pelo **[F-Droid](https://f-droid.org/packages/com.termux/)** (a versão da Play Store é antiga e não recebe pacotes novos).
2. Atualize e instale o necessário:

   ```bash
   pkg update -y && pkg upgrade -y
   pkg install -y nodejs-lts git nano
   ```
3. Baixe o projeto e instale as dependências:

   ```bash
   git clone https://github.com/SEU_USUARIO/SEU_FORK.git
   cd SEU_FORK
   npm install
   ```
4. Configure os segredos e os IDs:

   ```bash
   cp .env.example .env
   nano .env            # preencha TOKEN e GUILD_ID  (salvar: Ctrl+O, Enter; sair: Ctrl+X)
   nano config/ids.js   # preencha os IDs
   ```
5. Inicie:

   ```bash
   npm start
   ```
6. **Manter ligado com a tela apagada**:

   ```bash
   pkg install -y tmux termux-api
   termux-wake-lock          # impede o Android de "dormir" o Termux
   tmux new -s bot           # abre uma sessão
   npm start                 # inicie o bot dentro do tmux
   # para sair sem parar o bot: Ctrl+B, depois D   |   para voltar: tmux attach -t bot
   ```

   Desative a **otimização de bateria** do Termux nas configurações do Android. O Android pode encerrar o app em segundo plano — para 24h, prefira a Discloud ou uma VPS.
7. Se o `npm install` não conseguir instalar o `@napi-rs/canvas` no seu aparelho, o bot continua funcionando e o **log do ticket sai em `.txt`** (aparece um aviso no console).

## 6. Problemas comuns

| O que aparece no log | O que fazer |
| --- | --- |
| `❌ CONFIG: token ausente ou inválido` | O `.env`/`config/secrets.js` não foi lido. Confira o nome do arquivo e o conteúdo (`TOKEN=...` sem espaços). Na Discloud use `config/secrets.js`. |
| `❌ CONFIG: GUILD_ID ausente ou inválido` | Coloque o ID numérico do servidor (Modo Desenvolvedor → Copiar ID). |
| `o Discord recusou o token` | O token foi resetado ou está errado. Gere um novo em *Bot → Reset Token*. |
| `"Message Content Intent" desligado` | Developer Portal → Bot → ligue **MESSAGE CONTENT INTENT** e reinicie. |
| `Falha ao registrar comandos: 403` / `Missing Access` | O bot não está no servidor ou foi convidado sem `applications.commands`. Convide de novo com o link da seção 1. |
| `⚠️ CONFIG: IDs não preenchidos em config/ids.js → …` | Preencha os IDs listados. |
| `não consegui add/remove cargos` | Coloque o cargo do bot **acima** dos cargos gerenciados e dê *Gerenciar Cargos*. |
| `não consegui mudar o apelido` | Dê *Gerenciar Apelidos* ao bot. Dono do servidor e cargos acima do bot não podem ser alterados. |
| `não consegui enviar no canal …` | ID do canal errado ou o bot sem permissão de ver/enviar nele. |
| `não consegui criar o ticket` / `a call` | Confira o ID da categoria e a permissão *Gerenciar Canais* do bot. |
| `falha na API do Roblox` | A API pública do Roblox pode estar instável ou bloqueada na sua rede/hospedagem; tente de novo. |
| `LICENÇA: os créditos … foram removidos ou alterados` | Restaure o `config/credits.js` original. O bot não inicia sem os créditos (veja o [LICENSE](LICENSE)). |

## 7. Checklist final

- [ ] Token e `GUILD_ID` configurados (e **fora** do GitHub).
- [ ] `config/ids.js` totalmente preenchido.
- [ ] Nome, banners e emojis **próprios** (nada da Hyper Squad).
- [ ] Créditos intactos (`config/credits.js`, cabeçalhos, `LICENSE`).
- [ ] **Revisão manual aprovada** pelo autor (joaocarvalhojp40@gmail.com).
- [ ] Cargo do bot acima dos cargos gerenciados.
- [ ] `$$painel` e `$$up` enviados nos canais certos.
