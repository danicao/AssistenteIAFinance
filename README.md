🪙 FinanceAI - Gestão Financeira Inteligente com IA & Telegram
O FinanceAI é um ecossistema completo de gestão financeira pessoal e familiar. Ele combina um painel web responsivo (Dashboard) com um assessor financeiro pessoal no Telegram, operado por inteligência artificial multimodal (Google Gemini) rodando em uma infraestrutura serverless na Cloudflare e banco de dados PostgreSQL no Supabase.

🌟 Principais Funcionalidades
🤖 1. Assessor Financeiro Pessoal no Telegram
Entrada Multimodal:

✍️ Texto Livre: Digite frases naturais como "50 gasolina", "almoço 35 no débito" ou "recebi 400 de frete".

🎙️ Áudios de Voz: Envie gravações rápidas de voz na correria do dia a dia; a IA transcreve e extrai os valores automaticamente.

🧾 Visão Computacional (Fotos de Notinhas): Fotografe comprovantes ou cupons fiscais compridos. O sistema extrai o estabelecimento, data e valor total exato.

Débito e Crédito Bancário Automático: Toda despesa ou receita registrada pelo bot atualiza instantaneamente o saldo da sua Conta Bancária Principal.

Gestão de Investimentos Integrada: Ao falar "coloquei 500 no CDB do Inter 100% CDI", o valor é direcionado para a carteira de investimentos, sem poluir os gráficos de despesas.

Memória de Conversa & Correções: Permite correções imediatas (ex: "na verdade foi 45" ou "apaga esse último").

Prevenção de Duplicidade: Se enviar uma foto de notinha já computada nas últimas 24h, o bot avisa antes de duplicar.

Bloqueio de Escopo (Guardrail): Foco 100% em finanças. Dúvidas externas (jogos, receitas, amenidades) são educadamente recusadas.

Consultas Interativas com Emojis: Responde perguntas sobre saldo disponível, total investido em CDB, quanto falta para atingir o teto do mês e despesas fixas.

💻 2. Dashboard Web Completo
📊 Visão Geral Dinâmica: Indicadores de Receitas, Despesas, Saldo Disponível e Total Investido.

⭐ Contas Bancárias com Conta Principal: Suporte a múltiplos bancos e carteiras, permitindo marcar uma conta com selo de Conta Principal para as baixas do Telegram.

📈 Investimentos com Rendimento CDI: Cadastro e edição de aplicações (CDB, Tesouro Selic, LCI/LCA, FIIs, Ações) com cálculo diário de rendimento pelo CDI e alíquota regressiva de IR.

🗓️ Módulo de Despesas Fixas: Acompanhamento de contas recorrentes (aluguel, condomínio, internet, luz) com dia do vencimento.

🎯 Teto Universal de Gastos: Controle do teto padrão global (ex: R$ 1.300,00/mês) integrado ao ritmo de gastos por categoria.

🏆 Metas Financeiras: Acompanhamento visual de objetivos com barra de progresso e botão de aportes rápidos.

🔐 Painel Administrativo Master: Permite ao administrador gerenciar usuários e redefinir acessos sem violar a privacidade dos saldos e registros de cada conta.

🌓 Interface Moderna: Suporte completo a Modo Claro e Modo Escuro, design responsivo para smartphones e botão flutuante para abrir o bot do Telegram com 1 clique.

🏗️ Arquitetura do Sistema
Plaintext
[ Telegram App ]
  (Texto / Áudio / Foto)
          │
          ▼
[ Cloudflare Worker ] ── (Webhook Serverless)
          │
          ├──► [ Google Gemini Flash API ] ──► (Processamento de Linguagem Natural & Visão)
          │
          ▼
[ Supabase (PostgreSQL) ]
  ├── Autenticação & RLS (Segurança multi-usuário)
  ├── perfis_telegram (Vínculo de contas via código)
  ├── transacoes & contas_bancarias (Atualização de saldo em tempo real)
  ├── investimentos_renda_fixa & despesas_fixas
  └── historico_chat (Memória recente do bot)
          ▲
          │
[ Dashboard Web ] (HTML5 / TailwindCSS / Chart.js)
🗄️ Estrutura do Banco de Dados (Supabase)
O sistema opera com isolamento de dados por usuário utilizando Row Level Security (RLS):

contas_bancarias: Armazena bancos, saldos e a flag is_padrao para baixa automática.

transacoes: Lançamentos diários categorizados de receitas e despesas vinculados à conta.

despesas_fixas: Contas recorrentes mensais e dias de vencimento.

investimentos_renda_fixa: Registros de CDB, Tesouro, taxas de CDI e reserva de emergência.

limites_gastos: Orçamentos mensais divididos por categoria.

configuracoes_usuario: Guarda parâmetros globais, como o Teto Mensal Global.

perfis_telegram: Armazena o código de pareamento temporário e o telegram_chat_id.

historico_chat: Buffer de memória recente para suporte a correções e contexto.

🚀 Como Configurar e Rodar o Projeto
1. Banco de Dados (Supabase)
Crie um projeto gratuito em supabase.com.

Acesse o SQL Editor, abra uma aba em branco (+ New query) e execute o script de criação das tabelas e funções RPC de pareamento.

Em Project Settings > API, anote a URL, a chave pública anon e a chave administrativa service_role.

2. Criação do Bot no Telegram
No Telegram, converse com o @BotFather e crie um novo bot com o comando /newbot.

Guarde o HTTP API Token gerado.

3. Google Gemini API
Obtenha sua chave gratuita em aistudio.google.com.

A API suporta nativamente o modelo gemini-1.5-flash ou gemini-3.5-flash.

4. Deploy do Cloudflare Worker
Crie uma conta no Cloudflare Dashboard e crie um novo Worker.

Cole o código de worker.js, preenchendo as constantes com seus tokens e chaves:

TELEGRAM_BOT_TOKEN

GEMINI_API_KEY

SUPABASE_URL

SUPABASE_SERVICE_KEY

Clique em Deploy.

Configure o Webhook do Telegram abrindo o seguinte link no seu navegador:

Plaintext
https://api.telegram.org/bot<SEU_TELEGRAM_TOKEN>/setWebhook?url=https://<SEU_WORKER>.workers.dev
5. Configuração do Frontend Web
No arquivo index.html, atualize as variáveis no início do script:

JavaScript
const SUPABASE_URL = 'https://SUA_URL_SUPABASE.supabase.co';
const SUPABASE_KEY = 'SUA_CHAVE_PUBLICA_ANON';
const ADMIN_EMAIL = 'seu-email@dominio.com';
Hospede gratuitamente no GitHub Pages, Vercel ou rode localmente com a extensão Live Server.

📱 Pareando o Telegram com sua Conta
Abra o painel web e crie sua conta (ou faça login).

Clique no botão Conectar Telegram no cabeçalho.

Copie o comando exibido (ex: /conectar 123456).

Abra o robô no Telegram, envie o comando e receba a confirmação imediata de vínculo.

💬 Exemplos de Uso no Telegram
Anotar despesa: "50 reais gasolina" ou "almoço 32,50 no débito"

Lançar investimento: "Coloquei 400 reais no CDB do Inter a 100% do CDI"

Consultas: "Quanto já gastei esse mês?" / "Qual o meu saldo em conta?" / "Quanto tenho em CDB?"

Correções: "Na verdade o mercado foi 85"

Cancelamento: "Cancela o último lançamento"

🔒 Segurança e Privacidade
Isolamento Total: As políticas de Row Level Security (RLS) do PostgreSQL garantem que usuários distintos nunca tenham acesso às transações ou saldos uns dos outros.

Tokens Seguros: O webhook roda inteiramente em nuvem protegida (Cloudflare Workers) sem expor as chaves de serviço no código do cliente.

Controle Administrativo Cego: A interface administrativa gerencia apenas status de e-mail e cadastro, sem visualização de movimentações financeiras.
