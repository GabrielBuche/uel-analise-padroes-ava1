# Replicação de Qin et al. (2020) — Doença Renal Crônica

Projeto da atividade AVA 1 da disciplina de Análise de Padrões, do Mestrado em Ciência da Computação da Universidade Estadual de Londrina, no segundo semestre de 2026.

**Autor — Gabriel Buche**

O objetivo é reproduzir e analisar criticamente o pré-processamento e a avaliação da random forest apresentados em *A Machine Learning Methodology for Diagnosing Chronic Kidney Disease*, de Qin et al. (2020). O trabalho utiliza Python, pandas e scikit-learn. O estudo original foi implementado em R.

## Escopo

A replicação segue as seções II e III-A do artigo, incluindo a importância das variáveis da Figura 2, calculada com a base imputada com K = 9. O roteiro da atividade contempla leitura, limpeza, codificação, imputação KNN, classificação e análise crítica da avaliação.

O conjunto de dados contém 400 pacientes, 24 preditores e a variável-alvo `class`. Há 250 pacientes com DRC (`ckd`) e 150 sem DRC (`notckd`).

## Organização do repositório

```text
.
├── README.md
├── artigo/
│   ├── A_Machine_Learning_Methodology_for_Diagnosing_Chronic_Kidney_Disease.pdf
│   └── Traducao_DRC_Qin2020.pdf
├── codigo/
│   ├── RF_desafio_DRC_enunciado.ipynb
│   └── resultado.ipynb
├── dados/
│   ├── Chronic_Kidney_Disease/
│   │   ├── chronic_kidney_disease_full.arff
│   │   ├── chronic_kidney_disease.arff
│   │   └── chronic_kidney_disease.info.txt
│   ├── Chronic_Kidney_Disease.rar
│   └── chronic+kidney+disease.zip
└── doc/
    └── registro.md
```

- **`artigo/`** contém o artigo original e a tradução para consulta.
- **`codigo/resultado.ipynb`** reúne o código, os gráficos, as tabelas e as observações dos experimentos.
- **`codigo/RF_desafio_DRC_enunciado.ipynb`** contém o enunciado do professor e uma seção de respostas ao final.
- **`dados/`** armazena os arquivos do conjunto de dados. A leitura usa `chronic_kidney_disease_full.arff`.
- **`doc/registro.md`** registra decisões, adaptações e verificações realizadas durante o projeto.

Os arquivos brutos são preservados. As correções de leitura e a imputação são aplicadas em memória.

## Preparação do ambiente

O projeto foi desenvolvido com Python 3.14. Na raiz do repositório, crie e ative um ambiente virtual. Os comandos abaixo são para Linux ou macOS.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install pandas numpy scipy scikit-learn matplotlib jupyterlab ipykernel
```

Para abrir os notebooks no JupyterLab, execute a partir da raiz do projeto.

```bash
python -m jupyter lab
```

Abra `codigo/resultado.ipynb` e selecione o kernel do ambiente `.venv`. No VS Code, selecione esse mesmo interpretador no menu de kernels do notebook.

O caminho relativo dos dados pressupõe que o diretório de trabalho do kernel seja `codigo/`. Confira com `%pwd` em uma célula se houver erro ao localizar o ARFF. Execute as células em ordem, preferencialmente reiniciando o kernel antes de uma execução completa. As avaliações repetidas da random forest podem levar alguns minutos.

## Guia de leitura do notebook

1. **Leitura e limpeza.** O cabeçalho do ARFF fornece os nomes e tipos das colunas. O bloco depois de `@data` contém os registros. A leitura remove espaços e trata três linhas com um campo excedente. O símbolo `?` é convertido em valor ausente.
2. **Conferência dos faltantes.** A tabela compara o percentual calculado em cada coluna com a Tabela 1 do artigo.
3. **Codificação.** As categorias são mapeadas para 0 e 1. Os valores de `sg`, `al` e `su` são convertidos para números, e a classe é separada dos preditores.
4. **Imputação.** O método utiliza apenas pacientes completos como candidatos a vizinhos e preenche os faltantes com mediana nas numéricas e moda nas categóricas. Os experimentos usam K = 3, 5, 7, 9 e 11.
5. **Comparação com o scikit-learn.** O `KNNImputer` é comparado com o método descrito no artigo. Tabelas e gráficos mostram os preenchimentos diferentes e os valores fracionários produzidos em colunas binárias.
6. **Random forest.** Uma floresta de 500 árvores é avaliada com quatro dobras estratificadas. A classe positiva é `ckd`. São calculados acurácia, sensibilidade, especificidade, F1 e matriz de confusão.
7. **Importância das variáveis.** O gráfico usa a redução de impureza Gini. A avaliação compara todos os preditores com conjuntos reduzidos.
8. **Comparações com o artigo.** As tabelas apresentam os resultados publicados e os da replicação da random forest nas Tabelas 5 e 10.
9. **Análise crítica.** Os experimentos investigam o momento da imputação, a variação entre divisões das dobras, as escalas dos preditores e a composição dos pacientes usados como vizinhos.

Para acompanhar as respostas escritas, consulte a seção “Respostas” do notebook do enunciado. Cada item indica a seção correspondente em `resultado.ipynb`.

## Decisões da replicação

### Escala na imputação

Foram implementadas duas versões, uma nas escalas originais e outra com padronização dos preditores numéricos. Na versão padronizada, a média e o desvio-padrão ajustam as distâncias. A mediana e a moda são calculadas com os valores originais dos vizinhos.

O artigo descreve distância euclidiana e cita o pacote `DMwR`, mas não explicita o parâmetro de padronização da imputação. A versão padronizada foi adotada como hipótese para a avaliação principal, e a comparação das escalas está documentada.

### Avaliação e vazamento de informação

A primeira avaliação imputa a base inteira antes de dividir as dobras, seguindo a ordem descrita no artigo. Uma segunda avaliação, com K = 9, calcula a padronização e seleciona os candidatos a vizinhos exclusivamente no treino de cada dobra. Essa referência do treino é usada para preencher tanto o treino quanto o teste.

A replicação da Tabela 10 varia as sementes das dobras e da floresta. O experimento da etapa 7.2 varia somente a semente das dobras, mantendo a floresta fixa.

### Diferenças de implementação

O scikit-learn apresenta importâncias Gini normalizadas. As implementações de random forest e os geradores aleatórios de Python e R podem produzir resultados diferentes, mesmo com a mesma semente numérica. Empates entre vizinhos são resolvidos pela ordem das linhas do ARFF.

Na comparação com imputação aleatória, o preenchimento sorteia valores observados da própria coluna, com reposição. Essa é uma decisão da replicação, pois a distribuição do sorteio não está detalhada no artigo.

## Resultados de referência da execução

| Verificação | Resultado |
| --- | --- |
| Percentuais de faltantes | Coincidiram com a Tabela 1 nas 25 colunas |
| Pacientes completos | 158, sendo 43 `ckd` e 115 `notckd` |
| RF com K = 9 e divisão fixa | 99,75% de acurácia, 249 VP, 0 FP, 1 FN e 150 VN |
| Imputação dentro das mesmas dobras, K = 9 | Mesmas 400 previsões da avaliação anterior |
| Variação apenas das dobras, K = 9 | Acurácia de 99,50% a 99,75% em dez rodadas |
| Importância Gini exatamente zero | Apenas `cad` na execução principal |
| Escalas originais nas distâncias | `wbcc`, `bgr` e `bu` somaram aproximadamente 93,29% da contribuição média à distância quadrática |

As tabelas completas e os protocolos de cada comparação estão no notebook. Os resultados se referem ao conjunto de dados e às configurações avaliadas neste estudo acadêmico.

## Uso de inteligência artificial

O ChatGPT auxiliou na geração de código, na organização e revisão dos textos e em sugestões de complementação. A declaração de uso e responsabilidade do autor está ao final do notebook do enunciado.

## Referências

- Qin, J. et al. *A Machine Learning Methodology for Diagnosing Chronic Kidney Disease*. IEEE Access, v. 8, p. 20991–21002, 2020. [DOI do artigo](https://doi.org/10.1109/ACCESS.2019.2963053).
- UCI Machine Learning Repository. *Chronic Kidney Disease*. [Página do conjunto de dados](https://archive.ics.uci.edu/dataset/336/chronic+kidney+disease).
- scikit-learn. [Documentação do KNNImputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.KNNImputer.html).
- scikit-learn. [Documentação do RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html).
