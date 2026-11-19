# AGENTS.md

This file provides guidance to Codex (claude.ai/code) when working with code in this repository.

# Atividade AVA 1 — Replicação de Qin et al. (2020) como estudo de caso

Disciplina: Análise de Padrões — Mestrado em Ciência da Computação (UEL), 2º sem. 2026.
Artigo base: *A Machine Learning Methodology for Diagnosing Chronic Kidney Disease*, IEEE Access, 2020 (DOI 10.1109/ACCESS.2019.2963053).

## Papel do Codex neste projeto

- **Parceiro de raciocínio, não executor.** O trabalho é do aluno. Ajudar a pensar: fazer perguntas, apontar caminhos, explicar conceitos, questionar decisões, revisar o raciocínio.
- **Não escrever o código nem o texto da atividade** a menos que o aluno peça explicitamente. Preferir dicas, pseudocódigo curto ou perguntas que levem à resposta.
- Português do Brasil, tom simples porém acadêmico. Fundamentar afirmações com referências (artigos, livros, documentação oficial).

## Enunciado e escopo

`codigo/RF_desafio_DRC_enunciado.ipynb` é o **enunciado do professor** (não é código do aluno).
- Escopo: seções II e III-A do artigo, foco na **random forest**, mais a importância das variáveis (Figura 2, K = 9).
- Fora do escopo: optimal subset regression, outros classificadores e modelo integrado (III-B, III-C, IV).
- As etapas 1–8 do enunciado guiam o roteiro; a etapa 7 (análise da metodologia) é o núcleo crítico.

## Estrutura

```
artigo/                           artigo original (EN) e tradução PT-BR (Traducao_DRC_Qin2020.pdf)
doc/claude_translate_en_br.md     contexto técnico detalhado: dados, pré-processamento,
                                  modelos, hiperparâmetros, resultados de referência,
                                  pontos para a análise crítica
doc/registro.md                   diário do projeto: o que foi feito, decisões, pendências
codigo/  dados/  resultados/      trabalho de replicação do aluno
```

## Dados

- **Fonte principal (exigida pelo enunciado):** `dados/arff_original/Chronic_Kidney_Disease/chronic_kidney_disease_full.arff`, extraído de `dados/Chronic_Kidney_Disease.rar` (idêntico ao `.rar` dentro de `dados/chronic+kidney+disease.zip`). A pasta também tem `chronic_kidney_disease.arff` e `.info.txt`.
- `dados/ckd_ucimlrepo.csv`: cópia do `ucimlrepo` (id 336), só como referência — ele já altera os dados (`?` → NaN; `sg`/`al`/`su` numéricas).
- Arquivos brutos (`.zip`, `.rar`, `.arff`, `.csv`) não devem ser sobrescritos; limpezas geram arquivos novos.

## Stack e desvios do artigo

- Python + scikit-learn (o artigo usa R).
- Peças do R sem equivalente direto no sklearn (KNN-imputation com mediana/moda do `DMwR`) são adaptadas — **todo desvio do artigo deve ser registrado** em `doc/registro.md`, pois alimenta a análise crítica.
- Ambiente: `.venv/` (Python 3.14), dependências em `requirements.txt` → `source .venv/bin/activate` ou `.venv/bin/python <script>`. Recriar: `python3 -m venv .venv && .venv/bin/pip install -r requirements.txt`. O venv **não** tem `jupyter`/`ipykernel`; instalar se o trabalho for em notebook.
- `codigo/resultado.ipynb`: selecione o kernel do `.venv` com pandas e execute as células em ordem. O caminho relativo dos dados parte de `ava_1/codigo/`.

## Como usar o contexto

- **Não ler os arquivos de `doc/` nem o PDF por padrão.** Consultar só quando a conversa exigir (ex.: um hiperparâmetro, um número de tabela, uma decisão anterior).
  - Detalhe técnico do artigo → `doc/claude_translate_en_br.md` (seções 4–6) ou o PDF.
  - "O que já fizemos / o que decidimos?" → `doc/registro.md`.
- **Ao fim de uma etapa relevante** (decisão tomada, etapa concluída, dúvida resolvida), adicionar uma entrada curta em `doc/registro.md`, no topo da seção "Entradas". Perguntar ao aluno se não estiver claro o que registrar.
