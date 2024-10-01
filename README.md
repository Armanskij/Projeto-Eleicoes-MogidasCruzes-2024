# Projeto - Análise dos Candidatos Municipais de Mogi das Cruzes - 2024

Projeto de orquestração de pipelines de integrassão de bot do Telegram com a cloud AWS.

Este repositório contém um projeto de orquestração de pipeline de dados entre o Telegram e a AWS, com foco na conversão de dados transacionais em dados analíticos. Utilizamos serviços como AWS S3, AWS Lambda, AWS API Gateway, e AWS EventBridge para processar e analisar dados captados por bots do Telegram.
## Objetivo do Projeto

O objetivo deste projeto é transformar dados transacionais gerados por interações com um chatbot no Telegram em dados analíticos. Esse processo possibilita extrair insights valiosos, melhorar o desempenho do chatbot e compreender melhor o comportamento dos usuários. A ingestão, transformação e armazenamento dos dados são realizados de forma automatizada e escalável utilizando os serviços da AWS.

### Contexto

#### Sobre a cidade de Mogi das Cruzes

Mogi das Cruzes é um município localizado na região metropolitana de São Paulo, no Brasil, a cerca de 50 km da capital paulista. Fundada em 1560, a cidade é conhecida por sua história rica e cultura diversificada, além de sua economia dinâmica, que abrange desde a agricultura, especialmente o cultivo de hortaliças e flores, até setores industriais e comerciais.

Com uma população de aproximadamente 450 mil habitantes, Mogi das Cruzes destaca-se por sua infraestrutura urbana, boas opções de lazer e educação, além de importantes áreas de preservação ambiental, como a Serra do Itapeti e o Parque Natural Municipal Francisco Affonso de Mello. A cidade é um polo de desenvolvimento no Alto Tietê e possui uma crescente influência no cenário econômico e político da região.

Alguns dados interessantes sobre a cidade:
- **Fundação:** 1° de setembro de 1611 (413 anos)
- **Área Total:** 712,541 km² (12° do Estado)
- **Distância da capital:** 60km
- **População Total:** 449.955 hab.
- **IDH:** 0,783 (*alto*, 60° BR)
- **PIP:** R$ 15.386.499,31
- **Prefeito:** Caio Cesar Machado da Cunha (PODE, 2021-2024)
# Coleta de dados

A base de dados utilizada neste projeto foi extraída do site do Tribunal Superior Eleitoral (TSE) em setembro de 2024. Ela contém informações detalhadas de todos os candidatos que concorrem às Eleições Municipais de 2024 em todo o território nacional. Essa base inclui dados como nome, partido, gênero, idade, cor/raça, escolaridade e ocupação dos candidatos. Embora o TSE também disponibilize outras informações, como bens dos candidatos, coligações e motivos de cassação, este projeto focará exclusivamente nos dados demográficos e gerais dos candidatos, conforme definido pelo escopo. A análise de outras variáveis poderá ser incluída em projetos futuros, proporcionando uma visão ainda mais aprofundada do perfil dos candidatos.

Para caracterizar adequadamente os candidatos e identificar padrões significativos entre eles, selecionei dentre um conjunto de mais de 40 variáveis relevantes que possibilitam uma análise aprofundada. Essas variáveis foram escolhidas por sua capacidade de oferecer uma visão abrangente sobre o perfil dos candidatos, permitindo a separação de conjuntos relacionados e a geração de insights valiosos. As variáveis selecionadas incluem:
- DS_CARGO : Descriçao do cargo do candidato (vereador, vice-prefeito e prefeito)
- NM_CANDIDATO: Nome completo do candidato
- NR_PARTIDO: Número do partido do candidato
- SG_PARTIDO: Sigla do partido do candidato
- SG_UF_NASCIMENTO: Estado de nascimento do candidato
- CD_GENERO: Código do gênero do candidato (2:Masculino, 4:Feminino)
- CD_GRAU_INSTRUCAO: Código do grau de instrução (escolaridade) do candidato, podendo assumir os seguintes valores:
  - 1: Analfabeto;
  - 2: Lê e escreve;
  - 3: Ensino fundamental incompleto;
  - 4: Ensino fundamental completo;
  - 5: Ensino médio incompleto;
  - 6: Ensino médio completo;
  - 7: Superior incompleto; e
  - 8: Superior completo.
- CD_ESTADO_CIVIL: Código do estado civil do candidato.
- DT_NASCIMENTO: Data de nascimento do candidato.
- CD_COR_RACA: Código da cor/raça do candidato, podendo assumir os seguintes valores:
  - 01: Branca;
  - 02: Preta;
  - 03: Parda;
  - 04: Amarela;
  - 05: Indígena; e
  - 06: Não Informado.
- CD_OCUPACAO: Código da ocupação do candidato.
- DS_OCUPACAO: Descrição da ocupação do candidato.

### Análise Descritiva

#### Idade
![Pirâmide Etária](https://raw.githubusercontent.com/Armanskij/Projeto-Eleicoes-MogidasCruzes-2024/refs/heads/main/assets/estatistica%20idade.png)

As medidas descritivas da idade dos candidatos revelam uma visão geral interessante sobre o perfil etário.

Com 397 **candidatos no total**, a **idade média** é de 48,99 anos, o que indica que a maioria dos candidatos está na faixa da meia-idade. O **desvio padrão** de 11,18 anos mostra uma dispersão moderada em torno da média, indicando uma variação considerável nas idades. O candidato **mais jovem** tem 18 anos e o **mais velho**, 81 anos.

Os quartis mostram que 25% dos candidatos têm até 42 anos, 50% até 48 anos (mediana), e 75% até 56 anos, o que reflete que a maior parte dos candidatos está na faixa entre os 40 e 60 anos

#### Composição Racial
![Composição Racial](https://raw.githubusercontent.com/Armanskij/Projeto-Eleicoes-MogidasCruzes-2024/refs/heads/main/assets/racial.png)

Com base na distribuição de cor/raça, é possível identificar padrões entre a representatividade dos candidatos e a população. Observamos que a cor/raça branca apresenta a maior representatividade, com 54,9% da população e 52,6% dos candidatos. Na segunda posição, estão as pessoas que se autodeclaram pardas, correspondendo a 32,8% da população e 33,3% dos candidatos. Já as pessoas que se autodeclaram pretas, que representam 8,5% da população, compõem 11,8% dos candidatos. Por fim, as pessoas de cor/raça amarela representam 3,7% da população e 2,3% dos candidatos.

De modo geral, a representatividade dos candidatos reflete de maneira próxima a composição racial da população. No entanto, é interessante notar que as pessoas brancas e amarelas estão ligeiramente sub-representadas entre os candidatos em relação à sua participação na população total, enquanto as pessoas pardas e pretas estão super-representadas. Esse padrão pode sugerir um maior engajamento político entre os grupos de cor/raça parda e preta, ou possivelmente indicar uma demanda maior por políticas públicas voltadas a esses grupos.

Como sugestão para aprimorar essa análise, seria interessante observar o histórico temporal das eleições, a fim de identificar possíveis mudanças. Além disso, seria relevante verificar se houve a implementação de políticas públicas ou programas voltados para esses grupos que possam ter influenciado essa representatividade.

#### Graduação
![Escolaridade](https://raw.githubusercontent.com/Armanskij/Projeto-Eleicoes-MogidasCruzes-2024/refs/heads/main/assets/escolaridade.png)

Observamos que o ensino superior é predominante em todos os grupos, o que sugere que a maioria dos candidatos possui uma boa preparação acadêmica para assumir um cargo público. Em relação aos gêneros, o feminino apresenta uma taxa um pouco maior de ensino superior completo em comparação ao masculino, o que pode indicar que mulheres talvez enfrentem diferentes **exigências educacionais** para progredir na política.

Quando analisamos por cor/raça, alguns insights significativos podem ser observados. O mais marcante é o percentual de 77% de candidatos da cor/raça amarela com ensino superior completo, embora seja importante lembrar que eles representam apenas 3% dos candidatos. Outro dado relevante é o percentual de ensino superior completo entre as pessoas das cores/raças preta e parda, 29% e 40%, respectivamente, que são os menores entre os diversos grupos. No entanto, esses mesmos grupos são os que mais possuem ensino médio completo, o que pode sugerir a existência de **barreiras no acesso à educação superior** para essas populações.

#### Perfil dos Partidos
![Perfil Partidos](https://raw.githubusercontent.com/Armanskij/Projeto-Eleicoes-MogidasCruzes-2024/refs/heads/main/assets/perfil%20partido.png)

***Gênero***

 - Representação Masculina: O **PSOL se destaca como o partido com a maior proporção de homens, apresentando 70%** de seus membros masculinos. Essa alta representatividade masculina sugere uma predominância no apoio e na liderança masculina dentro da estrutura do partido.

 - Representação Feminina: **O PMB se sobressai com 60% de mulheres**, indicando um compromisso significativo com a inclusão feminina. Isso pode refletir políticas voltadas para a promoção de igualdade de gênero e empoderamento feminino dentro do partido.

***Cor/Raça***

- Raça Branca: **O partido UNIÃO possui a maior representatividade de indivíduos brancos, com 76% de seus membros**. Essa predominância indica uma possível falta de diversidade racial. Por outro lado, partidos como SOLIDARIEDADE e PC do B não têm representantes brancos, o que pode refletir uma abordagem inclusiva ou, alternativamente, uma falta de foco na representação racial.

- Raça Preta: **O PC do B lidera com 50% de representação preta**. Essa alta porcentagem sugere uma significativa inclusão de pessoas negras na política deste partido. Contudo, há uma preocupação, pois partidos como PL, REPUBLICANOS, PV, e MOBILIZA não apresentam nenhum representante da cor/race preta, evidenciando uma falta de diversidade racial.

- Raça Parda: **O partido SOLIDARIEDADE se destaca com 75%** de pardos, o que demonstra uma boa inclusão racial. No entanto, o UNIÃO apresenta apenas 16% de pardos, indicando uma disparidade significativa em comparação a outros partidos.

- Raça Amarela: **O partido PL é o único a ter um percentual de 12% de representantes amarelos**. A ausência de representantes amarelos em partidos como PSD, SOLIDARIEDADE, REPUBLICANOS, PRD, entre outros, sugere uma necessidade de maior inclusão racial.

***Idade Média***

- Idade Média dos Membros: **O PDT tem a maior média de idade entre seus membros, com 54 anos**. Isso pode indicar uma base de apoio mais envelhecida, o que pode influenciar as políticas e a abordagem do partido em relação a questões sociais. Em contraste, o **PRTB possui a menor média de idade, 43 anos**, o que pode sinalizar uma tentativa de atrair uma base mais jovem e progressista.



# Ferramentas Utilizadas
[![NumPy](https://img.shields.io/badge/NumPy-1.24.2-blue.svg)](https://numpy.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0.3-blue.svg)](https://pandas.pydata.org/)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.7.1-blue.svg)](https://matplotlib.org/)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.12.2-blue.svg)](https://seaborn.pydata.org/)
[![Requests](https://img.shields.io/badge/Requests-2.31.0-blue.svg)](https://docs.python-requests.org/en/master/)
[![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-2.0.10-blue.svg)](https://www.sqlalchemy.org/)
[![SQLite](https://img.shields.io/badge/SQLite-3.41.2-blue.svg)](https://www.sqlite.org/)







