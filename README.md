# My Cash Controller

## 📌 Contexto
Uma aplicação inteligente e moderna para controle de receitas, despesas, orçamentos e metas de economia. O foco é ajudar o usuário a ter previsibilidade financeira, evitar o endividamento e alcançar seus objetivos.

## 🛠 Tecnologias e Arquitetura
* **Backend:** Python / Django utilizando o padrão CBV (Class Based Views).
* **API:** Django REST Framework com proteção via Token Authentication.
* **Segurança & Privacidade:** Isolamento restrito de dados por usuário (via `LoginRequiredMixin` e filtros em `get_queryset()`).
* **Frontend:** Interface rica, responsiva e moderna, pensada para usabilidade em Desktop e Mobile (Mobile-first).

---

## 1. 🎯 Requisitos do Sistema

### Requisitos Funcionais (RF)
*   **RF01 - Autenticação:** Cadastro, login, logout e recuperação de senha.
*   **RF02 - Isolamento de Dados:** Garantia de que cada usuário acesse exclusivamente seus dados.
*   **RF03 - Transações Inteligentes:** Registro de Entradas e Saídas, associadas a categorias, datas e status (Pago/Pendente). Opção de marcar transações como recorrentes (ex: assinaturas).
*   **RF04 - Gestão de Categorias e Orçamentos:** O usuário deve criar categorias e poder definir **Limites Mensais (Orçamentos)** para elas (ex: Máximo de R$ 500 em Lazer).
*   **RF05 - Metas de Economia:** Criação de objetivos financeiros (ex: "Viagem de Férias", "Reserva de Emergência") com meta de valor e acompanhamento de progresso.
*   **RF06 - Motor de Alertas:** O sistema deve alertar (visualmente) se uma transação exceder o saldo atual ou estourar o orçamento definido para a categoria.
*   **RF07 - Dashboard e Analytics:** Gráficos de evolução mensal do patrimônio (linha), distribuição de gastos (pizza/doughnut) e barras de progresso para Orçamentos e Metas.
*   **RF08 - Relatórios e Exportação:** Geração de extrato e relatórios analíticos em PDF e CSV.
*   **RF09 - API REST:** Disponibilização dos dados do usuário para integrações de terceiros via endpoints protegidos.

### Requisitos Não Funcionais (RNF)
*   **RNF01 - Padrão de Código:** O backend deve seguir rigorosamente os princípios DRY e usar CBVs do Django.
*   **RNF02 - UI/UX:** A interface deve seguir princípios de design moderno (Clean Design, Micro-interações, Dark/Light mode).
*   **RNF03 - Segurança:** Rotas da API devem validar o Token JWT ou Token simples no cabeçalho HTTP.

---

## 2. 👤 Estórias de Usuário (User Stories)

*   **US01 - Transações:** *Como* usuário, *quero* registrar minhas despesas e receitas facilmente, *para que* eu saiba exatamente para onde meu dinheiro está indo.
*   **US02 - Orçamentos Mensais:** *Como* usuário, *quero* definir um limite de gastos para "Alimentação", *para que* eu seja avisado quando estiver perto de gastar demais.
*   **US03 - Metas Financeiras:** *Como* usuário, *quero* cadastrar uma meta para comprar um carro, *para que* eu possa acompanhar visualmente o quanto falta para alcançá-la.
*   **US04 - Alertas Proativos:** *Como* usuário, *quero* receber um aviso de "Saldo Insuficiente" ao lançar uma despesa alta, *para que* eu evite o cheque especial.
*   **US05 - Dashboard Visual:** *Como* usuário, *quero* ver gráficos que resumam meu mês (receitas vs despesas), *para que* a análise da minha saúde financeira seja rápida.
*   **US06 - Exportação de Dados:** *Como* usuário, *quero* baixar meu extrato em PDF/CSV, *para que* eu possa mandar para o contador.
*   **US07 - Acesso via API:** *Como* desenvolvedor/integrador, *quero* consumir as rotas via API com Token, *para que* eu possa conectar minha conta bancária futuramente.

---

## 3. 🧪 Cenários BDD / Gherkin

### Funcionalidade: Controle de Orçamento e Alertas (US02, US04)

```gherkin
Funcionalidade: Controle de Orçamento Mensal
  Sendo um usuário com limites financeiros
  Quero que o sistema monitore meus gastos por categoria
  Para me alertar quando eu estiver passando do limite

  Contexto:
    Dado que eu tenho um Orçamento de R$ 1000,00 para a categoria "Lazer"
    E já gastei R$ 800,00 em "Lazer" este mês

  Cenário: Lançamento que excede o orçamento
    Quando eu tento registrar uma nova despesa de R$ 300,00 em "Lazer"
    Então o sistema deve salvar a transação
    Mas deve exibir um alerta visual: "Você excedeu seu orçamento de Lazer neste mês!"
    E a barra de progresso do orçamento de Lazer deve ficar vermelha (110%)
```

### Funcionalidade: Progresso de Metas Financeiras (US03)

```gherkin
Funcionalidade: Acompanhamento de Metas de Economia
  Sendo um usuário focado em poupar
  Quero alocar dinheiro para minhas metas
  Para ver meu progresso em direção aos meus sonhos

  Contexto:
    Dado que eu tenho uma meta "Viagem" com valor alvo de R$ 5000,00
    E o saldo guardado atualmente na meta é de R$ 1000,00

  Cenário: Depositando dinheiro na meta
    Quando eu registro um aporte de R$ 500,00 para a meta "Viagem"
    Então o saldo da meta deve ser atualizado para R$ 1500,00
    E o sistema deve mostrar que atingi "30%" da minha meta
```

### Funcionalidade: Dashboard e Analytics (US05)

```gherkin
Funcionalidade: Análise Visual Financeira
  Sendo um usuário logado
  Quero visualizar meus dados no Dashboard
  Para ter um resumo rápido

  Cenário: Visualização dos gráficos principais
    Dado que possuo transações registradas no mês vigente
    Quando eu acesso a página principal do Dashboard
    Então devo ver o card de "Saldo Atual"
    E um "Gráfico de Pizza" das despesas agrupadas
    E barras de progresso dos meus "Orçamentos" ativos
```q
Atue como um Engenheiro de UI/UX Sênior. Preciso de uma interface Single Page Application (SPA) para um SaaS de Controle Financeiro Pessoal chamado "My Cash Controller". O design deve ser extremamente premium, limpo e inspirar confiança (estilo Fintechs, com referências a TailwindUI ou Vercel Design).

A aplicação deve ter um layout com Sidebar lateral para navegação e um conteúdo central fluído. Construa as seguintes sessões visuais:

1. SIDEBAR DE NAVEGAÇÃO:
- Logo do "My Cash Controller".
- Links com ícones: Dashboard, Transações, Orçamentos, Metas, Relatórios.
- Seção inferior com o Perfil do Usuário e botão "Sair".

2. TELA DE DASHBOARD (A visão principal):
- Cabeçalho: Saudação ao usuário e um botão primário flutuante/destacado de "+ Nova Transação".
- Área de "Resumo Rápido" (3 Cards): 
  1. Saldo em Conta (Em destaque).
  2. Receitas do Mês (Verde).
  3. Despesas do Mês (Vermelho).
- Área Analítica:
  - Um Gráfico de Linhas (Line Chart) grande mostrando a "Evolução do Patrimônio" nos últimos meses.
  - Um Gráfico de Pizza/Doughnut (Doughnut Chart) mostrando "Despesas por Categoria".
- Área de "Saúde Financeira" (Cards menores ou lista):
  - "Orçamentos": Uma lista com barras de progresso (Progress Bars). Exemplo: Categoria "Lazer" (R$ 800 / R$ 1.000) com a barra em amarelo. Se estourado, barra vermelha.
  - "Metas": Um card de progresso circular mostrando a meta "Viagem de Férias" em 30% concluída.

3. MODAL DE TRANSAÇÃO (O que abre ao clicar em + Nova Transação):
- Um Modal com background blurred (Glassmorphism).
- Tabs no topo do modal para selecionar: "Entrada" ou "Saída".
- Inputs modernos com labels flutuantes para: Valor (em R$), Categoria (Select/Dropdown), Data (Datepicker) e Checkbox para "Transação Recorrente".
- Botão de submit.

4. DIRETRIZES DE ESTÉTICA E DESIGN:
- Utilize cores harmoniosas: fundo cinza/branco muito leve (off-white), textos em cinza escuro/chumbo para contraste elegante. Para valores, use verde esmeralda (sucesso/entradas) e vermelho suave (alertas/saídas).
- Use uma tipografia sem serifa moderna (ex: Inter, Outfit ou Roboto).
- Inclua micro-animações (hover nos botões, carregamento sutil dos gráficos).
- Opcional: Implemente um "Dark Mode" luxuoso com fundos escuros (slate-900) e bordas sutis.

Crie os componentes visuais focando em uma excelente experiência de usuário e layout responsivo. A interface não deve parecer genérica, e sim um aplicativo premium de finanças.
