
# Tech Challenge — Fase 2
## Análise de crédito e classificação de atrasos

**FIAP Pós-Tech | Machine Learning**

## Equipe

- Denise Figueira
- Luana Souza de Oliveira
- Possidonio Pereira da Fonseca Neto
- Sheila da Silva Conceição Feitosa

## 1. Contexto e objetivo

A concessão de crédito exige a avaliação de riscos associados ao comportamento de pagamento dos clientes.

Este projeto investiga a utilização de técnicas de aprendizado de máquina para identificar padrões associados à ocorrência de atrasos de 60 dias ou mais no histórico de crédito disponível.

O objetivo é desenvolver, comparar e avaliar modelos de classificação, identificando seu potencial, suas limitações e as condições necessárias para uma futura aplicação operacional.

**Delimitação:** as bases não informam diretamente se um pedido de cartão foi aprovado. Portanto, o projeto utiliza um indicador de atraso construído a partir do histórico de crédito, sem afirmar que os modelos preveem diretamente a aprovação de solicitações ou a inadimplência futura.

## 2. Bases de dados

Foram utilizadas duas bases:

- `application_record.csv`: informações cadastrais, pessoais e financeiras dos clientes.
- `credit_record.csv`: registros mensais do histórico de crédito.

**Fonte:** [Base disponibilizada no Tech Challenge — Fase 2](https://drive.google.com/file/d/1z4yEyiCE_CGCWbvAAZQZSz-5E5T5eYd/view)

A população principal contém **36.457 clientes** presentes em ambas as bases.

### Variável-alvo

- **TARGET = 1:** pelo menos um registro com `STATUS` entre 2 e 5, representando atraso de 60 dias ou mais.
- **TARGET = 0:** nenhum registro dessa condição no histórico disponível.

| Classificação | Clientes | Percentual |
|---|---:|---:|
| TARGET = 0 | 35.841 | 98,31% |
| TARGET = 1 | 616 | 1,69% |
| **Total** | **36.457** | **100%** |

A ausência de atraso no histórico observado não garante ausência de eventos futuros.

## 3. Metodologia

O projeto contempla:

1. Análise exploratória, distribuições, correlações, valores ausentes e outliers.
2. Construção e justificativa da variável-alvo.
3. Preparação dos dados com imputação, codificação e padronização em pipelines.
4. Comparação de algoritmos supervisionados.
5. Validação considerando perfis cadastrais idênticos.
6. Avaliação de métricas, limiares e desbalanceamento.
7. Análise de sensibilidade com janela comum de 12 meses.
8. Clusterização exploratória de clientes.

### Algoritmos avaliados

- Regressão Logística
- Árvore de Decisão
- Random Forest
- Gradient Boosting

As avaliações utilizaram métricas como ROC AUC, Average Precision, precisão, recall e F1-score.

## 4. Principais resultados

Na análise principal, a regressão logística apresentou a maior Average Precision média na comparação por grupos de perfis cadastrais idênticos.

A validação por grupos mostrou que a estratégia de separação dos dados influencia a avaliação dos modelos, especialmente Random Forest e Gradient Boosting.

O ajuste do limiar permitiu investigar diferentes relações entre identificação de eventos positivos e falsos positivos, mas a sensibilidade permaneceu limitada.

### Análise complementar — janela de 12 meses

Foi construída uma base com **17.238 clientes** observados durante os mesmos 12 meses.

A regressão logística sem sobreamostragem foi selecionada para avaliação nesse cenário.

| Indicador | Resultado |
|---|---:|
| ROC AUC | 0,6529 |
| Average Precision | 0,0859 |
| Limiar | 0,05 |
| Precisão | 21,43% |
| Recall | 11,11% |
| Verdadeiros positivos | 3 |
| Falsos positivos | 11 |
| Falsos negativos | 24 |
| Verdadeiros negativos | 3.421 |

O modelo identificou 3 dos 27 clientes com evento positivo no conjunto de teste.

A clusterização também identificou dois segmentos com diferenças demográficas e financeiras, mas sem separação consistente das proporções de atraso entre treino e teste.

## 5. Avaliação de viabilidade e recomendação executiva

O projeto permitiu desenvolver e comparar modelos de classificação, investigar características dos dados e identificar fatores que limitam sua utilização.

Apesar de apresentar alguma capacidade de discriminação, **o desempenho observado não sustenta a implantação do modelo para decisões automáticas de concessão de crédito**.

As principais limitações identificadas foram:

- Forte desbalanceamento das classes.
- Repetição de perfis cadastrais.
- Diferenças no tempo de histórico disponível.
- Quantidade reduzida de eventos positivos.
- Ausência de datas cadastrais que permitam estabelecer a sequência temporal entre os atributos e os eventos de atraso.

### Recomendação

**Não implantar o modelo neste estágio.**

Para avançar em direção a uma solução operacional, recomenda-se estruturar dados cadastrais datados, estabelecer uma data de corte anterior ao período de desempenho, realizar validação prospectiva e independente e avaliar os custos de falsos positivos e falsos negativos.

O principal resultado do projeto é a identificação das condições necessárias para o desenvolvimento de uma solução de análise de crédito mais confiável e tecnicamente defensável.

## 6. Estrutura do repositório

```text
tech-challenge-fase2-credito/
├── notebooks/
│   ├── Cópia_de_tech_challenge_credito.ipynb
│   └── README.md
├── requirements.txt
└── README.md
```

O notebook único reúne todas as etapas do projeto.

A estrutura será complementada com os materiais de apresentação e submissão.

## 7. Como reproduzir o projeto

1. Abrir `notebooks/Cópia_de_tech_challenge_credito.ipynb` no Google Colab.
2. Disponibilizar na sessão o arquivo original `BaseDadosFase2.zip`, contendo `application_record.csv` e `credit_record.csv`.
3. Conferir o nome do ZIP utilizado na célula de carregamento.
4. Executar as células sequencialmente, do início ao fim.

O notebook utiliza `RANDOM_STATE = 42` para favorecer a reprodutibilidade.

O ambiente de referência utilizou **Python 3.13.16**. As dependências estão registradas em `requirements.txt`.

As bases originais não são disponibilizadas neste repositório; devem ser obtidas pelo link indicado na seção 2.

## 8. Apresentação e entrega

- **Repositório GitHub:** este repositório.
- **Apresentação executiva (PDF):** [Visualizar apresentação em PDF](docs/Tech_Challenge_Fase2_Apresentacao_Executiva.pdf).
- **Apresentação executiva (PowerPoint):** [Baixar apresentação em PowerPoint](docs/Tech_Challenge_Fase2_Apresentacao_Executiva.pptx).
- **Vídeo de apresentação:** [Assistir no YouTube](https://youtu.be/FXhZSxd_XNw).

A apresentação gerencial tem duração de 4 minutos e 41 segundos, respeitando o limite máximo de cinco minutos previsto nas orientações do Tech Challenge.
