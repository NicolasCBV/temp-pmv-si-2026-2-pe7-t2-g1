# Introdução  

O diabetes mellitus é uma condição crônica de instalação silenciosa, cujo diagnóstico frequentemente ocorre apenas quando já existem complicações estabelecidas. Uma parcela expressiva das pessoas que convivem com a doença desconhece o próprio diagnóstico e permanece sem acompanhamento. O rastreamento universal por exame laboratorial resolveria o problema, mas é inviável por custo e capacidade instalada. A alternativa praticada é a priorização: identificar quem tem maior probabilidade de apresentar a condição e direcionar a esses o recurso escasso do exame. Essa priorização costuma usar critérios fixos e isolados, como idade ou índice de massa corporal, que são transparentes mas pouco sensíveis a combinações de fatores e incapazes de ordenar indivíduos por grau de risco. 

É nesse ponto que o aprendizado de máquina se torna pertinente. A estatística descritiva responde a perguntas sobre o que já ocorreu na amostra, de forma agregada e retrospectiva. Modelos supervisionados produzem estimativa individual aplicável a novos casos, combinando mais de vinte indicadores com interações não lineares em um escore ordenável e acionável. 

Este trabalho investiga essa possibilidade com o Diabetes Health Indicators Dataset, derivado do Behavioral Risk Factor Surveillance System (BRFSS), inquérito telefônico do CDC. A versão utilizada reúne 253.680 respostas descritas por 21 indicadores de saúde, comportamento e condição socioeconômica. O objetivo é construir e comparar modelos de classificação que estimem a probabilidade individual da condição a partir de informações de baixo custo de coleta, avaliando desempenho preditivo e limitações metodológicas e éticas. 

## Problema

A diabetes é uma doença crônica comum e, quando o diagnóstico chega tarde, aumenta o risco de complicações no coração, nos rins e no sistema nervoso. Mesmo assim, ainda é difícil saber com antecedência quem tem mais chance de desenvolver a doença. Na triagem, os métodos mais usados olham fatores como idade, peso, sedentarismo, tabagismo e consumo de álcool, quase sempre de forma isolada (BALAJI, K. et al., 2023). Raramente esses dados são cruzados com outros indicadores clínicos e de estilo de vida que já existem em pesquisas populacionais.

Na prática, muita gente só é identificada quando a doença já está instalada. Serviços de saúde acumulam registros com vários atributos, mas ainda não é comum ter um modelo que, diante de um novo conjunto de informações, estime o risco de diabetes de forma reproduzível.

Este projeto é acadêmico e será desenvolvido na disciplina de Pesquisa e Experimentação em Sistemas de Informação, da PUC Minas. Não há empresa parceira. A base escolhida é o Diabetes Health Indicators Dataset (TEBOUL, 2021), montado a partir da pesquisa BRFSS 2015 do CDC (CDC, 2015). São dados clínicos reais de adultos, o que permite treinar e comparar modelos de ciência de dados para prever a presença ou o aparecimento da doença em registros novos.

## Questão de pesquisa

Quais modelos de aprendizado de máquina, treinados com os indicadores clínicos e de estilo de vida do Diabetes Health Indicators Dataset (BRFSS 2015), preveem melhor o risco ou a presença de diabetes em registros que não foram usados no treino?

A resposta vem da comparação entre os modelos ao final da experimentação, com métricas como acurácia, precisão, recall, F1-score e AUC.

## Objetivos preliminares. 

### Objetivo geral

Desenvolver e comparar modelos de aprendizado supervisionado para estimar a probabilidade de diabetes ou pré-diabetes em adultos, a partir dos indicadores autorrelatados do BRFSS 2015, avaliando seu potencial como instrumento de priorização de rastreamento e discutindo suas limitações. 

### Objetivos específicos 

1. Caracterizar o conjunto quanto a dimensões, tipos, distribuição do rótulo, duplicações e valores atípicos, definindo a unidade de análise.
2. Realizar análise exploratória das 21 variáveis, identificando distribuições e associações com o rótulo.
3. Construir pipeline de preparação reprodutível, com toda transformação ajustada apenas após a separação treino/teste.
4. Avaliar equidade, comparando taxas de erro entre estratos de sexo, renda e escolaridade.
5. Implantar solução que retorne probabilidade, faixa de prioridade e fatores contribuintes, com aviso explícito de caráter não diagnóstico

## Justificativa

Nesta seção, apresente a importância e a motivação para trabalhar com o conjunto de dados escolhido. Explique por que esse dataset é relevante e como ele se conecta ao problema identificado anteriormente.

Indique:
* Razões para a escolha dos objetivos específicos – Justifique por que decidiu aprofundar sua investigação nessas metas, relacionando-as ao potencial de solução ou melhoria para o problema.
* Relevância do estudo do problema – Mostre a importância de compreender e tratar a questão apresentada, tanto no contexto acadêmico quanto no profissional.
* Impacto social, econômico ou ambiental – Descreva como o problema afeta a sociedade ou um setor específico, buscando sempre quantificar o impacto por meio de dados reais.

**Importante:**
* Apresente números, estatísticas e informações concretas, citando as fontes (relatórios, artigos científicos, portais oficiais etc.).
* Mantenha a objetividade e a clareza, evitando argumentos genéricos.
* Construa um texto coeso que conecte o problema, os objetivos e a relevância do trabalho.

> **Links Úteis**:
> - [Como montar a justificativa](https://guiadamonografia.com.br/como-montar-justificativa-do-tcc/)

## Público-Alvo

O projeto tem como público-alvo principal profissionais e pesquisadores que atuam na análise de fatores associados ao diabetes e na avaliação de estratégias de prevenção, com destaque para equipes de saúde pública, epidemiologia, atenção primária, gestão de programas de prevenção e Ciência de Dados aplicada à saúde. Nesse contexto, os modelos desenvolvidos podem ser utilizados para investigar quais combinações de indicadores de saúde, hábitos de vida e condições socioeconômicas apresentam maior capacidade de discriminar indivíduos sem diabetes, com pré-diabetes ou com diabetes na população representada pelo conjunto de dados.

Também integram o público de interesse analistas e gestores responsáveis pelo planejamento de ações de rastreamento e prevenção de doenças crônicas. Para esses usuários, a utilidade do projeto está na comparação objetiva de modelos de aprendizado de máquina e na identificação dos atributos mais relevantes para a classificação do estado de diabetes.

Adiante, a população adulta potencialmente beneficiada por estratégias de rastreamento constitui um público indireto. No entanto, cabe ressaltar que o resultado do projeto não deve ser interpretado como diagnóstico clínico autônomo. O conjunto de dados deriva de um inquérito populacional telefônico realizado nos Estados Unidos e contém, em grande parte, informações autorrelatadas. Por essa razão, os modelos produzidos são adequados para estudar classificação e estratificação de risco com base em indicadores observados, mas não permitem, por si só, afirmar que ocorrerá o aparecimento futuro da doença em um indivíduo específico.


## Estado da arte

A diabetes é um distúrbio metabólico caracterizado por níveis elevados de glicose no sangue, tornou-se uma preocupação significativa de saúde pública em todo o mundo. Ele está associado a várias complicações, incluindo doenças cardiovasculares, doenças renais e neuropatia. A detecção precoce e a avaliação precisa do risco de diabetes desempenham um papel fundamental na prevenção ou no retardamento de seu início, bem como no manejo eficaz da doença (BALAJI, K et al., 2023). Os métodos tradicionais de avaliação de risco baseiam-se em fatores como idade, peso, estilo de vida ativo ou sedentário, o uso de cigarro, ingestão de bebidas alcoólicas e outros inúmeros fatores.

Na literatura são descritos inúmeros artigos científicos sobre o tema. Para esse projeto destacamos quatro estudos de forma aplicada e um estudo sobre avaliação de métodos de balanceamento, problema recorrente para dados reais.

**DANIEL et al. (2024)** estudaram um dataset proveniente da Welsh Primary Care Electronic Health Records (EHRS) combinado com o Brecon Dataset, ambos sendo registros de crianças diagnosticadas com diabetes do tipo 1, totalizando 34.089.103 linhas de registro e 26 colunas. O propósito do trabalho foi investigar se um algoritmo de ML poderia prever o aparecimento da doença em uma criança e também o tempo previsto de diagnóstico. Para isso foi desenvolvido um superaprendiz (SuperLearner). Para a análise estatística foram construídos 34 algoritmos e usadas as técnicas de Univariate Correlation Screening e Random Forest Screening para selecionar as variáveis mais importantes. Para a avaliação da performance de cada algoritmo foi usada a técnica AUROC (Área sob a Curva ROC) para selecionar os melhores algoritmos para fazer parte do SuperLearner. Finalmente, para comparar o modelo final com aproximações, como por exemplo regressão linear logística, eles desenvolveram um próprio algoritmo de regressão logística.

Já **MAO T (2026)** criou um modelo baseado no dataset do National Health and Nutrition Examination Survey (NHANES) integrado com características demográficas, indicadores clínicos e fatores de estilo de vida. Todos os arquivos do dataset continham 5.000 linhas depois de tratados e 9 parâmetros, com exceção do alvo. Para o pré-processamento, todas as variáveis contínuas foram processadas numericamente, sendo que valores nulos foram preenchidos com média, enquanto as não numéricas foram codificadas por tipo. Para a predição da diabetes foi construído um modelo Multi Layer Perceptron (MLP) e um conjunto de características de entrada de alta qualidade por meio de limpeza de características, padronização e análise de componentes principais (PCA), usando a saída do modelo para previsão de estratificação de risco. Para o treinamento, os dados foram padronizados com Z-score e posteriormente foram usadas as métricas de performance Exatidão, Precisão, Recall, F1 Score e AUC. Em geral, o estudo demonstra uma estratégia para a avaliação precoce do risco de diabetes, ao mesmo tempo que fornece informações sobre a aplicação de técnicas de inteligência artificial na pesquisa em saúde pública.

**EDLITZ Y. e SEGAL E. (2022)** analisaram 44.709 dados com 798 parâmetros do UK Biobank referentes a participantes não diabéticos com idade entre 40 e 69 anos para prever o risco de diabetes tipo 2. Os dados foram analisados usando árvores de decisão com gradient boosting, análise de sobrevivência e regressão logística. Foram desenvolvidos dois modelos simplificados com formulário de scorecard (cartão de pontuação). Para o modelo não laboratorial foram definidos os parâmetros: sexo, idade, peso, altura, circunferência de cintura e quadril, relação cintura-quadril e IMC. Já o modelo laboratorial acrescenta quatro exames de sangue comuns (HDL, gama-glutamil transferase, hemoglobina glicada e triglicerídeos). Como resultado, o modelo não laboratorial atingiu uma área sob a curva ROC (AUC) de 0,81, enquanto o modelo laboratorial alcançou AUC de 0,87 — ambos superando os escores de risco tradicionalmente usados na literatura (FINDRISC e GDRS) — mantendo desempenho satisfatório (AUC de 0,75) na validação externa.

**PALA & ABUSHAHLA (2024)** estudaram um dataset relacionado a diabetes, aplicando seis técnicas de pré-processamento para lidar com o desbalanceamento de classes. O propósito do trabalho foi investigar se técnicas de tratamento de dados poderiam melhorar a performance de classificação de diabetes em bases desbalanceadas. Para isso foram construídos dez algoritmos de classificação: Regressão Logística, Árvore de Decisão, Random Forest, Gradient Boosting, SVM, KNN, Naive Bayes, XGBoost, LightGBM e CatBoost. Para o tratamento dos dados foram usadas as técnicas de padronização, normalização, padronização combinada com validação cruzada K-Fold, e duas variações com a técnica de sobreamostragem SMOTE. Para a avaliação da performance de cada combinação de modelo e técnica de pré-processamento foram usadas as métricas de acurácia, precisão, recall e F1-score. Finalmente, para comparar os modelos entre si, o CatBoost se destacou como o de melhor desempenho, atingindo 95,18% de acurácia, 91,10% de precisão, 95,52% de recall e 93,26% de F1-score.

Por fim, **GOURISARIA M. et al. (2021)** estudaram dois datasets relacionados a diabetes, o primeiro do Hospital de Frankfurt, na Alemanha, e o segundo do repositório da Universidade da Califórnia, Irvine (UCI). O propósito do trabalho foi detectar diabetes mellitus através de técnicas de aprendizado de máquina, aprendizado profundo e redução de dimensionalidade dos dados. Para isso foram construídos os algoritmos de classificação Support Vector Machines, Naïve Bayes e Random Forest, aplicados para diferenciar pacientes diabéticos de não diabéticos. Para a análise estatística, o processo foi repetido em versões dos datasets com dimensionalidade reduzida, usando as técnicas de Linear Discriminant Analysis (LDA) e Principal Component Analysis (PCA). Para a avaliação da performance de cada modelo foi realizado ajuste de hiperparâmetros seguido de estudo comparativo entre os resultados. Finalmente, para comparar os modelos entre si, o K-Nearest Neighbours se destacou como o de melhor desempenho no dataset de Frankfurt, atingindo 98,2% de acurácia, enquanto o Random Forest se destacou no dataset da UCI, atingindo 99,2% de acurácia.

Um resumo dos estudos está descrito na tabela abaixo:

<table>
<tr>
<th>Autores</th>
<th>Objetivo do estudo</th>
<th>Métodos de Data Science</th>
<th colspan="2">Linhas e colunas do dataset</th>
</tr>
<tr>
<td>DANIEL et al. (2024)</td>
<td>Previsão de diabetes tipo 1 e tempo até o diagnóstico.</td>
<td>SuperLearner; 34 algoritmos; Univariate Correlation Screening; Random Forest Screening; AUROC; regressão logística.</td>
<td>34.089.103</td>
<td>26</td>
</tr>
<tr>
<td>MAO T. (2026)</td>
<td>Predição/estratificação de risco de diabetes.</td>
<td>MLP; limpeza de dados; padronização Z-score; PCA; codificação de variáveis; métricas de classificação.</td>
<td>5.000</td>
<td>10 (9 atributos + alvo)</td>
</tr>
<tr>
<td>EDLITZ Y. e SEGAL E. (2022)</td>
<td>Previsão de risco de diabetes tipo 2.</td>
<td>Gradient Boosting Trees; análise de sobrevivência; regressão logística; scorecards.</td>
<td>44.709</td>
<td>798</td>
</tr>
<tr>
<td>ABUSHAHLA K e PALA M. (2024)</td>
<td>Avaliação de modelos para classificação de diabetes em base desbalanceada.</td>
<td>Regressão Logística, Árvores de Decisão, Random Forest, Gradient Boosting, SVM, KNN, Naive Bayes, XGBoost, LightGBM, CatBoost; SMOTE</td>
<td>N/A</td>
<td>N/A</td>
</tr>
<tr>
<td>GOURISARIA M. et al. (2021)</td>
<td>Diabetes mellitus (não diferencia tipo 1 e tipo 2).</td>
<td>SVM, Naive Bayes, Random Forest, KNN; redução de dimensionalidade com LDA e PCA.</td>
<td colspan="2">Dois datasets: Frankfurt Hospital com 2.000 registros e 9 atributos; UCI com aproximadamente 520 registros e 17 atributos (16 características + classe).</td>
</tr>
</table>



# Descrição do _dataset_ selecionado

## Identificação, origem e finalidade

O conjunto selecionado é o **Diabetes Health Indicators Dataset**, publicado por Alex Teboul no Kaggle em 2021. A base é uma versão tratada e consolidada dos dados do **Behavioral Risk Factor Surveillance System (BRFSS) de 2015**, sistema de vigilância mantido pelos *Centers for Disease Control and Prevention* (CDC). Teboul não realizou a coleta primária, sua contribuição consistiu na seleção, recodificação e organização de variáveis do BRFSS em arquivos voltados a tarefas de classificação relacionadas ao diabetes (TEBOUL, 2021).

A versão tratada está disponível em <https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset>. A documentação e os arquivos da fonte primária do ciclo de 2015 estão disponíveis no portal do CDC em <https://www.cdc.gov/brfss/annual_data/annual_2015.html>.

A fonte primária, o BRFSS, é um inquérito transversal de saúde realizado por telefone fixo e celular com adultos não institucionalizados de 18 anos ou mais. Sua finalidade original é produzir informações populacionais sobre comportamentos de risco à saúde, condições crônicas, acesso aos serviços de saúde e utilização de serviços preventivos. Portanto, a coleta original não foi criada especificamente para treinamento de modelos de aprendizado de máquina nem exclusivamente para estudo do diabetes. No ciclo de 2015, o conjunto agregado do CDC contém **441.456 registros e 330 variáveis** e abrange os 50 estados dos Estados Unidos, o Distrito de Columbia, Guam e Porto Rico.

A versão disponibilizada no Kaggle reduz essa base a **21 variáveis preditoras** relacionadas a condições de saúde, comportamento e características sociodemográficas, além da variável alvo. Cada registro representa um respondente do levantamento após o processo de seleção e tratamento realizado pelo autor. A unidade de observação, portanto, é uma pessoa participante do BRFSS, e não uma consulta, internação, exame laboratorial ou série temporal de um paciente.

A página do Kaggle distribui a base em três arquivos CSV, todos derivados do mesmo ciclo de coleta:

| Arquivo | Registros | Colunas | Variável alvo | Característica da distribuição |
| --- | ---: | ---: | --- | --- |
| `diabetes_012_health_indicators_BRFSS2015.csv` | 253.680 | 22 | `Diabetes_012` | Três classes e distribuição desbalanceada |
| `diabetes_binary_health_indicators_BRFSS2015.csv` | 253.680 | 22 | `Diabetes_binary` | Duas classes e distribuição desbalanceada |
| `diabetes_binary_5050split_health_indicators_BRFSS2015.csv` | 70.692 | 22 | `Diabetes_binary` | Duas classes, com divisão aproximada de 50% por classe |

No arquivo multiclasse, `Diabetes_012` assume os valores **0** para ausência de diabetes ou diabetes apenas durante a gestação, **1** para pré-diabetes e **2** para diabetes. Nos arquivos binários, `Diabetes_binary` assume **0** para ausência de diabetes e **1** para pré-diabetes ou diabetes, conforme a documentação publicada pelo autor. Em todos os arquivos há 21 atributos de entrada e uma coluna alvo (TEBOUL, 2021).

Os arquivos derivados são fornecidos em **CSV**, formato  de texto amplamente compatível com ferramentas de análise de dados. Embora os valores estejam armazenados numericamente, a maior parte das variáveis possui natureza binária, categórica ou ordinal. O BRFSS original, por sua vez, foi disponibilizado pelo CDC em formatos ASCII e SAS Transport; o arquivo SAS Transport de 2015 contém as 330 variáveis da base anual (CDC, 2015).

Quanto às condições de uso, a página do **Diabetes Health Indicators Dataset** no Kaggle informa licença **CC0: Public Domain** e frequência esperada de atualização **“Never”**. A versão tratada deve, portanto, ser considerada uma fotografia fixa do ciclo de 2015. O BRFSS original continua sendo realizado anualmente pelo CDC, mas novas edições do levantamento não atualizam automaticamente os três arquivos publicados por Teboul.

## Atributos

A tabela a seguir descreve as variáveis presentes nos arquivos tratados. As codificações foram mantidas de acordo com a documentação do conjunto e com o codebook do BRFSS 2015. Variáveis binárias utilizam 0 e 1; variáveis ordinais representam faixas ou escalas, e não medidas contínuas em sua unidade original.

| Atributo | Papel e tipo | Descrição | Unidade ou codificação |
| --- | --- | --- | --- |
| `Diabetes_012` | Alvo, categórico ordinal | Estado de diabetes no arquivo com três classes. | 0 = sem diabetes ou apenas gestacional; 1 = pré-diabetes; 2 = diabetes. |
| `Diabetes_binary` | Alvo, binário | Estado de diabetes nos arquivos binários. | 0 = sem diabetes; 1 = pré-diabetes ou diabetes, conforme documentação do conjunto. |
| `HighBP` | Preditor, binário | Indica histórico de pressão arterial elevada. | 0 = não; 1 = sim. |
| `HighChol` | Preditor, binário | Indica colesterol elevado. | 0 = não; 1 = sim. |
| `CholCheck` | Preditor, binário | Indica realização de exame de colesterol nos cinco anos anteriores. | 0 = não; 1 = sim. |
| `BMI` | Preditor, numérico | Índice de Massa Corporal. | kg/m², representado numericamente no arquivo. |
| `Smoker` | Preditor, binário | Indica se o participante fumou pelo menos 100 cigarros ao longo da vida. | 0 = não; 1 = sim. |
| `Stroke` | Preditor, binário | Indica relato prévio de acidente vascular cerebral. | 0 = não; 1 = sim. |
| `HeartDiseaseorAttack` | Preditor, binário | Indica relato de doença coronariana ou infarto do miocárdio. | 0 = não; 1 = sim. |
| `PhysActivity` | Preditor, binário | Indica realização de atividade física nos 30 dias anteriores, desconsiderando atividade relacionada ao trabalho. | 0 = não; 1 = sim. |
| `Fruits` | Preditor, binário | Indica consumo de fruta uma ou mais vezes ao dia. | 0 = não; 1 = sim. |
| `Veggies` | Preditor, binário | Indica consumo de vegetais uma ou mais vezes ao dia. | 0 = não; 1 = sim. |
| `HvyAlcoholConsump` | Preditor, binário | Identifica consumo elevado de álcool segundo o critério adotado no conjunto. | 0 = não; 1 = sim. |
| `AnyHealthcare` | Preditor, binário | Indica existência de alguma cobertura de assistência à saúde, como seguro ou plano pré-pago. | 0 = não; 1 = sim. |
| `NoDocbcCost` | Preditor, binário | Indica se, nos 12 meses anteriores, o participante precisou consultar um médico e não conseguiu por causa do custo. | 0 = não; 1 = sim. |
| `GenHlth` | Preditor, ordinal | Autoavaliação do estado geral de saúde. | 1 = excelente; 2 = muito boa; 3 = boa; 4 = regular; 5 = ruim. |
| `MentHlth` | Preditor, inteiro | Número de dias, nos 30 dias anteriores, em que a saúde mental foi considerada ruim. | 0 a 30 dias. |
| `PhysHlth` | Preditor, inteiro | Número de dias, nos 30 dias anteriores, em que a saúde física foi considerada ruim. | 0 a 30 dias. |
| `DiffWalk` | Preditor, binário | Indica dificuldade importante para caminhar ou subir escadas. | 0 = não; 1 = sim. |
| `Sex` | Preditor, binário | Sexo registrado no levantamento e recodificado no conjunto tratado. | 0 = feminino; 1 = masculino. |
| `Age` | Preditor, ordinal | Faixa etária em 13 categorias. | 1 = 18–24 anos; ...; 13 = 80 anos ou mais. |
| `Education` | Preditor, ordinal | Escolaridade em seis categorias. | 1 = sem escolarização ou apenas jardim de infância; ...; 6 = graduação de quatro anos ou mais. |
| `Income` | Preditor, ordinal | Faixa de renda domiciliar anual em oito categorias. | 1 = menos de US$ 10.000; ...; 8 = US$ 75.000 ou mais. |

## Qualidade dos dados, limitações e restrições de uso

A versão de Teboul é apresentada como uma base previamente limpa e consolidada, o que reduz a quantidade de variáveis e elimina parte da complexidade do arquivo anual do BRFSS. A redução de **441.456 registros e 330 variáveis** da fonte primária para **253.680 registros e 21 preditores** na principal versão tratada mostra, entretanto, que houve seleção de observações e atributos. Esse processo deve ser considerado na interpretação dos resultados e a amostra efetivamente utilizada pelo projeto é um subconjunto da pesquisa original e não preserva todas as informações necessárias para reconstruir o desenho amostral do BRFSS.

O primeiro problema evidente é o **desbalanceamento das classes** nos arquivos de 253.680 registros, explicitamente informado pelo autor. Esse aspecto pode produzir modelos com boa acurácia global e desempenho insuficiente para as classes menos frequentes, sobretudo pré-diabetes. Por essa razão, a avaliação não deve se limitar à acurácia; métricas como precisão, revocação (*recall*), F1-score, matriz de confusão e medidas baseadas em ROC ou precisão-revocação devem ser consideradas conforme a estratégia de modelagem. O arquivo com 70.692 registros fornece uma alternativa balanceada, porém altera artificialmente a prevalência observada e, por isso, não deve ser tratado como representação da prevalência populacional de diabetes.

Outro limite decorre da própria natureza do BRFSS. Parte relevante das informações é **autorrelatada** em entrevista telefônica, incluindo diagnóstico informado pelo participante, hábitos, estado de saúde e acesso a serviços. Assim, a base está sujeita a viés de memória, erro de classificação, não resposta e diferenças de interpretação das perguntas. O levantamento busca representar adultos não institucionalizados alcançáveis pelos métodos de amostragem empregados; embora o CDC utilize ponderação por *raking* para reduzir distorções amostrais, a versão tratada no Kaggle não contém todas as variáveis de ponderação e desenho amostral necessárias para reproduzir estimativas populacionais oficiais do BRFSS (CDC, 2015; CDC, 2016a).

Há ainda uma **restrição temporal e geográfica**. Os dados correspondem ao ciclo de 2015 e refletem a população adulta dos Estados Unidos e territórios incluídos naquele levantamento. Relações entre renda, acesso à saúde, hábitos e diagnóstico podem variar ao longo do tempo e entre países. Consequentemente, o desempenho obtido nessa base não pode ser assumido como válido para a população brasileira ou para dados atuais sem validação externa em amostra compatível com o contexto de aplicação.

O conjunto tratado também não preserva identificador individual, data da entrevista ou localização do respondente entre os 21 preditores. Isso limita análises espaciais, temporais e de acompanhamento. Caso a análise exploratória identifique linhas integralmente iguais, elas não devem ser removidas automaticamente como duplicatas indevidas.

Por fim, o objetivo do projeto deve considerar que o conjunto é **transversal**, isto é, os preditores e o estado de diabetes são observados no mesmo levantamento. A base permite treinar modelos para classificar o estado de diabetes ou estimar risco associado aos indicadores disponíveis, mas não registra a evolução de uma mesma pessoa ao longo do tempo. Portanto, ela não sustenta diretamente uma tarefa de previsão temporal do “aparecimento futuro” da doença. Para esse tipo de conclusão seria necessária uma base longitudinal com indivíduos inicialmente sem diabetes e acompanhamento posterior de novos diagnósticos. No escopo atual, a formulação metodologicamente compatível é a classificação ou estratificação do risco de diabetes a partir dos indicadores de saúde disponíveis.

# Canvas analítico

<img width="2000" height="1414" alt="CanvasAnalítico (imagem)" src="https://github.com/user-attachments/assets/356c7a9f-c15c-447a-bc75-b38640c35c2c" />


# Vídeo de apresentação da Etapa 01

Nesta etapa, o grupo deverá produzir um vídeo de 5 a 8 minutos apresentando o trabalho realizado, no qual cada integrante deve dizer seu nome e apresentar uma parte do conteúdo desenvolvido, garantindo que todos participem ativamente da gravação. A ausência de participação de qualquer membro resultará em penalização na nota final desta etapa. Recomenda-se que o grupo elabore previamente um roteiro para organizar a ordem das falas, distribuir o tempo de forma equilibrada e assegurar que todos os tópicos relevantes sejam apresentados de maneira clara e objetiva.

# Referências

Inclua todas as referências (livros, artigos, sites, etc) utilizados no desenvolvimento do trabalho utilizando o padrão ABNT.

CENTERS FOR DISEASE CONTROL AND PREVENTION (CDC). 2015 BRFSS Survey Data and Documentation. Atlanta: CDC, 2015. Disponível em: https://www.cdc.gov/brfss/annual_data/annual_2015.html. Acesso em: 28 ago. 2026. 

TEBOUL, Alex. Diabetes Health Indicators Dataset. Kaggle, 2021. Disponível em: https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset. Acesso em: 28 ago. 2026. 

BALAJI, K. Vivek; SUGUMAR, R. Harnessing the power of machine learning for diabetes risk assessment: A promising approach. In: 2023 International Conference on Data Science, Agents & Artificial Intelligence (ICDSAAI). IEEE, 2023. p. 1-6. 

DANIEL, Rhian et al. Predicting type 1 diabetes in children using electronic health records in primary care in the UK: development and validation of a machine-learning algorithm. The Lancet Digital Health, v. 6, n. 6, p. e386-e395, 2024. 

MAO, Tiancheng. Diabetes Risk Prediction Based on Clinical and Lifestyle Data. In: ITM Web of Conferences. EDP Sciences, 2026. p. 01015. 

EDLITZ, Yochai; SEGAL, Eran. Prediction of type 2 diabetes mellitus onset using logistic regression-based scorecards. Elife, v. 11, p. e71862, 2022. 

ABUSHAHLA, Khalid Hani; PALA, Muhammed Ali. Optimizing diabetes prediction: addressing data imbalance with machine learning algorithms. ADBA Computer Science, v. 1, n. 1, p. 26-35, 2024. 

GOURISARIA, Mahendra Kumar et al. Data science appositeness in diabetes mellitus diagnosis for healthcare systems of developing nations. IET Communications, v. 16, n. 5, p. 532-547, 2022.

