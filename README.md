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
- <a href="">Nome do Tutor</a>
### Coordenador(a)
- <a href="">Nome do Coordenador</a>
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
