# CardioIA — Fase 1: Batimentos de Dados
## Documento Resumo
 
**Aluno:** Pedro Gustavo França Moreira
**Entrega:** 02/09/2026
**Repositório:** https://github.com/pedromoreira427/cardioia-fase1
 
---
 
### Objetivo da fase
 
Levantar, organizar e documentar criticamente três conjuntos de dados cardiológicos — numéricos, textuais e visuais — que servirão de insumo para os módulos de Machine Learning, NLP e Visão Computacional das fases seguintes do projeto CardioIA.
 
---
 
### O que foi entregue
 
| Parte | Conjunto | Volume | Fonte | Licença |
|---|---|---|---|---|
| 1 | Heart Failure Prediction Dataset (.csv) | 918 registros, 12 variáveis | Kaggle (fedesoriano) | ODbL |
| 2 | Dois artigos científicos (.txt) | Cadernos de Saúde Pública e Rev. Bras. Epidemiologia | SciELO | CC BY 4.0 |
| 3 | Imagens de eletrocardiograma (.png) | 150 imagens, 3 classes balanceadas | Kaggle (ecg_image_data) | Pública |
 
Todos os conjuntos estão documentados no README principal com link de origem, licença, dicionário de variáveis e justificativa de uso nas fases seguintes.
 
---
 
### Principais decisões de curadoria
 
**Balanceamento de classes nas imagens.** Foram selecionadas 50 imagens de cada uma das três classes, em vez de amostragem aleatória, para evitar que modelos futuros aprendam a favorecer a classe majoritária — problema crítico em classificação clínica, onde as classes raras costumam ser as mais relevantes.
 
**Escolha do texto original em vez da tradução.** A versão em português do artigo 1 é gerada por tradução automática e apresenta erros terminológicos graves — "isquemia" traduzida como "deficiência intelectual", "estresse térmico" como "síndrome de hiperinsulinemia". Em um pipeline de NLP, esses erros gerariam entidades médicas falsas. Optou-se pelo texto original em inglês.
 
**Limpeza dos textos.** Foram removidos menus de navegação, referências bibliográficas, URLs de citação e resumos duplicados em outros idiomas, preservando apenas o corpo do artigo e o resumo em português, para reduzir ruído em análises futuras.
 
---
 
### Análise de governança — pontos centrais
 
**Viés de seleção no dataset numérico.** Quatro das doze variáveis (`MaxHR`, `ExerciseAngina`, `Oldpeak`, `ST_Slope`) só podem ser medidas durante teste ergométrico. Como encaminhamento a esse exame decorre de suspeita clínica prévia, a base representa pacientes já sob investigação cardiológica — não a população geral. A prevalência de doença no conjunto é, portanto, artificialmente elevada, e um modelo treinado nele superestimaria risco se aplicado como triagem populacional. Adicionalmente, os registros com `HeartDisease = 0` não correspondem a pessoas saudáveis, mas a pessoas sintomáticas cujo exame não confirmou a doença.
 
**Heterogeneidade de origem.** O dataset combina cinco bases coletadas em países e períodos distintos, com protocolos e equipamentos provavelmente diferentes — fonte de inconsistência não documentada.
 
**Assimetria do custo do erro.** Em cardiologia, falso negativo pode ser fatal; falso positivo gera custo e ansiedade. Modelos otimizados apenas por acurácia global tratam os dois como equivalentes, o que é inadequado. As fases seguintes deverão priorizar recall e ajustar limiares ao custo clínico real.
 
**Rastreabilidade.** Todas as fontes estão documentadas com link, licença e procedência, permitindo reconstituir a origem de qualquer dado que venha a alimentar um modelo nas fases futuras.
 
---
 
### Conexão com as fases seguintes
 
- **Fase 2** — os dados numéricos alimentarão os classificadores supervisionados de risco cardíaco
- **Fase 4** — as imagens de ECG servirão ao módulo de visão computacional
- **Fase 5** — o corpus textual será a base do assistente cardiológico virtual e dos módulos de NLP
As limitações identificadas nesta fase — viés de seleção, heterogeneidade de origem e ausência de contexto clínico nas imagens — devem ser retomadas na validação dos modelos das fases seguintes.
