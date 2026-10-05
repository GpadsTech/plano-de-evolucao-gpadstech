🌦️ Morhinga — Plano de Evolução do Dashboard

Plano técnico e operacional para evolução do Dashboard Morhinga, contemplando a implementação de Chatbot, Geração de Relatórios e Exportação do Histórico em CSV.

---

📅 Cronograma Geral

Sprint| Período| Objetivo
🧱 Sprint 1| 05/10/2026 → 11/10/2026| Estruturação das funcionalidades
🔌 Sprint 2| 12/10/2026 → 18/10/2026| Implementação e integração
🚀 Sprint 3| 19/10/2026 → 25/10/2026| Testes e estabilização
🏁 Entrega Final| 26/10/2026| Finalização do projeto

---

🎯 1. Objetivo

O objetivo deste ciclo de desenvolvimento é evoluir o Dashboard Morhinga com três novas funcionalidades principais:

🤖 1. Chatbot

Adicionar um chatbot capaz de responder perguntas relacionadas aos dados de uma estação ou boia selecionada pelo usuário.

Exemplos:

- "Qual foi a temperatura média dessa estação?"
- "Qual foi a maior umidade registrada?"
- "Qual foi a última medição?"
- "Quantos registros existem?"
- "Qual foi a menor temperatura registrada?"

O chatbot deverá utilizar somente os dados do equipamento atualmente selecionado.

---

📄 2. Gerar Relatório

No Dashboard, ao lado do botão Histórico, deverá ser adicionado o botão:

«Gerar Relatório»

O botão deverá gerar um relatório referente aos dados que estão sendo visualizados/analisados no Dashboard.

O relatório poderá conter:

- Identificação da estação/boia;
- Tipo do equipamento;
- Período analisado;
- Dados coletados;
- Valores mínimos;
- Valores máximos;
- Médias;
- Quantidade de registros;
- Data/hora de geração;
- Tabelas e/ou gráficos, quando aplicável.

---

📊 3. Gerar CSV

Na página Histórico, deverá ser adicionado ao lado do botão Pesquisar o botão:

«Gerar CSV»

O fluxo será:

Usuário seleciona equipamento
        ↓
Define período/filtros
        ↓
Clica em "Pesquisar"
        ↓
Histórico é carregado
        ↓
Clica em "Gerar CSV"
        ↓
CSV é gerado
        ↓
Download do arquivo

O CSV deverá representar os dados retornados pela pesquisa realizada, respeitando os filtros selecionados.

---

🏗️ 2. Arquitetura Atual

O Morhinga utiliza uma arquitetura baseada em:

- Frontend: React + Vite + JavaScript
- Backend: Django
- Banco/serviços: Firebase / Firestore
- Visualização: Recharts
- Roteamento: React Router
- Estilização: CSS Modules

Repositórios:

- "Frontend — gpads-tech-front-end" (https://github.com/GpadsTech/gpads-tech-front-end)
- "Backend — backend-gpadstech-django" (https://github.com/GpadsTech/backend-gpadstech-django)

---

🔄 3. Comunicação entre as camadas

A arquitetura deve seguir o fluxo:

┌───────────────────────────┐
│         FRONTEND          │
│      React + Vite         │
└─────────────┬─────────────┘
              │
              │ HTTP / API
              ▼
┌───────────────────────────┐
│          BACKEND          │
│          Django           │
└─────────────┬─────────────┘
              │
              │ Consulta / Processamento
              ▼
┌───────────────────────────┐
│     FIREBASE / FIRESTORE  │
└───────────────────────────┘

O frontend deve ser responsável principalmente pela interface e experiência do usuário.

O backend deve concentrar:

- Regras de negócio;
- Processamento;
- Consultas;
- Geração de arquivos;
- Processamento do chatbot;
- Validações;
- Comunicação com serviços externos.

---

⚠️ 4. Regra de Arquitetura

Evitar colocar toda a lógica diretamente nos componentes React.

❌ Evitar

Botão
  ↓
React consulta todos os dados
  ↓
React calcula estatísticas
  ↓
React monta relatório
  ↓
React gera arquivo

✅ Preferir

React
  ↓
Solicitação
  ↓
Django
  ↓
Processamento
  ↓
Firebase
  ↓
Resultado
  ↓
React

Essa separação será especialmente importante para:

- Chatbot;
- Relatórios;
- CSV;
- Processamento de históricos;
- Futuras funcionalidades analíticas.

---

👥 5. Responsabilidades

👨‍💻 Jorge — Backend + Integração

Responsável principal por:

- Backend Django;
- APIs;
- Regras de negócio;
- Consultas ao Firebase;
- Processamento dos dados;
- Chatbot;
- Geração do relatório;
- Geração do CSV;
- Tratamento de erros;
- Documentação dos endpoints;
- Integração Backend ↔ Frontend.

Fluxo principal:

Firebase
   ↓
Django
   ↓
APIs
   ├── Chatbot
   ├── Relatório
   └── CSV
   ↓
React

---

👨‍💻 Ryan — Frontend + Componentes

Responsável principalmente por:

- Integração dos componentes com a API;
- Componente do chatbot;
- Botão "Gerar Relatório";
- Botão "Gerar CSV";
- Estados de carregamento;
- Mensagens de erro;
- Feedback visual;
- Organização dos componentes;
- Testes de integração.

---

👨‍💻 Thiago — Frontend + UI/UX

Responsável principalmente por:

- Layout dos novos componentes;
- Estilização;
- Responsividade;
- Componentes reutilizáveis;
- Experiência do usuário;
- Estados visuais;
- Interface do chatbot;
- Interface de geração de arquivos;
- Ajustes visuais na página de Histórico.

---

🧱 6. Sprint 1 — Estruturação

📅 Período

05/10/2026 → 11/10/2026

🎯 Objetivo

Criar a estrutura técnica das três novas funcionalidades e definir como frontend e backend irão se comunicar.

---

👨‍💻 Jorge — Backend

05/10 — Mapeamento

Estudar:

- Estrutura atual do Django;
- Views;
- Services;
- Autenticação;
- Firebase;
- Equipamentos;
- Estações;
- Boias;
- Histórico;
- Estrutura atual dos dados.

---

06/10 — Definição das APIs

Definir os endpoints necessários.

Exemplo inicial:

GET  /api/equipamentos/{id}/dados
GET  /api/equipamentos/{id}/historico
POST /api/chatbot
POST /api/relatorio
POST /api/historico/csv

Os nomes deverão ser adaptados à arquitetura existente.

---

07/10 — Consulta de dados

Implementar/ajustar a consulta de dados de um equipamento.

Exemplo:

{
  "equipamento_id": "EST001",
  "tipo": "estacao"
}

O backend deverá identificar corretamente o equipamento.

---

08/10 — Estrutura do CSV

Definir:

- Campos;
- Datas;
- Formato;
- Separador;
- Codificação;
- Nome do arquivo.

Exemplo:

historico_EST001_2026-10-05.csv

---

09/10 — Estrutura do relatório

Definir as informações que deverão aparecer no relatório.

---

10/10 — Estrutura do chatbot

Definir o fluxo:

Pergunta
   +
Equipamento
   +
Dados
   ↓
Processamento
   ↓
Resposta

---

11/10 — Testes

Realizar testes iniciais e documentar a estrutura.

✅ Entrega de Jorge

Ao final da Sprint:

- [ ] APIs definidas;
- [ ] Consulta de dados funcionando;
- [ ] Estrutura do CSV criada;
- [ ] Estrutura do relatório definida;
- [ ] Arquitetura do chatbot definida.

---

👨‍💻 Ryan — Frontend

05–06/10

Mapear a estrutura atual do Dashboard.

Identificar:

Dashboard
├── Informações do equipamento
├── Gráficos
├── Filtros
├── Histórico
└── Área dos novos recursos

07/10

Criar o componente inicial do chatbot.

Inicialmente, pode utilizar respostas mockadas.

08/10

Adicionar:

Histórico | Gerar Relatório

09/10

Criar fluxo visual do relatório.

10/10

Criar estados:

Gerando relatório...
Relatório gerado!
Erro ao gerar relatório.

11/10

Testes e ajustes.

---

👨‍💻 Thiago — Frontend/UI

05–06/10

Estudar os componentes existentes e identificar elementos reutilizáveis.

07/10

Criar o layout do chatbot.

Estrutura sugerida:

┌───────────────────────────────┐
│ 🤖 Assistente Morhinga        │
├───────────────────────────────┤
│                               │
│ Pergunte sobre este           │
│ equipamento...                │
│                               │
├───────────────────────────────┤
│ Digite sua pergunta...    ➤   │
└───────────────────────────────┘

08/10

Criar componente visual do botão de relatório.

09/10

Criar componente visual do botão CSV.

10/10

Responsividade.

11/10

Revisão com Ryan e Jorge.

---

✅ Resultado da Sprint 1

Ao final:

                 SPRINT 1
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
     Backend      Frontend       UI
        │            │            │
       APIs       Componentes    Layout
        │            │            │
        └────────────┼────────────┘
                     ▼
              Estrutura pronta

O chatbot não precisa estar inteligente ainda.

A prioridade é deixar a arquitetura pronta para integração.

---

🔌 7. Sprint 2 — Implementação e Integração

📅 Período

12/10/2026 → 18/10/2026

🎯 Objetivo

Conectar frontend e backend e fazer as três funcionalidades trabalharem com dados reais.

---

👨‍💻 Jorge — Backend

12–13/10

Implementar definitivamente a geração do CSV.

Receber:

Equipamento
Período
Filtros

e devolver o arquivo correspondente.

---

14/10

Implementar a geração do relatório.

Exemplo:

{
  "equipamento_id": "EST001",
  "data_inicio": "2026-10-01",
  "data_fim": "2026-10-05"
}

---

15–16/10

Implementar a primeira versão funcional do chatbot.

Fluxo:

Pergunta
   ↓
Backend
   ↓
Identifica equipamento
   ↓
Consulta dados
   ↓
Prepara contexto
   ↓
Processa pergunta
   ↓
Resposta
   ↓
Frontend

---

17/10

Implementar tratamento de erros.

Exemplos:

Equipamento não encontrado.

Não existem dados para este período.

Não foi possível gerar o relatório.

Não foi possível processar a pergunta.

---

18/10

Testes completos do backend.

---

👨‍💻 Ryan — Frontend

12/10

Conectar:

Gerar Relatório
       ↓
API

13/10

Implementar recebimento/download do relatório.

14/10

Conectar:

Gerar CSV
       ↓
API

15/10

Conectar o chatbot:

Input
 ↓
API
 ↓
Resposta
 ↓
Chat

16/10

Implementar loading:

🤖 Analisando os dados...

17/10

Tratamento de erros.

18/10

Testes.

---

👨‍💻 Thiago — Frontend/UI

12–13/10

Aprimorar o chatbot:

- Mensagens;
- Balões;
- Histórico da conversa;
- Loading;
- Botão enviar;
- Responsividade.

14/10

Aprimorar interface do relatório.

15/10

Aprimorar interface do CSV.

Fluxo:

Pesquisar
   ↓
Dados encontrados
   ↓
Gerar CSV

16–17/10

Testes de responsividade.

18/10

Revisão visual.

---

✅ Resultado da Sprint 2

Chatbot

Pergunta real
     ↓
Backend
     ↓
Dados reais
     ↓
Resposta

Relatório

Dashboard
     ↓
Gerar Relatório
     ↓
Arquivo

CSV

Pesquisar
     ↓
Histórico
     ↓
Gerar CSV
     ↓
Download

---

🚀 8. Sprint 3 — Testes e Estabilização

📅 Período

19/10/2026 → 25/10/2026

🎯 Objetivo

Finalizar as funcionalidades, corrigir bugs e preparar a entrega.

A Sprint 3 não deverá ser utilizada para iniciar novas funcionalidades grandes.

---

👨‍💻 Jorge — Backend

19/10

Testar todas as APIs.

20/10

Testar diferentes equipamentos:

EST001
EST002
EST003
BOIA001
BOIA002

Verificar se não há mistura de dados.

21/10

Testar períodos:

1 dia
7 dias
30 dias
Sem dados

22/10

Validar CSV:

- [ ] Colunas;
- [ ] Datas;
- [ ] Valores;
- [ ] Equipamento;
- [ ] Quantidade de registros.

23/10

Validar relatório.

24/10

Correção de bugs.

25/10

Documentação final do backend.

---

👨‍💻 Ryan — Frontend

19/10

Teste da integração completa.

20/10

Testar chatbot.

Perguntas:

Qual foi a última temperatura?

Qual foi a maior temperatura?

Qual a média de umidade?

Quantos registros existem?

21/10

Testar perguntas fora do contexto.

Exemplos:

Quem descobriu o Brasil?

Qual a previsão do tempo?

Me conte uma piada.

O chatbot deverá responder de maneira controlada.

22/10

Testar relatório.

23/10

Testar CSV.

24/10

Correções.

25/10

Revisão final.

---

👨‍💻 Thiago — Frontend/UI

19/10

Testar responsividade:

- Desktop;
- Notebook;
- Tablet;
- Celular.

20/10

Revisar chatbot.

21/10

Revisar botões.

22/10

Revisar página Histórico.

23/10

Padronização visual.

24/10

Correções.

25/10

Revisão final com Ryan e Jorge.

---

🏁 9. Entrega Final — 26/10/2026

O dia 26/10/2026 será destinado ao fechamento da entrega.

Checklist

Código

- [ ] Código integrado;
- [ ] Branches atualizadas;
- [ ] Pull Requests revisados;
- [ ] Code Review realizado;
- [ ] Merge realizado.

Backend

- [ ] Chatbot funcionando;
- [ ] Relatório funcionando;
- [ ] CSV funcionando;
- [ ] APIs documentadas;
- [ ] Tratamento de erros.

Frontend

- [ ] Chatbot funcionando;
- [ ] Botão "Gerar Relatório";
- [ ] Botão "Gerar CSV";
- [ ] Loading;
- [ ] Mensagens de erro;
- [ ] Responsividade;
- [ ] Interface revisada.

---

🤖 10. Especificação do Chatbot

Regra principal

«O chatbot responde sobre o equipamento que está sendo visualizado pelo usuário.»

Se o usuário estiver visualizando:

Estação EST001

e perguntar:

«Qual foi a temperatura média?»

A consulta deverá considerar EST001.

Não deverá misturar informações de:

EST002
BOIA001
BOIA002

---

Exemplo de requisição

{
  "equipamento_id": "EST001",
  "tipo": "estacao",
  "pergunta": "Qual foi a temperatura média?"
}

---

Fluxo

Receber pergunta
       ↓
Validar equipamento
       ↓
Consultar dados
       ↓
Preparar contexto
       ↓
Processar pergunta
       ↓
Gerar resposta

---

Exemplos de perguntas

Temperatura

«Qual foi a maior temperatura registrada?»

Umidade

«Qual foi a umidade média?»

Período

«Qual foi a última medição?»

Comparação

«A temperatura aumentou durante o período?»

Quantidade

«Quantos registros existem?»

Estatística

«Qual foi a menor temperatura registrada?»

---

🚫 11. Limitações do Chatbot

O chatbot não deverá funcionar como um assistente geral.

Perguntas fora do contexto deverão receber uma resposta controlada.

Exemplo:

«"Posso responder apenas perguntas relacionadas aos dados deste equipamento."»

Isso reduz respostas inventadas e mantém o chatbot focado no monitoramento.

---

📄 12. Especificação do Relatório

O relatório deverá representar os dados analisados pelo usuário.

Estrutura sugerida:

MORHINGA
Relatório de Monitoramento

Equipamento: EST001
Tipo: Estação
Período: 01/10/2026 – 05/10/2026

--------------------------------

Temperatura
Mínima:
Máxima:
Média:

Umidade
Mínima:
Máxima:
Média:

--------------------------------

Quantidade de registros:
Data de geração:

A estrutura deverá se adaptar às variáveis realmente existentes no equipamento.

---

📊 13. Especificação do CSV

O CSV deverá ser detalhado e representar os dados retornados pela pesquisa.

Exemplo:

equipamento,data_hora,temperatura,umidade,pressao,gas,luz,rpm,voltagem
EST001,05/10/2026 10:00,27.4,72,1012,12,450,120,5.1
EST001,05/10/2026 10:05,27.6,71,1012,13,460,121,5.1
EST001,05/10/2026 10:10,27.8,70,1011,12,455,119,5.0

Os campos deverão ser ajustados conforme os dados disponíveis.

---

🔐 14. Segurança e Integridade dos Dados

A equipe deverá garantir que o sistema não:

- Misture dados de equipamentos;
- Ignore filtros de data;
- Retorne dados de outro usuário;
- Exponha informações indevidas;
- Confie exclusivamente no ID enviado pelo frontend;
- Exponha credenciais do Firebase;
- Duplique regras de negócio entre frontend e backend.

---

🌿 15. Estratégia de Branches

Sugestão:

main
│
└── develop
    │
    ├── feature/chatbot
    ├── feature/relatorio
    ├── feature/csv
    ├── feature/frontend-chatbot
    ├── feature/frontend-relatorio
    └── feature/frontend-csv

Regra

Não desenvolver diretamente na "main".

Fluxo:

Criar branch
     ↓
Desenvolver
     ↓
Testar
     ↓
Commit
     ↓
Push
     ↓
Pull Request
     ↓
Code Review
     ↓
Merge

---

✅ 16. Definition of Done

Uma tarefa só será considerada concluída quando:

- [ ] Código implementado;
- [ ] Código testado;
- [ ] Integração realizada;
- [ ] Tratamento de erros implementado;
- [ ] Interface funcionando;
- [ ] Responsividade verificada;
- [ ] Dados reais utilizados;
- [ ] Pull Request criado;
- [ ] Code Review realizado;
- [ ] Sem erros no console;
- [ ] Sem erros no backend;
- [ ] Funcionalidade documentada.

---

📅 17. Resumo das Sprints

Sprint| Período| Foco
🧱 Sprint 1| 05–11/10| Estruturação
🔌 Sprint 2| 12–18/10| Implementação + Integração
🚀 Sprint 3| 19–25/10| Testes + Estabilização
🏁 Entrega| 26/10| Finalização

---

👥 18. Resumo da Equipe

Integrante| Responsabilidade principal
Jorge| Backend + APIs + Integração
Ryan| Frontend + Componentes + Integração
Thiago| Frontend + UI/UX + Responsividade

---

🎯 19. Resultado Final Esperado

Ao final do ciclo de desenvolvimento, o Dashboard Morhinga deverá permitir:

                         MORHINGA
                            │
                 ┌──────────┴──────────┐
                 │                     │
            EQUIPAMENTO             HISTÓRICO
                 │                     │
        ┌────────┼────────┐            │
        │        │        │            │
      Dados   Chatbot  Relatório    Pesquisar
        │        │        │            │
        │        │        └──────┐     │
        │        │               │     │
        │        │          Download   │
        │        │                     │
        │        └── Perguntas         │
        │            sobre dados       │
        │                              │
        └──────────────────────────────┤
                                       │
                                  Gerar CSV
                                       │
                                    Download

🚀 Meta da entrega

O objetivo não é simplesmente adicionar três novos botões.

O objetivo é entregar três funcionalidades completas, integradas ao backend e aos dados reais do Morhinga:

🤖 Chatbot
    ↓
Análise dos dados do equipamento

📄 Relatório
    ↓
Documento com os dados analisados

📊 CSV
    ↓
Exportação detalhada do histórico pesquisado

---

🔗 Repositórios

Frontend

"GpadsTech/gpads-tech-front-end" (https://github.com/GpadsTech/gpads-tech-front-end)

Backend

"GpadsTech/backend-gpadstech-django" (https://github.com/GpadsTech/backend-gpadstech-django)

---

Morhinga — GPADS Tech Solutions GovTech

Plano de desenvolvimento — Outubro/2026
