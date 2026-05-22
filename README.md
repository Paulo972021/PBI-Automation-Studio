<div align="center">

# PBI Automation Studio

### Automação e diagnóstico de interações em relatórios Power BI com Python e Playwright

<br>

<img src="https://img.shields.io/badge/Python-Automation-3776AB?style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/Playwright-Browser%20Automation-2EAD33?style=for-the-badge&logo=playwright&logoColor=white" />
<img src="https://img.shields.io/badge/Power_BI-Report%20Automation-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" />
<img src="https://img.shields.io/badge/Desktop-GUI-1F6FEB?style=for-the-badge" />

<br><br>

**Python • RPA • Playwright • Dashboards • Templates • Diagnóstico • Exportação Estruturada**

<br>

> Case técnico apresentado de forma anonimizada.  
> A versão pública utiliza somente dados fictícios e não acessa relatórios corporativos reais.

</div>

---

## Visão geral

O **PBI Automation Studio** é uma aplicação desktop desenvolvida em Python para automatizar interações recorrentes em relatórios Power BI acessados via navegador.

A solução foi estruturada para controlar um navegador real, navegar entre páginas e subpáginas do relatório, identificar filtros disponíveis, aplicar configurações salvas em templates JSON, localizar visuais potencialmente exportáveis e executar rotinas de exportação de dados para Excel.

Além da automação principal, o projeto incorpora recursos de diagnóstico e rastreabilidade, como:

- captura de screenshots por etapa;
- geração de PDF diagnóstico;
- medição de tempo por fase da execução;
- gravação e reprodução de interações;
- interface desktop para gerenciamento de templates;
- acompanhamento de logs durante o processamento.

O projeto foi criado para lidar com características comuns de aplicações analíticas web: carregamento assíncrono, menus exibidos apenas após interação, filtros personalizados, subpáginas acessadas por cards e necessidade de evidências para investigação de falhas.

---

## Problema

Relatórios analíticos em Power BI podem exigir sequências operacionais repetitivas, como:

- acessar páginas específicas do relatório;
- navegar entre abas, cards e subpáginas;
- identificar filtros disponíveis;
- aplicar combinações recorrentes de valores;
- localizar tabelas ou visuais exportáveis;
- abrir menus dinâmicos;
- iniciar exportações;
- repetir o processo para diferentes cenários analíticos.

Quando esse fluxo é executado manualmente, o processo se torna mais demorado, sujeito a inconsistências e difícil de diagnosticar em caso de falha.

Além disso, componentes dinâmicos da interface podem apresentar comportamentos variáveis, como:

- carregamento progressivo;
- elementos ocultos;
- filtros dentro de componentes personalizados;
- menus dependentes de hover;
- estados intermediários difíceis de reproduzir.

---

## Solução desenvolvida

Foi desenvolvida uma aplicação de automação baseada em **Python** e **Playwright assíncrono** para controlar o Microsoft Edge, acessar relatórios Power BI e executar cenários configuráveis de navegação, filtragem e exportação.

A solução inclui:

1. abertura automatizada do navegador;
2. acesso configurável ao relatório;
3. navegação por páginas e subpáginas;
4. descoberta de filtros e valores disponíveis;
5. aplicação de filtros a partir de templates;
6. identificação de visuais potencialmente exportáveis;
7. rotina implementada para exportação de dados para Excel;
8. registro de screenshots e PDF diagnóstico;
9. profiler para medição de desempenho;
10. interface desktop para gerenciamento de templates e acompanhamento da execução.

---

## Status do projeto

| Área | Status identificado |
|---|---|
| Navegação automatizada no relatório | Implementada em código e evidenciada por diagnóstico |
| Navegação por páginas e subpáginas | Implementada; artefato enviado registra falha em uma subpágina específica |
| Scan de filtros | Implementado em código |
| Aplicação de filtros por template | Implementada em código |
| Identificação de visuais exportáveis | Implementada em código |
| Rotina de exportação para Excel | Implementada em código, sem arquivo exportado comprovando sucesso no pacote analisado |
| Screenshots e PDF diagnóstico | Implementados e comprovados por artefato |
| Profiler de desempenho | Implementado e comprovado por relatórios |
| Interface desktop | Implementada em código |
| Templates para tabelas em slides | Parcialmente implementados na interface |
| Geração automática de PowerPoint | Não confirmada |
| Tratamento específico de alerta de confidencialidade | Não confirmado |
| Reutilização automática de sessão autenticada | Módulo existente, mas não integrado ao fluxo principal analisado |

---

## Funcionalidades implementadas

### Navegação automatizada

A aplicação abre uma sessão controlada do Microsoft Edge por meio do Playwright e acessa o relatório configurado para execução.

Recursos identificados:

- inicialização automatizada do navegador;
- suporte a downloads;
- detecção de necessidade de autenticação;
- navegação por páginas visíveis;
- navegação por subpáginas acessadas por cards;
- tratamento de abas ocultas em carousel;
- validação de mudança de página;
- espera por estabilidade dos visuais carregados.

---

### Descoberta de filtros

A solução possui módulos voltados a identificar filtros disponíveis no relatório e coletar seus possíveis valores.

Recursos identificados:

- detecção de slicers na página;
- leitura de valores disponíveis;
- coleta de opções em listas com scroll;
- suporte a componentes `ChicletSlicer`;
- leitura de componentes contidos em iframe;
- geração de templates reutilizáveis em JSON;
- mapeamento de dependências entre filtros.

---

### Aplicação parametrizada de filtros

A automação pode utilizar templates para aplicar cenários previamente configurados.

Exemplos de possibilidades suportadas:

- seleção de valores fixos;
- aplicação de combinações de filtros;
- seleção dinâmica do maior valor disponível;
- execução de diferentes páginas com parâmetros próprios;
- reutilização de configurações sem alteração direta do código.

Exemplo fictício de template:

```json
{
  "name": "Resumo Mensal - Demonstração",
  "page": "RESUMO",
  "filters": {
    "periodo": {
      "mode": "select_max"
    },
    "categoria": {
      "value": "CATEGORIA_A"
    },
    "status": {
      "value": "ATIVO"
    }
  }
}
```

---

### Identificação de visuais exportáveis

A aplicação analisa os elementos visuais da página para identificar quais apresentam opção válida de exportação.

O fluxo implementado considera:

- leitura dos containers existentes;
- exclusão de filtros e elementos de navegação;
- identificação de visuais candidatos;
- hover sobre o visual;
- localização do menu de opções;
- verificação da presença da ação **Exportar dados**;
- preparação da rotina de download.

```text
Visual detectado
        ↓
Classificação do container
        ↓
Hover e abertura do menu
        ↓
Opção “Exportar dados” encontrada?
        ↓
Sim: rotina de exportação é iniciada
Não: visual é ignorado ou registrado para diagnóstico
```

---

### Rotina de exportação para Excel

O projeto possui código implementado para:

- abrir o menu do visual;
- selecionar a opção de exportação;
- lidar com o diálogo correspondente;
- aguardar o download;
- salvar o arquivo resultante em formato `.xlsx`.

### Limite de comprovação

O material analisado não contém arquivos Excel exportados nem evidência visual de uma exportação concluída com sucesso.

Portanto, este case apresenta corretamente a funcionalidade como:

> Rotina implementada para exportação de dados de visuais Power BI para Excel.

Não como:

> Processo integral de exportação comprovadamente homologado em ambiente real.

---

## Fluxo principal da solução

```text
Configuração do cenário
        ↓
Abertura automatizada do navegador
        ↓
Acesso ao relatório Power BI
        ↓
Validação de carregamento inicial
        ↓
Navegação para página ou subpágina
        ↓
Scan ou aplicação de filtros configurados
        ↓
Identificação de visuais exportáveis
        ↓
Tentativa de exportação de dados
        ↓
Captura de evidências e tempos de execução
        ↓
Encerramento com logs e diagnóstico
```

---

## Fluxo orientado a templates

```text
Usuário escolhe um template na interface
        ↓
Configuração do cenário é carregada
        ↓
Página e filtros são preparados
        ↓
Automação executa o fluxo configurado
        ↓
Visuais são identificados
        ↓
Exportações são tentadas
        ↓
Logs e evidências são apresentados
```

O uso de templates permite separar a regra de execução dos parâmetros operacionais, tornando a automação mais reutilizável para diferentes cenários de análise.

---

## Arquitetura da solução

```text
┌─────────────────────────────────────────────┐
│ Interface Desktop                            │
│ Templates • Scan • Execução • Logs          │
│ app_gui.py + gui/APP.html                   │
└──────────────────────┬──────────────────────┘
                       │
                       │ configura e executa
                       ▼
┌─────────────────────────────────────────────┐
│ Orquestrador Principal                       │
│ main.py                                     │
│ Sessão • Modos • Encerramento • Diagnóstico │
└─────────────┬───────────────────────────────┘
              │
     ┌────────┼───────────┬──────────────┐
     │        │           │              │
     ▼        ▼           ▼              ▼
┌─────────┐ ┌─────────┐ ┌───────────┐ ┌─────────────┐
│Navigate │ │ Filters │ │ Templates │ │ Downloader  │
│navigator│ │scanner/ │ │ runner    │ │ Exportação  │
│         │ │slicer   │ │           │ │             │
└─────────┘ └─────────┘ └───────────┘ └─────────────┘
     │        │           │              │
     └────────┴───────────┴──────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────┐
│ Diagnóstico e Observabilidade                │
│ Screenshots • PDF • Profiler • Recorder     │
└─────────────────────────────────────────────┘
```

---

## Organização modular identificada

### `main.py` — Orquestração principal

Responsável por:

- carregar configurações;
- validar parâmetros;
- abrir o navegador;
- iniciar a navegação;
- escolher entre modo de scan e modo de execução;
- coordenar filtros, templates e exportações;
- gerar evidências ao final da execução.

---

### `navigator.py` — Navegação resiliente

Responsável por localizar páginas e subpáginas do relatório.

Estratégias implementadas:

- busca literal e normalizada por texto;
- navegação por cards internos;
- clique em abas visíveis;
- uso de `scrollIntoView`;
- scroll de containers;
- clique forçado;
- navegação em carousel;
- verificação da mudança de página;
- validação da estabilidade do conjunto de visuais.

---

### `filter_scanner.py` — Descoberta de filtros

Responsável por:

- detectar filtros existentes;
- coletar valores disponíveis;
- percorrer listas roláveis;
- identificar componentes personalizados;
- mapear dependências;
- gerar templates reutilizáveis.

---

### `slicer_manager.py` — Manipulação dos filtros

Responsável por:

- identificar tipos de slicer;
- limpar seleções anteriores;
- percorrer listas e componentes com scroll;
- aplicar valores configurados;
- tratar filtros em iframe;
- validar atualização da interface.

---

### `template_runner.py` — Execução parametrizada

Responsável por:

- carregar templates JSON;
- aplicar filtros definidos;
- selecionar dinamicamente o maior valor disponível quando configurado;
- executar cenários em páginas específicas;
- coordenar chamadas de exportação.

### Observação importante

A versão analisada possui risco de classificar um template como concluído sem validar efetivamente quantos arquivos foram exportados. Esse ponto está registrado no roadmap técnico do projeto.

---

### `downloader.py` — Exportação de visuais

Responsável por:

- identificar elementos visuais;
- diferenciar filtros e cards de navegação;
- localizar menus de opções;
- encontrar a ação de exportação;
- conduzir o diálogo de download;
- salvar arquivos Excel;
- registrar screenshots e tempos relacionados à etapa.

---

### `screenshotter.py` — Evidências visuais

Responsável por:

- capturar screenshots ao longo da execução;
- registrar informações temporais;
- consolidar as imagens em PDF diagnóstico.

O material analisado contém um PDF de diagnóstico que registra diferentes etapas da execução, incluindo uma falha de navegação em subpágina.

---

### `profiler.py` — Telemetria de desempenho

Responsável por:

- medir o tempo gasto em cada etapa;
- gerar relatórios estruturados;
- auxiliar na identificação de gargalos.

A análise encontrou relatórios de desempenho relacionados a uma execução em que a maior parte do tempo ficou concentrada na espera pelo carregamento do Power BI.

---

### `recorder.py` — Gravação e replay de interações

Responsável por:

- registrar ações realizadas;
- capturar estado antes e depois das interações;
- detectar mudanças de página;
- salvar evidências;
- tentar reproduzir sequências registradas.

A funcionalidade está implementada em código, mas não foram encontrados artefatos suficientes para afirmar que o replay foi utilizado com sucesso na execução analisada.

---

### `app_gui.py` + `gui/APP.html` — Interface desktop

Responsáveis por fornecer uma experiência visual para operação da automação.

Recursos identificados:

- criação e edição de templates;
- início de execução;
- início de scan;
- acompanhamento de logs;
- indicação de progresso;
- seleção de arquivos;
- preparação de configurações relacionadas a tabelas para apresentações.

---

## Interface desktop

A interface transforma o projeto em uma ferramenta operacional, reduzindo a necessidade de executar scripts manualmente ou alterar arquivos diretamente.

### Áreas identificadas

| Área | Finalidade |
|---|---|
| Execução | Iniciar o processamento configurado |
| Templates | Criar e gerenciar cenários de filtro e página |
| Scan | Identificar páginas, filtros e valores disponíveis |
| Logs | Acompanhar mensagens durante a execução |
| Informações | Apresentar informações do projeto |
| Tabelas para slides | Preparar configurações visuais para apresentações |

---

## Configuração de tabelas para apresentações

A interface possui uma área de preparação de tabelas destinadas a slides.

Foram identificadas configurações para:

- seleção do arquivo de origem;
- seleção de arquivo `.pptx`;
- inspeção de imagens existentes nos slides;
- definição de colunas;
- ordenação e nomes de exibição;
- alinhamento;
- quebra de linha;
- largura;
- formato de dados;
- casas decimais;
- prefixo e sufixo;
- estilos de cabeçalho;
- bordas;
- margens;
- posicionamento da tabela.

### Estado atual dessa funcionalidade

A configuração visual está presente na interface analisada. Entretanto, a geração automática de apresentações PowerPoint a partir dessas configurações **não foi confirmada como integrada ao backend**.

Por isso, o projeto apresenta essa capacidade como:

> Módulo de configuração visual para tabelas de apresentações em evolução.

E não como:

> Geração automática de apresentações concluída.

---

## Diagnóstico e rastreabilidade

Um dos principais diferenciais do projeto é a preocupação com diagnóstico operacional.

### Evidências visuais

O sistema registra screenshots durante etapas relevantes da execução e pode consolidá-las em um documento PDF.

Isso permite:

- localizar visualmente o ponto de falha;
- verificar se o relatório carregou;
- observar mudanças de página;
- registrar estados intermediários da interface.

### Medição de desempenho

O profiler registra tempo por etapa e produz relatórios em formato textual e estruturado.

Isso permite:

- identificar gargalos;
- comparar etapas mais lentas;
- investigar se o problema está na navegação, carregamento, filtro ou exportação.

### Replay e investigação

A presença de um módulo de gravação e replay demonstra uma estratégia adicional para diagnosticar fluxos difíceis de automatizar apenas com seletores convencionais.

---

## Desafios técnicos enfrentados

### Interfaces altamente dinâmicas

Relatórios Power BI podem conter elementos que:

- aparecem somente após hover;
- mudam conforme os filtros;
- carregam de forma assíncrona;
- ficam ocultos em componentes de navegação;
- dependem de cards ou carousels.

A solução incorpora estratégias alternativas de interação para lidar com esses cenários.

---

### Filtros personalizados

A aplicação identifica e trata:

- slicers tradicionais;
- listas roláveis;
- componentes personalizados;
- elementos em iframe;
- valores carregados progressivamente.

Esse ponto é relevante porque automações simples frequentemente falham ao lidar com filtros personalizados.

---

### Navegação em múltiplos níveis

O relatório pode conter páginas acessíveis diretamente ou subpáginas abertas a partir de cards internos.

A solução implementa estratégias diferentes para cada cenário, além de validação após a navegação.

---

### Sincronização e estabilidade

A automação utiliza esperas e validações para reduzir erros causados por:

- renderização incompleta;
- spinners;
- número variável de containers;
- menus que ainda não estão disponíveis;
- mudança de estado do relatório.

---

## Tecnologias utilizadas

### Automação e execução

<div>

<img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/Playwright-2EAD33?style=flat-square&logo=playwright&logoColor=white" />
<img src="https://img.shields.io/badge/asyncio-Asynchronous%20Execution-1F6FEB?style=flat-square" />
<img src="https://img.shields.io/badge/Microsoft_Edge-Browser%20Control-0078D7?style=flat-square&logo=microsoftedge&logoColor=white" />

</div>

<br>

### Interface e configuração

<div>

<img src="https://img.shields.io/badge/pywebview-Desktop%20GUI-4B5563?style=flat-square" />
<img src="https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white" />
<img src="https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white" />
<img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black" />
<img src="https://img.shields.io/badge/JSON-Templates-000000?style=flat-square&logo=json&logoColor=white" />

</div>

<br>

### Relatórios, arquivos e diagnóstico

<div>

<img src="https://img.shields.io/badge/Power_BI-F2C811?style=flat-square&logo=powerbi&logoColor=black" />
<img src="https://img.shields.io/badge/Excel-217346?style=flat-square&logo=microsoftexcel&logoColor=white" />
<img src="https://img.shields.io/badge/openpyxl-Excel%20Files-217346?style=flat-square" />
<img src="https://img.shields.io/badge/ReportLab-PDF%20Reports-C8102E?style=flat-square" />
<img src="https://img.shields.io/badge/Pillow-Image%20Processing-306998?style=flat-square" />

</div>

<br>

### Capacidades técnicas

<div>

<img src="https://img.shields.io/badge/Templates-Parameterized%20Execution-8B5CF6?style=flat-square" />
<img src="https://img.shields.io/badge/Profiler-Performance%20Analysis-F1B64B?style=flat-square" />
<img src="https://img.shields.io/badge/Screenshots-Visual%20Traceability-1F6FEB?style=flat-square" />
<img src="https://img.shields.io/badge/Replay-Diagnostic%20Fallback-30363D?style=flat-square" />

</div>

---

## Diferenciais profissionais

O **PBI Automation Studio** demonstra experiência em:

- automação web assíncrona com Playwright;
- interação com aplicações analíticas complexas;
- navegação resiliente em interfaces dinâmicas;
- parametrização de execução por templates;
- identificação e manipulação de filtros;
- tratamento de componentes customizados;
- construção de interface desktop para usuários;
- captura de evidências visuais;
- análise de desempenho da automação;
- criação de soluções orientadas a diagnóstico e manutenção.

O diferencial central do projeto não está apenas em exportar dados, mas em construir uma ferramenta capaz de observar, diagnosticar e estruturar fluxos repetitivos de interação com dashboards.

---

## Evidências e limitações

### Evidências identificadas

A versão analisada contém:

- código modular da automação;
- interface desktop;
- templates e configurações;
- relatório textual de desempenho;
- relatório JSON de desempenho;
- PDF diagnóstico com capturas de execução.

### Limitações atuais

Não foram encontrados no material analisado:

- arquivos `.xlsx` exportados pela automação;
- evidência de ciclo completo de exportação concluído com sucesso;
- comprovação de geração automática de PowerPoint;
- evidência de tratamento específico de alerta de confidencialidade;
- gravações utilizadas com sucesso em replay;
- métricas consolidadas de ganho de produtividade.

---

## Demonstração pública planejada

A versão pública do projeto deverá utilizar um dashboard inteiramente fictício, sem conexão com ambientes reais.

### Cenário demonstrativo sugerido

```text
Template selecionado: Resumo Mensal — Categoria A
        ↓
Dashboard fictício carregado
        ↓
Página selecionada: RESUMO
        ↓
Filtro Período aplicado: último valor disponível
        ↓
Filtro Categoria aplicado: CATEGORIA_A
        ↓
Visual exportável identificado: Tabela Principal
        ↓
Exportação demonstrativa gerada
        ↓
Relatório visual de diagnóstico apresentado
```

### Conteúdo visual recomendado

- tela inicial da aplicação;
- cadastro de template fictício;
- dashboard mockado;
- GIF da execução;
- log demonstrativo;
- PDF diagnóstico fictício;
- gráfico de tempo por etapa;
- tela de configuração de tabela para slides marcada como recurso em evolução.

---

## Exemplo de log demonstrativo

```text
[DEMONSTRAÇÃO] INFO  | Iniciando execução do template DEMO-001
[DEMONSTRAÇÃO] INFO  | Navegador controlado iniciado
[DEMONSTRAÇÃO] INFO  | Dashboard fictício carregado
[DEMONSTRAÇÃO] INFO  | Página selecionada: RESUMO
[DEMONSTRAÇÃO] INFO  | Filtro "periodo" aplicado: último valor disponível
[DEMONSTRAÇÃO] INFO  | Filtro "categoria" aplicado: CATEGORIA_A
[DEMONSTRAÇÃO] INFO  | Visual exportável identificado: Tabela Principal
[DEMONSTRAÇÃO] INFO  | Exportação fictícia concluída: resultado_demo.xlsx
[DEMONSTRAÇÃO] INFO  | Screenshot registrado
[DEMONSTRAÇÃO] INFO  | Relatório diagnóstico gerado
```

---

## Segurança e confidencialidade

Este repositório deve ser publicado somente como versão demonstrativa e anonimizada.

A versão pública **não disponibiliza**:

- URL real do relatório;
- identificadores de relatórios;
- nomes reais de páginas ou áreas;
- filtros ou valores utilizados em ambiente operacional;
- templates reais;
- arquivos exportados em execução real;
- screenshots ou PDFs do dashboard original;
- caminhos locais ou corporativos;
- perfis de navegador;
- sessões autenticadas;
- cookies;
- tokens;
- credenciais;
- arquivos de configuração produtivos.

Todo conteúdo público deverá utilizar:

- nomes genéricos;
- filtros fictícios;
- templates demonstrativos;
- dashboards mockados;
- logs sanitizados;
- métricas claramente indicadas como simulação.

---

## Estrutura recomendada para publicação segura

```text
pbi-automation-studio/
│
├── README.md
├── .gitignore
├── requirements.txt
│
├── assets/
│   ├── architecture.png
│   ├── application-preview.png
│   └── demo.gif
│
├── docs/
│   ├── architecture.md
│   ├── security.md
│   └── publication-roadmap.md
│
├── demo/
│   ├── index.html
│   ├── fake_dashboard.json
│   ├── fake_templates.json
│   ├── fake_execution_log.txt
│   └── fake_performance_report.json
│
└── examples/
    └── template.example.json
```

---

## Roadmap

### Publicação segura

- [x] Validar arquitetura e funcionalidades implementadas.
- [x] Separar capacidades confirmadas de itens não comprovados.
- [x] Definir posicionamento profissional do case.
- [ ] Remover arquivos compilados e artefatos sensíveis.
- [ ] Criar configurações fictícias.
- [ ] Substituir templates reais por exemplos genéricos.
- [ ] Criar dashboard demonstrativo.
- [ ] Gerar screenshots e GIFs seguros.
- [ ] Publicar somente a versão sanitizada.

### Melhorias técnicas

- [ ] Validar sucesso de exportação por arquivo efetivamente salvo.
- [ ] Padronizar origem das configurações utilizadas nos testes.
- [ ] Criar tratamento específico para pop-ups interferentes.
- [ ] Centralizar seletores e timeouts.
- [ ] Validar cache de visuais antes de reutilizar índices.
- [ ] Integrar ou remover módulos desconectados do fluxo principal.
- [ ] Criar testes automatizados em ambiente demonstrativo.

### Funcionalidades em evolução

- [ ] Geração automática de apresentações a partir das tabelas configuradas.
- [ ] Dashboard de acompanhamento da execução.
- [ ] Métricas consolidadas de sucesso, falhas e tempo por cenário.
- [ ] Sanitização automática de evidências geradas.

---

## Estado atual

O **PBI Automation Studio** possui uma base técnica robusta para apresentação em portfólio, com arquitetura modular, interface desktop, automação assíncrona, parametrização por templates e mecanismos de diagnóstico.

A versão analisada comprova implementação e evidências de navegação/diagnóstico, mas não comprova ainda a conclusão bem-sucedida de exportações em ambiente real.

Por esse motivo, o projeto é apresentado publicamente como:

> Aplicação desktop modular para automação e diagnóstico de interações em relatórios Power BI, com navegação resiliente, parametrização de filtros, rotina implementada para exportação de visuais e rastreabilidade da execução.

---

<div align="center">

### Automação analítica com rastreabilidade, templates e diagnóstico operacional

<br>

<img src="https://img.shields.io/badge/Python-Developer-3776AB?style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/RPA-Analytics%20Automation-6C63FF?style=for-the-badge" />
<img src="https://img.shields.io/badge/Power_BI-Diagnostic%20Runner-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" />

<br><br>

**Python • Playwright • Power BI • Desktop GUI • Templates • Observabilidade**

<br><br>

<sub>Projeto independente de portfólio. Power BI e Microsoft Edge são marcas da Microsoft.</sub>

</div>
