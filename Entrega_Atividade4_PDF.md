ATIVIDADE 4 — AVALIAÇÃO FINAL
Construção e Publicação de Dashboard de Dados Públicos

Aluna: Milenna Rezende da Silva
RA: 26001255159
Programa: RESTEC INTEGRE
Instituição: Receita Estadual do Paraná — SEFA-PR
Data de entrega: 15 de julho de 2026

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

LINK DE ACESSO AO DASHBOARD PUBLICADO

https://millena-restec.onrender.com/

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. BASE DE DADOS UTILIZADA

A base utilizada nesta atividade é a lista pública de empresas enquadradas no
Regime Especial de Devedores Contumazes da Receita Estadual do Paraná
(REPR/SEFA-PR), disponibilizada diretamente no portal oficial:

  https://www.fazenda.pr.gov.br/Pagina/Consultar-Devedores-Contumazes

Essa base reúne os contribuintes do ICMS que, de forma reiterada e deliberada,
deixam de recolher o imposto aos cofres do Estado do Paraná, mesmo realizando
operações tributáveis regularmente. O enquadramento no Regime Especial implica
restrições operacionais severas, como a obrigatoriedade de pagamento antecipado
do ICMS e o acompanhamento fiscal diferenciado.

A publicização dos dados é amparada legalmente pelo art. 198, §1°, inciso II, do
Código Tributário Nacional (CTN) e pelo art. 52 da Lei Estadual n° 11.580/1996,
que autorizam a divulgação de informações fiscais quando há representação de
interesse público, sem exposição de dados pessoais ou sujeitos ao sigilo.

Os registros incluem: CNPJ do contribuinte, razão social, número e data de
publicação no Diário Oficial do Estado (DIOE) e número do Termo de
Enquadramento (REPR). Não há dados pessoais, CPFs ou informações protegidas
pela LGPD — trata-se exclusivamente de dados de pessoas jurídicas submetidas a
um regime restritivo de natureza pública.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

2. TRATAMENTOS REALIZADOS

Os dados são carregados diretamente da página pública da SEFA-PR por meio de
raspagem (web scraping) automatizada, realizada a cada acesso ao dashboard
(com cache de 1 hora para evitar sobrecarga ao servidor da Fazenda). Foram
realizados os seguintes tratamentos:

a) Padronização de colunas: os nomes dos campos extraídos da tabela HTML foram
   mapeados para um padrão interno consistente (CNPJ, Razao_Social,
   Termo_Enquadramento, Num_DIOE, Data_DIOE), independentemente das variações
   de nomenclatura presentes na fonte original.

b) Limpeza e tipagem: o CNPJ foi tratado como texto para preservar a formatação;
   a razão social foi padronizada em Title Case; a data de publicação no DIOE foi
   convertida para o tipo datetime com suporte a múltiplos formatos de entrada.

c) Criação de indicadores derivados:
   — Meses_Enquadrado: calculado como a diferença em meses entre a data de
     publicação do enquadramento e a data atual, representando há quanto tempo
     o contribuinte permanece sob regime restritivo.
   — Ano_DIOE: ano de publicação, extraído da data para análise de série temporal.
   — CNPJ_Raiz: os 8 primeiros dígitos do CNPJ, que identificam o grupo econômico
     de origem.
   — Delegacia: estimativa da Delegacia Regional da Receita (DRR) com base nos
     dígitos do CNPJ, aproximando a localização geográfica do contribuinte nas
     regiões de Curitiba, Londrina, Maringá, Cascavel, Ponta Grossa e Francisco
     Beltrão.
   — Grupo_Econômico: classificação que identifica se o CNPJ pertence a um grupo
     com múltiplos CNPJs na lista (potencial fragmentação societária deliberada) ou
     se é um contribuinte isolado.

d) Tratamento de indisponibilidade: caso a página da SEFA-PR esteja temporariamente
   inacessível (manutenção, timeout ou bloqueio de IP), o sistema exibe
   automaticamente um dataset representativo gerado com registros fictícios, mas
   estruturalmente equivalentes à base real, garantindo a funcionalidade do
   dashboard em qualquer condição.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

3. INDICADORES CONSTRUÍDOS E VISUAIS DO DASHBOARD

O dashboard foi construído em Python com a biblioteca Streamlit e gráficos
interativos via Plotly. Está organizado em uma única página com os seguintes
elementos:

CARTÕES DE INDICADORES (KPIs):
  • Total de Contribuintes: quantidade total de empresas enquadradas no regime,
    aplicados os filtros selecionados.
  • TME — Tempo Médio de Enquadramento: mediana (em meses) do período que os
    contribuintes permanecem sob regime restritivo desde a publicação no DIOE.
  • CNPJs em Grupos Econômicos: quantidade de registros vinculados a grupos com
    múltiplos CNPJs, e o percentual correspondente sobre o total filtrado.
  • Publicação Mais Recente no DIOE: data do enquadramento mais recente
    presente na base, indicando a atualidade dos dados.

VISUAIS GRÁFICOS:
  • Gráfico 1 — Evolução Anual dos Enquadramentos (série temporal em área):
    mostra a quantidade de empresas enquadradas por ano de publicação no DIOE,
    evidenciando tendências de crescimento ou redução da inadimplência crônica.

  • Gráfico 2 — Distribuição Territorial por Delegacia Regional (barras
    horizontais): exibe a concentração de devedores por região do Paraná,
    permitindo identificar quais delegacias da Receita concentram a maior
    incidência de contribuintes contumazes.

  • Gráfico 3 — Distribuição do Tempo de Enquadramento — TME (histograma):
    apresenta como os contribuintes se distribuem pelo tempo que permanecem
    sob regime restritivo, com linha de mediana destacada. Permite identificar
    casos crônicos de longa duração.

  • Gráfico 4 — Perfil de Grupos Econômicos — TEG (gráfico de rosca):
    diferencia os contribuintes com CNPJ único daqueles pertencentes a grupos
    com múltiplos CNPJs, evidenciando eventuais estratégias de fragmentação
    societária para evasão fiscal.

TABELA DETALHADA:
  • Relação completa dos contribuintes enquadrados com CNPJ, razão social, termo
    de enquadramento, número e data do DIOE, TME em meses, delegacia regional e
    perfil do contribuinte — com suporte a busca e filtragem.

FILTROS (SIDEBAR):
  • Filtro por Delegacia Regional (multiselect)
  • Filtro por Ano de Publicação no DIOE (multiselect)
  • Filtro por Perfil do Contribuinte — CNPJ único ou grupo econômico (multiselect)
  • Busca livre por razão social (campo de texto)

SEÇÃO DE APOIO À DECISÃO:
  • Três cartões explicativos descrevem, em linguagem simples e acessível, como
    cada indicador (TME, DRR e TEG) pode fundamentar decisões concretas da
    administração tributária estadual.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

4. COMO O DASHBOARD APOIA UMA DECISÃO PÚBLICA

O painel foi desenhado para apoiar decisões de gestão na área de fiscalização e
recuperação de créditos tributários do ICMS no Estado do Paraná. Três linhas
principais de decisão são diretamente suportadas:

(i) PRIORIZAÇÃO DE EXECUÇÕES FISCAIS E PRORROGAÇÃO DO REGIME ESPECIAL
O indicador TME (Tempo Médio de Enquadramento) permite identificar contribuintes
que permanecem sob regime restritivo por períodos muito superiores à mediana,
sinalizando casos de inadimplência crônica intratável. Esses contribuintes são
candidatos prioritários para medidas mais drásticas, como ajuizamento de execução
fiscal acelerada, medidas cautelares fiscais ou arrolamento de bens — decisões que
demandam priorização dada a limitação de recursos humanos nas equipes de
fiscalização.

(ii) ALOCAÇÃO DE EQUIPES FISCAIS POR REGIÃO
O indicador DRR (Distribuição Regional de Devedores) permite à gestão da SEFA-PR
identificar quais delegacias regionais concentram o maior número de contribuintes
contumazes. Isso orienta a alocação de auditores fiscais, a definição de mutirões
de fiscalização presencial e o dimensionamento de equipes por território, com base
em dados objetivos em vez de percepções subjetivas.

(iii) IDENTIFICAÇÃO DE GRUPOS ECONÔMICOS E PREVENÇÃO DE EVASÃO ESTRUTURADA
O indicador TEG (Taxa de Efeito em Grupos Econômicos) destaca contribuintes cujos
grupos empresariais possuem múltiplos CNPJs simultaneamente enquadrados. Esse
padrão pode indicar estratégia deliberada de fragmentação societária para diluir
responsabilidades e dificultar a recuperação do crédito. O dado orienta o
ajuizamento de Ações Cautelares Fiscais, a desconsideração da personalidade
jurídica em processos administrativos e o monitoramento preventivo de grupos
empresariais específicos.

O painel também pode embasar relatórios gerenciais periódicos para a Coordenadoria
de Arrecadação (CAC) e a Divisão de Controle e Recuperação de Créditos (DCRC),
permitindo o acompanhamento da evolução da base de devedores contumazes ao longo
do tempo sem dependência de extração manual de dados.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

5. LIMITES E CUIDADOS COM OS DADOS

Embora a base seja pública e legalmente amparada, é importante reconhecer alguns
limites do painel:

• A estimativa de delegacia regional por CNPJ é uma aproximação heurística, não um
  dado oficial da SEFA-PR. Pode haver inconsistências para contribuintes de outros
  estados com inscrição estadual no Paraná.

• A data de enquadramento corresponde à publicação no DIOE, não necessariamente
  ao início efetivo das restrições operacionais.

• A base reflete os contribuintes atualmente enquadrados; histórico de empresas
  que saíram do regime (por regularização ou encerramento de atividade) não está
  disponível nessa fonte pública.

• Não há dados de valor do débito, apenas dados cadastrais e de enquadramento.
  Para análise financeira, seria necessário cruzamento com outros sistemas internos
  da SEFA-PR, fora do escopo desta atividade.

• O painel utiliza apenas dados de pessoas jurídicas. Não há qualquer dado pessoal,
  CPF ou informação sujeita à proteção da LGPD.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

REFERÊNCIAS

• Receita Estadual do Paraná — SEFA-PR. Consulta de Empresas Enquadradas no
  Regime Especial — Devedores Contumazes.
  Disponível em: https://www.fazenda.pr.gov.br/Pagina/Consultar-Devedores-Contumazes

• Brasil. Código Tributário Nacional. Art. 198, §1°, inciso II.

• Paraná. Lei Estadual n° 11.580/1996. Art. 52.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Milenna Rezende da Silva — RA 26001255159
Programa RESTEC INTEGRE — Receita Estadual do Paraná / SEFA-PR
