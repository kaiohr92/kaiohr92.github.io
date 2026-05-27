
<!--
  ============================================================
  FAQ Sicoob — Pronto para SharePoint
  ============================================================
  COMO USAR NO SHAREPOINT:
  Opção A (Clássico): Editar página → Inserir → "Script Editor Web Part"
                       → colar o conteúdo entre <body>...</body>
  Opção B (Moderno):  Usar Web Part "Incorporar" ou "HTML"
                       → colar o HTML completo
  Opção C (SPFX):     Criar extensão ApplicationCustomizer ou
                       WebPart e renderizar este HTML no render()
  ============================================================
-->
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FAQ — Sicoob Central de Ajuda</title>
  <!-- Ícones Tabler (gratuitos, CDN) -->
  <link rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.19.0/dist/tabler-icons.min.css" />

  <style>
    /* ===== Reset & Base ===== */
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      background: #F0F2F0;
      color: #222;
      min-height: 100vh;
    }

    /* ===== Tokens Sicoob ===== */
    :root {
      --green-dark:   #006633;
      --green-main:   #00843D;
      --green-light:  #009648;
      --green-pale:   #E8F5EE;
      --green-border: #B3D9C3;
      --yellow:       #FFD100;
      --yellow-dark:  #C9A500;
      --gray-text:    #4A4A4A;
      --gray-muted:   #777;
      --white:        #FFFFFF;
      --radius:       10px;
      --radius-lg:    16px;
      --shadow-sm:    0 1px 4px rgba(0,0,0,.08);
      --shadow-md:    0 4px 16px rgba(0,102,51,.10);
    }

    /* ===== Layout ===== */
    .faq-page {
      max-width: 880px;
      margin: 0 auto;
      padding: 0 16px 4rem;
    }

    /* ===== Hero ===== */
    .faq-hero {
      background: linear-gradient(135deg, #005529 0%, var(--green-main) 55%, var(--green-light) 100%);
      border-radius: 0 0 var(--radius-lg) var(--radius-lg);
      padding: 2.5rem 2rem 2.2rem;
      position: relative;
      overflow: hidden;
      margin-bottom: 1.6rem;
    }
    .faq-hero::before {
      content: '';
      position: absolute; top: -40px; right: -40px;
      width: 180px; height: 180px; border-radius: 50%;
      background: rgba(255,209,0,.12);
    }
    .faq-hero::after {
      content: '';
      position: absolute; bottom: -60px; left: 8%;
      width: 220px; height: 220px; border-radius: 50%;
      background: rgba(255,255,255,.05);
    }
    .logo-row {
      display: flex; align-items: center; gap: 12px;
      margin-bottom: 1.4rem; position: relative; z-index: 1;
    }
    .logo-badge {
      width: 42px; height: 42px; border-radius: 9px;
      background: var(--yellow);
      display: flex; align-items: center; justify-content: center;
      font-weight: 800; font-size: 16px; color: var(--green-dark);
      letter-spacing: -1px; flex-shrink: 0;
    }
    .logo-info { line-height: 1.25; }
    .logo-name  { font-size: 22px; font-weight: 700; color: #fff; }
    .logo-tagline { font-size: 11.5px; color: rgba(255,255,255,.7); margin-top: 1px; }
    .hero-title { font-size: 28px; font-weight: 600; color: #fff; margin-bottom: 6px; position: relative; z-index:1; }
    .hero-sub   { font-size: 14px; color: rgba(255,255,255,.8); position: relative; z-index:1; }

    /* ===== Barra de busca ===== */
    .search-wrap {
      margin-top: 1.4rem; position: relative; z-index: 1;
    }
    .search-wrap input {
      width: 100%; padding: 13px 48px 13px 18px;
      border-radius: var(--radius); border: none;
      font-size: 14px; color: #222; outline: none;
      box-shadow: 0 4px 18px rgba(0,0,0,.15);
      font-family: inherit;
    }
    .search-wrap input::placeholder { color: #999; }
    .search-wrap input:focus { box-shadow: 0 0 0 3px rgba(0,134,61,.3); }
    .search-icon {
      position: absolute; right: 16px; top: 50%;
      transform: translateY(-50%);
      color: var(--green-main); font-size: 20px; pointer-events: none;
    }

    /* ===== Filtros de categoria ===== */
    .cats {
      display: flex; gap: 8px; flex-wrap: wrap;
      padding: 4px 0 12px;
    }
    .cat-btn {
      display: flex; align-items: center; gap: 6px;
      padding: 7px 15px; border-radius: 20px;
      border: 1.5px solid #ccc;
      background: #fff;
      font-size: 13px; cursor: pointer;
      color: #555; transition: all .15s;
      font-family: inherit;
    }
    .cat-btn:hover { border-color: var(--green-main); color: var(--green-main); }
    .cat-btn.active {
      background: var(--green-dark); border-color: var(--green-dark); color: #fff;
    }
    .cat-btn i { font-size: 15px; }

    /* ===== Contagem ===== */
    .result-count {
      font-size: 12.5px; color: var(--gray-muted);
      margin-bottom: 10px; padding: 0 2px;
    }

    /* ===== Seções ===== */
    .faq-section { margin-bottom: 1.8rem; }
    .section-label {
      display: flex; align-items: center; gap: 8px;
      font-size: 12px; font-weight: 700;
      color: var(--green-dark);
      text-transform: uppercase; letter-spacing: 1px;
      margin-bottom: 10px; padding: 0 2px;
    }
    .section-label::after {
      content: ''; flex: 1; height: 1px; background: var(--green-border);
    }

    /* ===== Item FAQ ===== */
    .faq-item {
      background: var(--white);
      border: 1px solid #e4e4e4;
      border-radius: var(--radius);
      margin-bottom: 6px;
      overflow: hidden;
      transition: box-shadow .2s, border-color .2s;
    }
    .faq-item:hover   { box-shadow: var(--shadow-md); }
    .faq-item.open    { border-color: rgba(0,102,51,.3); box-shadow: var(--shadow-md); }

    .faq-q {
      display: flex; align-items: center; gap: 12px;
      padding: 14px 16px; cursor: pointer; user-select: none;
    }
    .faq-q-icon {
      width: 34px; height: 34px; border-radius: 8px; flex-shrink: 0;
      background: var(--green-pale);
      display: flex; align-items: center; justify-content: center;
      color: var(--green-dark); font-size: 16px;
      transition: background .2s, color .2s;
    }
    .faq-item.open .faq-q-icon { background: var(--green-dark); color: #fff; }
    .faq-q-text {
      flex: 1; font-size: 14px; font-weight: 500; line-height: 1.4; color: #1a1a1a;
    }
    .badge {
      display: inline-block; padding: 2px 8px; border-radius: 4px;
      font-size: 11px; font-weight: 600;
      background: #FFF6CC; color: #8A6700; margin-left: 8px;
      vertical-align: middle;
    }
    .faq-chevron {
      font-size: 17px; color: #aaa;
      transition: transform .22s, color .15s; flex-shrink: 0;
    }
    .faq-item.open .faq-chevron { transform: rotate(180deg); color: var(--green-dark); }

    /* ===== Resposta ===== */
    .faq-a {
      max-height: 0; overflow: hidden;
      transition: max-height .3s ease;
    }
    .faq-item.open .faq-a { max-height: 500px; }
    .faq-a-inner {
      padding: 12px 16px 16px 62px;
      border-top: 1px solid #eee;
      font-size: 13.5px; line-height: 1.68;
      color: #555;
    }
    .faq-a-inner strong { color: var(--green-dark); font-weight: 600; }
    .faq-a-inner a { color: var(--green-main); text-decoration: underline; }

    /* ===== Sem resultados ===== */
    .no-results {
      text-align: center; padding: 3.5rem 1rem;
      color: var(--gray-muted); display: none;
    }
    .no-results i { font-size: 40px; display: block; margin-bottom: 12px; opacity: .35; }
    .no-results p { font-size: 14px; }

    /* ===== Barra de contato ===== */
    .contact-bar {
      background: var(--green-pale);
      border: 1px solid var(--green-border);
      border-radius: var(--radius);
      padding: 1.1rem 1.4rem;
      display: flex; align-items: center; gap: 16px;
      margin-top: 2rem; flex-wrap: wrap;
    }
    .contact-bar i { font-size: 26px; color: var(--green-dark); flex-shrink: 0; }
    .contact-bar-text { flex: 1; min-width: 200px; }
    .contact-bar-text strong { font-size: 14.5px; color: var(--green-dark); display: block; margin-bottom: 2px; }
    .contact-bar-text span  { font-size: 12.5px; color: #3a7a55; }
    .contact-btn {
      padding: 10px 20px; background: var(--green-dark); color: #fff;
      border: none; border-radius: 8px;
      font-size: 13px; font-weight: 600; cursor: pointer;
      font-family: inherit; white-space: nowrap;
      transition: background .15s;
    }
    .contact-btn:hover { background: var(--green-main); }

    /* ===== Responsivo ===== */
    @media (max-width: 600px) {
      .faq-hero       { padding: 1.8rem 1.2rem 1.6rem; }
      .hero-title     { font-size: 22px; }
      .faq-a-inner    { padding-left: 16px; }
    }
  </style>
</head>
<body>

<div class="faq-page" role="main">

  <!-- === HERO === -->
  <header class="faq-hero">
    <div class="logo-row">
      <div class="logo-badge" aria-hidden="true">SC</div>
      <div class="logo-info">
        <div class="logo-name">Sicoob</div>
        <div class="logo-tagline">Sistema de Cooperativas de Crédito do Brasil</div>
      </div>
    </div>
    <h1 class="hero-title">Central de Ajuda — FAQ</h1>
    <p class="hero-sub">Encontre respostas rápidas sobre produtos e serviços do Sicoob</p>
    <div class="search-wrap">
      <input
        type="search"
        id="searchInput"
        placeholder="Buscar pergunta... ex: Pix, empréstimo, cartão..."
        aria-label="Buscar perguntas frequentes"
        autocomplete="off"
      />
      <i class="ti ti-search search-icon" aria-hidden="true"></i>
    </div>
  </header>

  <!-- === FILTROS === -->
  <nav class="cats" id="catBar" aria-label="Filtrar por categoria">
    <button class="cat-btn active" data-cat="all"        aria-pressed="true"><i class="ti ti-layout-grid" aria-hidden="true"></i> Todas</button>
    <button class="cat-btn"       data-cat="conta"       aria-pressed="false"><i class="ti ti-building-bank" aria-hidden="true"></i> Conta</button>
    <button class="cat-btn"       data-cat="pix"         aria-pressed="false"><i class="ti ti-bolt" aria-hidden="true"></i> Pix</button>
    <button class="cat-btn"       data-cat="cartao"      aria-pressed="false"><i class="ti ti-credit-card" aria-hidden="true"></i> Cartão</button>
    <button class="cat-btn"       data-cat="credito"     aria-pressed="false"><i class="ti ti-cash" aria-hidden="true"></i> Crédito</button>
    <button class="cat-btn"       data-cat="app"         aria-pressed="false"><i class="ti ti-device-mobile" aria-hidden="true"></i> App</button>
    <button class="cat-btn"       data-cat="cooperativa" aria-pressed="false"><i class="ti ti-users" aria-hidden="true"></i> Cooperativa</button>
    <button class="cat-btn"       data-cat="seguranca"   aria-pressed="false"><i class="ti ti-shield-lock" aria-hidden="true"></i> Segurança</button>
  </nav>

  <div class="result-count" id="resultCount" aria-live="polite"></div>

  <!-- === LISTA === -->
  <div id="faqList"></div>

  <!-- === SEM RESULTADOS === -->
  <div class="no-results" id="noResults" role="alert">
    <i class="ti ti-search-off" aria-hidden="true"></i>
    <p>Nenhum resultado encontrado para sua busca.<br>Tente outras palavras-chave ou explore as categorias acima.</p>
  </div>

  <!-- === BARRA CONTATO === -->
  <div class="contact-bar" role="complementary" aria-label="Canal de atendimento">
    <i class="ti ti-headset" aria-hidden="true"></i>
    <div class="contact-bar-text">
      <strong>Não encontrou o que precisava?</strong>
      <span>Fale pelo chat do app, WhatsApp ou ligue: <strong>0800 642 2001</strong> (24h, gratuito)</span>
    </div>
    <button class="contact-btn" onclick="window.open('https://www.sicoob.com.br/web/sicoob/atendimento','_blank')">
      Acessar atendimento →
    </button>
  </div>

</div><!-- /faq-page -->

<script>
/* ============================================================
   BANCO DE PERGUNTAS
   Para adicionar: inclua um novo objeto no array abaixo.
   Campos: cat, icon (classe Tabler), q, a (HTML permitido), tag (opcional)
   Categorias disponíveis: conta | pix | cartao | credito | app | cooperativa | seguranca
   ============================================================ */
const FAQS = [

  // ── CONTA ──────────────────────────────────────────────
  {
    cat: "conta", icon: "ti-building-bank",
    q: "Como abro uma conta no Sicoob?",
    a: "Para abrir sua conta, acesse o site <a href='https://www.sicoob.com.br' target='_blank'>sicoob.com.br</a> ou vá a uma cooperativa mais próxima. São necessários: RG ou CNH, CPF, comprovante de residência e de renda. Pessoas Jurídicas devem apresentar também CNPJ e contrato social. A abertura é gratuita para cooperados."
  },
  {
    cat: "conta", icon: "ti-key",
    q: "Como recuperar o acesso à minha conta digital?",
    a: "Abra o app Sicoob e toque em <strong>'Esqueci minha senha'</strong>. A recuperação pode ser feita via CPF + data de nascimento ou por reconhecimento biométrico. Se o problema persistir, ligue para <strong>0800 642 2001</strong> ou compareça à cooperativa com documento de identidade original."
  },
  {
    cat: "conta", icon: "ti-file-description",
    q: "Como obter extrato e comprovante bancário?",
    a: "Pelo app ou Internet Banking você acessa extratos dos últimos 90 dias. Para períodos maiores, solicite na cooperativa. Todos os comprovantes ficam disponíveis em PDF no app, em <strong>Histórico de transações</strong>."
  },
  {
    cat: "conta", icon: "ti-calendar-dollar",
    q: "Qual o horário de funcionamento das transferências TED/DOC?",
    a: "TED: dias úteis das <strong>06h30 às 17h00</strong> (crédito no mesmo dia) ou até 22h59 (crédito no próximo dia útil). DOC: até 21h59 com crédito no próximo dia útil. O <strong>Pix</strong> funciona 24h/7 dias e substituiu o DOC desde março de 2024."
  },
  {
    cat: "conta", icon: "ti-receipt",
    q: "Como solicitar segunda via do cartão de débito?",
    a: "No app acesse <strong>Cartões → Cartão de débito → Solicitar segunda via</strong>. O cartão é enviado ao endereço cadastrado em até 10 dias úteis. Taxa de R$ 12,00 pode ser cobrada conforme seu plano de tarifas — verifique no extrato de tarifas disponível no app."
  },

  // ── PIX ────────────────────────────────────────────────
  {
    cat: "pix", icon: "ti-bolt",
    q: "Como cadastrar uma chave Pix no Sicoob?",
    a: "No app, acesse <strong>Pix → Minhas Chaves → Cadastrar nova chave</strong>. Você pode usar CPF/CNPJ, e-mail, telefone celular ou chave aleatória. O cadastro é instantâneo. Pessoa Física pode ter até 5 chaves; Pessoa Jurídica até 20 por CNPJ."
  },
  {
    cat: "pix", icon: "ti-clock",
    q: "Qual o horário e limite do Pix no Sicoob?",
    a: "O Pix funciona <strong>24 horas por dia, 7 dias por semana</strong>. Limite padrão: R$ 5.000 no período diurno e R$ 1.000 no período noturno (20h às 6h). Você pode ajustar seus limites no app em <strong>Pix → Configurações → Meus Limites</strong>, sujeito a análise."
  },
  {
    cat: "pix", icon: "ti-clock-off",
    q: "Fiz um Pix errado. Como solicitar devolução?",
    a: "O Pix não pode ser cancelado após a confirmação. Para solicitar devolução: app → <strong>Pix → Histórico → selecione a transação → Solicitar devolução</strong>. O destinatário tem até 90 dias para aceitar. Em caso de suspeita de fraude, registre um BO e acione a cooperativa imediatamente."
  },
  {
    cat: "pix", icon: "ti-qrcode",
    q: "O que é o Pix Cobrança e como usar?",
    a: "O <strong>Pix Cobrança</strong> permite emitir QR Codes com vencimento, desconto, juros por atraso e multa — ideal para cobrar clientes. No app: <strong>Pix → Cobrar → Pix Cobrança</strong>. Disponível para Pessoa Jurídica e MEI. O pagador vê todas as condições antes de confirmar."
  },
  {
    cat: "pix", icon: "ti-calendar-repeat",
    q: "O que é o Pix Agendado e como funciona?",
    a: "Permite programar um Pix para data futura. No app, ao realizar uma transferência, selecione <strong>'Agendar'</strong> e escolha a data. Você pode cancelar o agendamento a qualquer momento antes do processamento, em <strong>Pix → Agendamentos</strong>."
  },

  // ── CARTÃO ─────────────────────────────────────────────
  {
    cat: "cartao", icon: "ti-credit-card",
    q: "Como solicitar o cartão de crédito Sicoob?",
    a: "Acesse o app → <strong>Cartões → Solicitar cartão de crédito</strong>. A análise é feita em até 5 dias úteis. O cartão (bandeira <strong>Mastercard</strong>) é enviado ao endereço cadastrado em até 10 dias. É necessário ser cooperado com conta ativa."
  },
  {
    cat: "cartao", icon: "ti-eye-off",
    q: "Como bloquear, desbloquear ou cancelar meu cartão?",
    a: "No app: <strong>Cartões → selecione o cartão → Gerenciar</strong>. Você pode bloquear temporariamente (suspeita de perda), desbloquear ou solicitar cancelamento com pedido de novo cartão. Emergências: <strong>0800 642 2001</strong> (24h)."
  },
  {
    cat: "cartao", icon: "ti-repeat",
    q: "Como parcelar compras no cartão Sicoob?",
    a: "Compras acima de R$ 100 podem ser parceladas em até <strong>24 vezes</strong> no crédito. O parcelamento pode ocorrer no momento da compra (no estabelecimento) ou depois pelo app: <strong>Cartões → Faturas → Parcelar compra</strong>. Taxas de juros variam conforme o perfil do cooperado."
  },
  {
    cat: "cartao", icon: "ti-trending-up",
    q: "Como aumentar o limite do meu cartão de crédito?",
    a: "Solicite pelo app em <strong>Cartões → Limite → Solicitar aumento</strong>. A análise considera histórico de pagamentos, renda e tempo como cooperado. Outra opção é solicitar limite garantido vinculado a depósito — aprovação em até 2 dias úteis."
  },
  {
    cat: "cartao", icon: "ti-rotate-clockwise",
    q: "Como funciona o pagamento mínimo da fatura?",
    a: "O pagamento mínimo é de <strong>15% do valor total da fatura</strong> (ou o valor integral se menor que R$ 10). Ao pagar o mínimo, o saldo restante é financiado com juros rotativo. Para evitar juros, pague sempre o valor integral até o vencimento."
  },

  // ── CRÉDITO ────────────────────────────────────────────
  {
    cat: "credito", icon: "ti-cash",
    q: "Quais modalidades de crédito o Sicoob oferece?",
    a: "O Sicoob oferece: <strong>crédito pessoal, crédito consignado, crédito rural, microcrédito, financiamento de veículos e imóveis, antecipação de FGTS e capital de giro</strong> para empresas. As taxas são diferenciadas para cooperados e variam por cooperativa."
  },
  {
    cat: "credito", icon: "ti-clipboard-list",
    q: "Como solicitar um empréstimo pessoal?",
    a: "No app: <strong>Crédito → Empréstimo pessoal → Simular</strong>. Informe o valor e prazo para ver as condições. Se aprovada a simulação, prossiga com a contratação digital. Documentos geralmente necessários: RG, CPF e comprovante de renda atualizado."
  },
  {
    cat: "credito", icon: "ti-money",
    q: "Como funciona a antecipação do 13º salário e IR?",
    a: "A antecipação do <strong>13º salário</strong> fica disponível a partir de maio de cada ano. Para restituição de IRPF, a antecipação pode ser feita após a entrega da declaração ao fisco. Ambas são acessadas pelo app em <strong>Crédito → Ver ofertas disponíveis</strong>."
  },
  {
    cat: "credito", icon: "ti-home",
    q: "O Sicoob financia imóveis?",
    a: "Sim. O <strong>Crédito Habitacional Sicoob</strong> financia imóveis residenciais novos ou usados com prazo de até 360 meses (30 anos) e taxa a partir de 9,5% a.a. mais TR. É possível usar o FGTS como entrada. Simule no app ou em qualquer agência da cooperativa."
  },

  // ── APP & INTERNET BANKING ─────────────────────────────
  {
    cat: "app", icon: "ti-device-mobile",
    q: "O app Sicoob está disponível para iOS e Android?",
    a: "Sim, o <strong>app Sicoob</strong> está disponível gratuitamente na <strong>App Store</strong> (iOS 14+) e na <strong>Google Play</strong> (Android 8+). Busque por 'Sicoob'. O login é feito com CPF e senha, com suporte a biometria e reconhecimento facial."
  },
  {
    cat: "app", icon: "ti-refresh",
    q: "O app está apresentando erros. O que devo fazer?",
    a: "Tente: (1) fechar e reabrir o app; (2) verificar conexão de internet; (3) verificar se há atualização na loja; (4) desinstalar e reinstalar. Se persistir, acesse pelo <strong>Internet Banking</strong> em <a href='https://sicoob.com.br' target='_blank'>sicoob.com.br</a> ou ligue <strong>0800 642 2001</strong>."
  },
  {
    cat: "app", icon: "ti-fingerprint",
    q: "Como ativar o acesso por biometria no app?",
    a: "Faça login → <strong>Perfil → Segurança → Biometria → Habilitar</strong>. O dispositivo precisa ter biometria configurada no sistema operacional. Para desativar, repita o caminho e toque em 'Desativar'. O PIN continua como alternativa."
  },
  {
    cat: "app", icon: "ti-signal",
    q: "Como acessar o Internet Banking Sicoob?",
    a: "Acesse <a href='https://www.sicoob.com.br' target='_blank'>www.sicoob.com.br</a> → clique em <strong>'Internet Banking'</strong>. Faça login com CPF e senha. O primeiro acesso requer o uso do app para liberar o dispositivo como confiável."
  },

  // ── COOPERATIVA ────────────────────────────────────────
  {
    cat: "cooperativa", icon: "ti-users",
    q: "O que é uma cooperativa de crédito e como funciona?",
    a: "Uma cooperativa de crédito é uma <strong>instituição financeira de propriedade coletiva</strong> — os clientes são também donos (cooperados). Isso garante taxas menores, serviços personalizados e participação nos resultados anuais (sobras). O Sicoob é o maior sistema cooperativista do Brasil."
  },
  {
    cat: "cooperativa", icon: "ti-coin",
    q: "O que são sobras e como recebo?",
    a: "As <strong>sobras</strong> são o resultado positivo da cooperativa, distribuído proporcionalmente ao uso de produtos e serviços pelos cooperados. São calculadas anualmente e creditadas em conta após aprovação em Assembleia Geral. Consulte sua posição em <strong>Cooperativa → Minha participação</strong> no app."
  },
  {
    cat: "cooperativa", icon: "ti-users",
    q: "Como participo das decisões da cooperativa?",
    a: "Todo cooperado tem direito a voto nas <strong>Assembleias Gerais</strong> (Ordinária e Extraordinária). Você recebe convocação por e-mail e SMS. A participação pode ser presencial, por procuração ou, em algumas cooperativas, por voto digital. Acompanhe o calendário no app."
  },
  {
    cat: "cooperativa", icon: "ti-user-plus",
    q: "Quem pode ser cooperado?",
    a: "Qualquer pessoa física maior de 16 anos ou pessoa jurídica que atenda ao vínculo associativo da cooperativa (ex.: área geográfica, categoria profissional ou empresarial). O processo começa pela abertura de conta e integralização de quotas-partes — sem burocracia."
  },

  // ── SEGURANÇA ─────────────────────────────────────────
  {
    cat: "seguranca", icon: "ti-shield-lock",
    q: "Como evitar golpes e fraudes no Sicoob?",
    a: "O Sicoob <strong>nunca entra em contato solicitando senha, token ou transferência para conta segura</strong>. Desconfie de mensagens por WhatsApp, SMS ou ligações nesse sentido. Ative notificações de transação no app e mantenha o antivírus atualizado. Em caso de suspeita, ligue <strong>0800 642 2001</strong>.",
    tag: "Importante"
  },
  {
    cat: "seguranca", icon: "ti-lock",
    q: "O que é o token e como funciona no Sicoob?",
    a: "O <strong>token Sicoob</strong> é um código de segurança gerado pelo app para confirmar operações de alto valor ou em novos dispositivos. Ative em <strong>Configurações → Segurança → Token digital</strong>. Nunca compartilhe o token com ninguém — ele é de uso exclusivo seu."
  },
  {
    cat: "seguranca", icon: "ti-alert-triangle",
    q: "Fui vítima de fraude. O que fazer?",
    a: "Aja imediatamente: (1) <strong>Bloqueie sua conta</strong> no app ou ligue 0800 642 2001; (2) Registre um Boletim de Ocorrência (BO) online ou presencialmente; (3) Notifique a cooperativa sobre o ocorrido; (4) Guarde comprovantes e prints. Quanto mais rápido agir, maior a chance de reversão.",
    tag: "Urgente"
  },
  {
    cat: "seguranca", icon: "ti-device-laptop",
    q: "Como denunciar um site ou perfil falso do Sicoob?",
    a: "Acesse o canal oficial em <a href='https://www.sicoob.com.br' target='_blank'>sicoob.com.br</a> e use o formulário <strong>'Fale Conosco → Denúncias'</strong>. Você também pode reportar diretamente ao Banco Central pelo site bcb.gov.br. Sempre verifique se está acessando o domínio oficial '.sicoob.com.br'."
  }

];

/* ── Mapa de rótulos das seções ── */
const SECOES = {
  conta:       "Conta Digital",
  pix:         "Pix",
  cartao:      "Cartão",
  credito:     "Crédito",
  app:         "App & Internet Banking",
  cooperativa: "Cooperativa",
  seguranca:   "Segurança"
};

const ORDEM_CATS = ["conta","pix","cartao","credito","app","cooperativa","seguranca"];

let catAtual   = "all";
let termoBusca = "";

/* ── Renderização ── */
function render() {
  const lista    = document.getElementById("faqList");
  const semRes   = document.getElementById("noResults");
  const contagem = document.getElementById("resultCount");
  lista.innerHTML = "";

  const filtrados = FAQS.filter(f => {
    const matchCat = catAtual === "all" || f.cat === catAtual;
    const matchBusca = !termoBusca ||
      f.q.toLowerCase().includes(termoBusca) ||
      f.a.toLowerCase().includes(termoBusca);
    return matchCat && matchBusca;
  });

  if (!filtrados.length) {
    semRes.style.display = "block";
    contagem.textContent = "";
    return;
  }
  semRes.style.display = "none";
  contagem.textContent = termoBusca
    ? `${filtrados.length} resultado${filtrados.length !== 1 ? "s" : ""} encontrado${filtrados.length !== 1 ? "s" : ""}`
    : "";

  /* Agrupa por categoria */
  const grupos = {};
  filtrados.forEach(f => { (grupos[f.cat] = grupos[f.cat] || []).push(f); });

  ORDEM_CATS.forEach(cat => {
    if (!grupos[cat]) return;
    const secao = document.createElement("div");
    secao.className = "faq-section";

    /* Título de seção somente quando mostrando múltiplas categorias */
    if (catAtual === "all" || termoBusca) {
      secao.innerHTML = `<div class="section-label"><i class="ti ti-chevron-right" aria-hidden="true"></i>${SECOES[cat]}</div>`;
    }

    grupos[cat].forEach(f => {
      const item = document.createElement("div");
      item.className = "faq-item";
      item.setAttribute("role","group");
      item.innerHTML = `
        <div class="faq-q" role="button" tabindex="0"
             aria-expanded="false"
             aria-controls="ans-${f.q.slice(0,20).replace(/\s/g,'-')}">
          <div class="faq-q-icon" aria-hidden="true">
            <i class="ti ${f.icon}"></i>
          </div>
          <span class="faq-q-text">${f.q}${f.tag ? `<span class="badge">${f.tag}</span>` : ""}</span>
          <i class="ti ti-chevron-down faq-chevron" aria-hidden="true"></i>
        </div>
        <div class="faq-a" role="region"
             id="ans-${f.q.slice(0,20).replace(/\s/g,'-')}">
          <div class="faq-a-inner">${f.a}</div>
        </div>`;

      const btn = item.querySelector(".faq-q");
      btn.addEventListener("click", () => toggleItem(item, btn));
      btn.addEventListener("keydown", e => {
        if (e.key === "Enter" || e.key === " ") { e.preventDefault(); toggleItem(item, btn); }
      });
      secao.appendChild(item);
    });
    lista.appendChild(secao);
  });
}

function toggleItem(item, btn) {
  const abrindo = !item.classList.contains("open");
  /* Fecha todos */
  document.querySelectorAll(".faq-item.open").forEach(el => {
    el.classList.remove("open");
    el.querySelector(".faq-q").setAttribute("aria-expanded","false");
  });
  if (abrindo) {
    item.classList.add("open");
    btn.setAttribute("aria-expanded","true");
  }
}

/* ── Eventos ── */
document.getElementById("catBar").addEventListener("click", e => {
  const btn = e.target.closest(".cat-btn");
  if (!btn) return;
  document.querySelectorAll(".cat-btn").forEach(b => { b.classList.remove("active"); b.setAttribute("aria-pressed","false"); });
  btn.classList.add("active");
  btn.setAttribute("aria-pressed","true");
  catAtual = btn.dataset.cat;
  render();
});

document.getElementById("searchInput").addEventListener("input", e => {
  termoBusca = e.target.value.toLowerCase().trim();
  catAtual   = "all";
  document.querySelectorAll(".cat-btn").forEach(b => { b.classList.remove("active"); b.setAttribute("aria-pressed","false"); });
  document.querySelector("[data-cat='all']").classList.add("active");
  document.querySelector("[data-cat='all']").setAttribute("aria-pressed","true");
  render();
});

/* ── Inicializa ── */
render();
</script>

</body>
</html>
