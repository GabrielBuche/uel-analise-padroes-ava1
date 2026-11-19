# Registro do projeto

## Entradas

- **2026-11-19** — Criado `README.md` com objetivo, estrutura real do repositório, preparação do ambiente, guia dos notebooks, decisões metodológicas e resultados de referência.

- **2026-11-19** — Revisada a declaração final de uso de IA no enunciado, incluindo as sugestões de complementação textual feitas durante a sessão.

- **2026-11-19** — Revisada apenas a célula de respostas do enunciado. Organizadas as etapas, indicadas as seções correspondentes do notebook de resultados e marcados os complementos de interpretação para revisão do aluno.

- **2026-11-19** — Concluídos os experimentos das etapas 7.2 a 7.4. Rodadas 60 avaliações variando só a semente das dobras, com floresta fixa. Em K = 9, a acurácia variou de 99,50% a 99,75%, com média de 99,725%, e superou média/moda em 0,65 p.p. em média. Na escala original, `wbcc`, `bgr` e `bu` somaram 93,29% da contribuição média à distância quadrática. Incluídas a comparação com referências da Tabela 11 e as estatísticas dos valores observados por classe e imputados em `ckd`.

- **2026-11-19** — Revisado o notebook completo contra o enunciado e executadas todas as células no `.venv`. Com K = 9 e as dobras usadas na comparação, as 400 previsões coincidiram antes e depois de restringir a imputação ao treino. Entre as 158 amostras completas, 115 são `notckd` e 43 são `ckd`. Para a etapa 7.2, é preciso distinguir a repetição da Tabela 10, que varia as duas sementes, do experimento que varia apenas a semente das dobras.

- **2026-11-19** — Replicada a coluna RF das Tabelas 5 e 10 na posição da célula 123. Na Tabela 5, os cinco K deram 99,75%; média e moda deram 99,00%; o sorteio com reposição entre valores observados da coluna deu 98,35% em cinco rodadas. Na Tabela 10, as médias de dez rodadas com as sementes 0 a 9 coincidiram com o artigo nas quatro métricas para K = 9, a duas casas decimais.

- **2026-11-19** — Comparada a imputação antes da validação com a imputação e padronização baseadas somente no treino de cada dobra. Mantidos K = 9, os 24 preditores, as mesmas quatro dobras e a mesma configuração da random forest. As duas avaliações deram 99,75% de acurácia, 249 VP, 0 FP, 1 FN e 150 VN.

- **2026-11-19** — Adicionadas as contagens da matriz de confusão na validação de quatro dobras com K = 9 e `ckd` como classe positiva. As três configurações de preditores tiveram 249 VP, 0 FP, 1 FN e 150 VN.

- **2026-11-19** — Reproduzida a análise da Figura 2 com K = 9, imputação padronizada e 500 árvores no scikit-learn. Só `cad` teve importância Gini igual a zero. A avaliação com quatro dobras deu 99,75% e um erro com todos os preditores, sem `cad` e sem a lista publicada (`pcc`, `ba`, `cad`). As importâncias são normalizadas no scikit-learn.

- **2026-11-19** — Comparado o método do artigo com `KNNImputer` para os cinco valores de K, usando a mesma padronização das numéricas e pesos uniformes no scikit-learn. Com K = 9, 935 de 1.012 preenchimentos diferiram, e o scikit-learn gerou 180 valores fracionários nas colunas binárias. A comparação e os gráficos estão no notebook.

- **2026-11-19** — Adicionadas as duas versões da imputação ao notebook. A nova versão padroniza apenas os preditores numéricos para calcular distâncias, e o preenchimento usa os valores originais dos vizinhos. Para K de 3, 5, 7, 9 e 11, as versões diferem em 707, 674, 666, 662 e 675 células imputadas, respectivamente. Todas as tabelas ficaram sem faltantes e preservaram os valores observados.

- **2026-11-19** — Conferida a imputação contra a seção II-B e o código do `DMwR` 0.4.1. O notebook usa escalas originais; `knnImputation` padroniza as colunas numéricas por padrão. A chamada exata dos autores não foi identificada no artigo. Em teste de sensibilidade em memória para `K = 9`, padronizar as numéricas na busca alterou 662 valores imputados.

- **2026-11-19** — Adicionada a tabela comparativa dos percentuais de faltantes antes da codificação e da imputação. Os 25 percentuais calculados coincidem com a Tabela 1 do artigo.

- **2026-11-19** — Substituído o teste avulso por imputação KNN para `K = 3, 5, 7, 9, 11`, com 158 doadores completos, distância euclidiana sobre os preditores observados nas escalas originais, mediana para numéricas e moda para categóricas. A implementação em pandas usa a ordem das linhas do ARFF para desempatar vizinhos à mesma distância. Os cinco DataFrames ficaram sem faltantes, com preservação de todos os valores observados.

- **2026-11-19** — Conferida a preparação antes da imputação. Os 25 percentuais de faltantes coincidem com a Tabela 1, há 158 registros completos e a codificação não cria novos faltantes. A verificação usou `encoded_df.dtypes` em memória, pois `encoded_df.dtypes()` gera `TypeError` no notebook.

- **2026-11-19** — Convertidas `bp`, `al`, `su`, `bgr`, `pcv` e `wbcc` para `Int64` na célula de codificação. A execução confirmou a preservação dos valores e dos faltantes.

- **2026-11-19** — Convertida `age` para o tipo inteiro anulável `Int64` após confirmar que as idades observadas são inteiras. Os nove valores faltantes foram preservados, e os preditores binários já usavam `Int64`.

- **2026-11-19** — Organizada a célula de codificação. Os mapeamentos binários e a conversão de `sg`, `al` e `su` são aplicados a `encoded_df` antes da separação em `X` e `y`. Verificadas as 24 colunas numéricas de `X` e as contagens de faltantes.

- **2026-11-19** — Incluída no notebook a Tabela 1 de Qin et al. com os percentuais de referência e os mapeamentos da seção II-B.

- **2026-11-19** — Simplificada a leitura no notebook, usando `@data` para dividir o arquivo em dois blocos. Mantido o caminho relativo escolhido no notebook. Verificado no `.venv` que o DataFrame é idêntico ao da leitura anterior.

- **2026-11-19** — Implementada a leitura do ARFF bruto em `codigo/resultado.ipynb` com `csv.reader` e pandas. A leitura remove dois campos finais vazios e interpreta o campo vazio interno da linha 515 como excedente, alinhando os valores seguintes com as categorias do cabeçalho. Verificados 400 registros, 25 colunas e as contagens de classe 250 `ckd` e 150 `notckd` no `.venv`.

- **2026-11-19** — A inspeção do bloco após `@data` encontrou 397 registros com 25 campos e três com 26. As linhas 215 e 218 têm vírgula final, e a linha 515 tem um campo vazio interno cuja interpretação ainda precisa ser validada.

- **2026-11-19** — Diagnosticado o `ValueError` do SciPy ao ler o ARFF bruto. O primeiro erro reproduzido ocorre na linha 176, onde `dm` contém ` yes` com espaço inicial e o cabeçalho declara `{yes,no}`.

- **2026-11-19** — Formatação inicial de `codigo/resultado.ipynb`. A anotação sobre a leitura do ARFF passou para uma célula Markdown, com o caminho corrigido conforme o arquivo presente no projeto.
