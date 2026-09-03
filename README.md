# FIAP - Faculdade de Informática e Administração Paulista
 
<p align="center">
<a href="https://www.fiap.com.br/"><img src="assets/logo-fiap.png" alt="FIAP" border="0" width=40% height=40%></a>
</p>
<br>
# CardioIA — Fase 1: Batimentos de Dados
 
## Nome do grupo
 
<!-- PREENCHER -->
 
## 👨‍🎓 Integrantes:
- <a href="https://www.linkedin.com/in/pedrogu-moreira">Pedro Gustavo França Moreira</a>
<!-- PREENCHER demais integrantes -->
 
## 👩‍🏫 Professores:
 
### Tutor(a)
- <a href="">Sabrina Otoni</a>
### Coordenador(a)
- <a href="">Adré Godoi</a>
---
 
## 📜 Descrição
 
As doenças cardiovasculares são a principal causa de morte no mundo, com aproximadamente 17,9 milhões de óbitos anuais. Infartos, arritmias, insuficiência cardíaca e AVCs são condições comuns que podem ser prevenidas com diagnóstico precoce — e é exatamente nesse ponto que a Inteligência Artificial tem potencial de impacto real, antecipando eventos críticos e personalizando cuidados.
 
O **CardioIA** é um projeto acadêmico que simula o ecossistema digital de uma cardiologia moderna, integrando dados clínicos, modelos de Machine Learning, Visão Computacional, IoT e agentes inteligentes ao longo de sete fases.
 
Esta **Fase 1 — Batimentos de Dados** ocupa a posição de fundação do projeto. Aqui não se constrói modelo: constrói-se a base sobre a qual todos os módulos seguintes serão treinados. O papel assumido é o de cientista de dados hospitalar, responsável por levantar, organizar e documentar criticamente três tipos de dados cardiológicos:
 
- **Dados numéricos** — variáveis clínicas de pacientes, que alimentarão os classificadores de risco da Fase 2;
- **Dados textuais** — literatura científica sobre saúde cardiovascular, corpus inicial para os módulos de NLP e para o assistente virtual da Fase 5;
- **Dados visuais** — imagens de eletrocardiograma, insumo para os modelos de Visão Computacional da Fase 4.
Mais do que coletar, esta fase exige **pensamento crítico sobre a origem e a qualidade desses dados**. Um sistema de IA aplicado à saúde herda todos os vieses da base que o treinou, e erros de diagnóstico têm custo humano assimétrico. Por isso, este documento dedica uma seção específica à governança de dados, discutindo procedência, licenciamento, representatividade e limitações de cada conjunto — conforme os conceitos abordados no capítulo de Governança em IA desta fase.
 
---
 
## 🩺 Parte 1 — Dados Numéricos
 
### Fonte e procedência
 
| Item | Descrição |
|---|---|
| Dataset | Heart Failure Prediction Dataset |
| Autor | fedesoriano (Kaggle) |
| Link original | https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction |
| Licença | Open Database License (ODbL) |
| Registros | 918 |
| Variáveis | 12 |
| Origem | Combinação de 5 bases independentes: Cleveland, Hungria, Suíça, Long Beach VA e Statlog |
| Formato | .csv |
| Localização no repositório | `assets/dados/heart.csv` |
 
### Dicionário de variáveis
 
| Variável | Descrição | Relevância clínica |
|---|---|---|
| `Age` | Idade em anos | Quanto maior a idade, maior a probabilidade de evento cardíaco — o risco cardiovascular é cumulativo ao longo da vida |
| `Sex` | Sexo (M/F) | Homens e mulheres apresentam perfis de risco distintos, com diferenças de predisposição e de apresentação clínica. Vale registrar que a maioria dos estudos históricos em cardiologia foi conduzida em homens, o que gera lacuna de conhecimento sobre a população feminina |
| `ChestPainType` | Tipo de dor torácica (TA, ATA, NAP, ASY) | O padrão da dor orienta a suspeita: angina típica sugere obstrução coronariana, enquanto o quadro assintomático (ASY) é traiçoeiro justamente por não gerar alerta no paciente |
| `RestingBP` | Pressão arterial em repouso (mm Hg) | Hipertensão crônica sobrecarrega o músculo cardíaco e danifica as artérias ao longo dos anos |
| `Cholesterol` | Colesterol sérico (mg/dl) | Colesterol elevado forma placas de gordura que estreitam as artérias e reduzem o fluxo sanguíneo ao coração |
| `FastingBS` | Glicemia em jejum > 120 mg/dl (1 = sim) | Glicemia alta indica diabetes, que danifica os vasos sanguíneos ao longo do tempo e acelera o processo de obstrução arterial |
| `RestingECG` | Eletrocardiograma em repouso (Normal, ST, LVH) | Detecta alterações estruturais e elétricas do coração, como a hipertrofia do ventrículo esquerdo — geralmente consequência de anos de pressão alta |
| `MaxHR` | Frequência cardíaca máxima atingida | Um coração saudável atinge frequências altas sob esforço. Quem tem doença coronariana não consegue elevar a frequência como deveria, portanto **frequência máxima baixa é o sinal de alerta** |
| `ExerciseAngina` | Angina induzida por exercício (Y/N) | Dor torácica durante o esforço ocorre quando o coração demanda mais oxigênio e as artérias obstruídas não conseguem entregar. Sintoma clássico de isquemia |
| `Oldpeak` | Depressão do segmento ST | Marcador mais direto de isquemia no teste ergométrico: quanto maior a depressão, mais o coração sofre sob carga |
| `ST_Slope` | Inclinação do segmento ST no pico do exercício | Inclinação ascendente (*Up*) é o padrão normal; plana (*Flat*) ou descendente (*Down*) indica comprometimento |
| `HeartDisease` | Classe alvo (1 = doença, 0 = normal) | Variável resposta — é o que os modelos das fases seguintes tentarão prever |
 
### Variáveis de maior relevância
 
Entre as onze variáveis preditoras, destaco quatro como as mais relevantes do ponto de vista clínico:
 
**`Age`**, porque o risco cardiovascular é cumulativo e a idade funciona como proxy de exposição acumulada a todos os demais fatores.
 
**`ChestPainType`**, porque é o sintoma que efetivamente leva o paciente ao consultório e orienta a conduta médica inicial — e porque a categoria assintomática revela um grupo de risco que não se queixa.
 
**`MaxHR`** e **`Oldpeak`**, porque são medidas obtidas sob esforço, quando o coração está em demanda máxima. Alterações que permanecem invisíveis em repouso se manifestam nessas condições, o que confere a essas variáveis alto poder discriminativo.
 
### Contexto clínico da coleta
 
Quatro variáveis do conjunto — `MaxHR`, `ExerciseAngina`, `Oldpeak` e `ST_Slope` — só podem ser medidas durante um **teste ergométrico**, exame em que o paciente é monitorado enquanto se exercita em esteira.
 
Isso significa que estes dados **não vêm de consulta de rotina nem de triagem populacional**. Vêm de pacientes que foram encaminhados a um exame específico, e ninguém é encaminhado a um teste ergométrico sem motivo clínico prévio.
 
Essa constatação tem consequências diretas sobre o uso do dataset, discutidas na seção de Governança.
 
---
 
## 📚 Parte 2 — Dados Textuais (NLP)
 
### Textos selecionados
 
| # | Título | Fonte | Licença | Arquivo |
|---|---|---|---|---|
| 1 | Influência das ondas de calor extremo como agravante na causa de morte por doenças cardiovasculares no Sudeste do Brasil | Cadernos de Saúde Pública, 2026, v.42 · DOI 10.1590/0102-311xen088725 | CC BY 4.0 | `assets/textos/texto1.txt` |
| 2 | Noncommunicable chronic diseases and health challenges in 2050 | Revista Brasileira de Epidemiologia, 2026, v.29 · DOI 10.1590/1980-549720260011 | CC BY 4.0 | `assets/textos/texto2.txt` |
 
Ambos são artigos científicos revisados por pares, de acesso aberto, publicados em periódicos brasileiros de saúde pública. Foram selecionados por tratarem de ângulos complementares do mesmo problema: o primeiro analisa um fator ambiental agravando a mortalidade cardiovascular; o segundo projeta a carga de doenças crônicas e seus fatores de risco até 2050.
 
### Aplicações possíveis de NLP
 
**Extração de entidades médicas.** Os textos contêm termos como *hipertensão arterial*, *diabetes*, *tabagismo*, *doença isquêmica do coração* e *neoplasias*. Um algoritmo de reconhecimento de entidades nomeadas poderia identificar automaticamente esses termos e transformar texto corrido em dados estruturados, gerando uma base consultável de fatores de risco cardiovascular sem que alguém precise ler artigo por artigo.
 
**Classificação de tópicos.** Os dois textos tratam de temas distintos — um de fator ambiental e clima, outro de carga de doença crônica e projeções futuras. Essa diferença permite treinar um classificador capaz de organizar automaticamente grandes volumes de literatura médica por assunto, filtrando o que é relevante para cada módulo do CardioIA.
 
**Sumarização.** Artigos científicos são longos e densos. Um modelo de sumarização permitiria extrair o essencial de cada publicação em poucas linhas, viabilizando que tanto o sistema quanto o profissional de saúde acompanhem a produção científica sem leitura integral.
 
**Por que isso importa para o CardioIA.** As fases seguintes dependem de dados textuais estruturados. Estes artigos constituem o corpus inicial que alimentará os módulos de NLP e o assistente cardiológico virtual previsto para a Fase 5.
 
---
 
## 🖼️ Parte 3 — Dados Visuais (Visão Computacional)
 
### Conjunto de imagens
 
| Item | Descrição |
|---|---|
| Tipo de exame | Eletrocardiograma (ECG) |
| Quantidade | 150 imagens |
| Classes | 3 categorias, 50 imagens de cada |
| Formato | .png |
| Fonte | `ecg_image_data` — versão em imagem de base de arritmias (Kaggle) |
| Link público | https://drive.google.com/drive/folders/1OWdyDjdoBs6eL8UMjdhZfBS87OExg6Ah?usp=sharing |
 
### Aplicações possíveis de Visão Computacional
 
**Classificação de traçados.** Uma rede neural convolucional (CNN) poderia aprender a distinguir as três classes presentes no conjunto, identificando padrões de traçado que diferenciam registros normais de alterados.
 
**Detecção de padrões.** As imagens de ECG apresentam variações de amplitude e de intervalo entre batimentos que são difíceis de formalizar em regras manuais, mas que uma rede convolucional consegue aprender a partir de exemplos rotulados.
 
**Segmentação.** Técnicas de segmentação permitiriam isolar automaticamente os componentes do traçado — complexo QRS, onda P, segmento ST — viabilizando medições automatizadas de intervalos.
 
### Decisão de curadoria
 
Selecionei **50 imagens de cada uma das três classes**, em vez de amostragem aleatória sobre o conjunto completo. Uma base desbalanceada levaria o modelo a aprender a chutar a classe majoritária, inflando a acurácia sem qualquer ganho real de capacidade diagnóstica — problema conhecido em tarefas de classificação clínica, onde as classes raras costumam ser justamente as mais importantes.
 
### Limitações
 
Imagens provenientes de dataset público não substituem laudo médico. Variam em qualidade de digitalização, não vêm acompanhadas do contexto clínico do paciente e não permitem verificar a acurácia da rotulagem original. Seu uso é adequado para treinamento e experimentação acadêmica, não para diagnóstico.
 
---
 
## ⚖️ Governança de Dados e Viés
 
A tabela de riscos do ciclo de vida de dados e modelos aponta que as etapas de **coleta** e **curadoria** — as duas trabalhadas nesta fase — estão sujeitas a dados inconsistentes ou enviesados e à perda de diversidade. As contramedidas indicadas são políticas claras de coleta e validação e análises de qualidade e balanceamento. As fases seguintes do CardioIA introduzirão riscos de modelagem, validação e implantação, que exigirão testes de fairness, validação externa e monitoramento contínuo.
 
### 1. Procedência
 
Os três conjuntos têm origem documentada e verificável. O dataset numérico é uma combinação de cinco bases clínicas independentes (Cleveland, Hungria, Suíça, Long Beach VA e Statlog), coletadas em países e períodos distintos. Os textos são artigos revisados por pares com DOI registrado. As imagens provêm de base pública de arritmias disponibilizada no Kaggle.
 
A combinação de cinco bases no conjunto numérico é, em si, um ponto de atenção: critérios de coleta, equipamentos e protocolos de exame provavelmente diferiram entre os centros, o que introduz heterogeneidade não documentada.
 
### 2. Licenciamento e uso ético
 
O dataset numérico está sob Open Database License; os artigos, sob Creative Commons BY 4.0, que permite uso com atribuição. Nenhum dos conjuntos contém dado pessoal identificável.
 
Caso o projeto utilizasse registros de pacientes brasileiros reais, aplicar-se-ia a LGPD: dados de saúde são classificados como dados pessoais sensíveis, exigindo base legal específica, consentimento informado ou amparo em tutela da saúde, além de medidas técnicas de anonimização e controle de acesso.
 
### 3. Viés de seleção — principal limitação identificada
 
Como registrado na Parte 1, quatro variáveis do dataset numérico só existem porque os pacientes realizaram teste ergométrico. Encaminhamento para esse exame não é aleatório: parte de suspeita clínica prévia, sintoma relatado ou fator de risco identificado.
 
**A base, portanto, não representa a população geral — representa pessoas já sob investigação cardiológica.** A prevalência de doença cardíaca no conjunto é necessariamente muito superior à da população brasileira.
 
Duas consequências práticas:
 
Um modelo treinado nesta base e aplicado como ferramenta de triagem populacional superestimaria risco de forma sistemática, porque aprendeu em um universo onde quase todos apresentavam algum indício de problema.
 
E os registros classificados como `HeartDisease = 0` **não correspondem a pessoas saudáveis**. Correspondem a pessoas que tiveram sintomas, foram investigadas e cujo exame não confirmou a doença — categoria clinicamente distinta de "população sem queixa".
 
### 4. Qualidade de dados — erro identificado durante a coleta
 
Durante a coleta do texto 1, verifiquei a versão em português disponibilizada pelo portal e constatei que se trata de **tradução automática com erros terminológicos relevantes**:
 
| Termo original | Tradução automática | Termo correto |
|---|---|---|
| IQ (ischemia) | "deficiência intelectual" | isquemia |
| heat stress | "síndrome de hiperinsulinemia" | estresse térmico |
| correlation analyses | "análises de brilho" | análises de correlação |
 
Em um pipeline de NLP voltado à extração de entidades médicas, esses erros produziriam entidades falsas — o modelo aprenderia que "deficiência intelectual" é um desfecho cardiovascular. Por esse motivo, optei pelo texto original em inglês, registrando a decisão e o motivo.
 
Este caso ilustra concretamente por que procedência não se resume a "de onde veio o arquivo", mas inclui **por qual processo ele passou até chegar às minhas mãos**.
 
### 5. Assimetria do custo do erro em saúde
 
Em cardiologia, falso negativo e falso positivo não são erros equivalentes.
 
Um **falso negativo** — dizer que está tudo bem a quem tem doença coronariana — pode resultar em evento cardíaco fatal sem intervenção prévia. Um **falso positivo** — encaminhar para investigação alguém saudável — gera custo, exposição a exames desnecessários e ansiedade, mas raramente é fatal.
 
Um modelo otimizado apenas para acurácia global trata os dois erros como equivalentes, o que é inadequado neste domínio. As fases seguintes deverão priorizar **recall** sobre precisão e considerar limiares de decisão ajustados ao custo clínico real, não à métrica mais bonita no relatório.
 
### 6. Rastreabilidade
 
Todas as fontes estão documentadas neste README com link original, licença e data de acesso. Os arquivos textuais preservam cabeçalho com título, periódico, DOI e licença. Essa documentação permite que, em qualquer fase futura do projeto, seja possível reconstituir de onde veio cada dado que alimentou um modelo — requisito básico de auditabilidade em sistemas de IA aplicados à saúde.
 
---
 
## 📁 Estrutura de pastas
 
- **assets**: elementos não-estruturados do repositório
  - `assets/textos/` — arquivos .txt da Parte 2
  - `assets/dados/` — dataset numérico da Parte 1
- **config**: arquivos de configuração do projeto
- **document**: documentos do projeto
  - `document/resumo_fase1.md` — documento resumo da entrega
- **scripts**: scripts auxiliares
- **src**: código-fonte a ser desenvolvido ao longo das 7 fases
- **README.md**: este arquivo
---
 
## 🔧 Como executar
 
Esta fase não contém código executável. Os dados podem ser acessados da seguinte forma:
 
- **Dados numéricos**: arquivo `.csv` disponível em `assets/dados/`, legível por Excel, Pandas ou qualquer ferramenta de análise
- **Dados textuais**: arquivos `.txt` em `assets/textos/`, em codificação UTF-8
- **Dados visuais**: link público do Google Drive indicado na Parte 3
Para as fases seguintes, os pré-requisitos previstos são Python 3.10+, Pandas, NumPy, scikit-learn e ambiente Jupyter ou Google Colab.
 
---
 
## 🗃 Histórico de lançamentos
 
- 0.1.0 — 02/09/2026
    - Entrega da Fase 1: coleta e documentação dos conjuntos de dados numérico, textual e visual, com análise crítica de governança e viés.
---
 
## 📋 Licença
 
<img style="width:100px;height:35px;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg"> <img style="width:100px;height:35px;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg">
 
<a href="https://github.com/agodoi/template">MODELO GIT FIAP</a> por <a href="https://fiap.com.br">Fiap</a> está licenciado sobre <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1">Attribution 4.0 International</a>.
