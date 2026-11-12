# 🚀 Transformers com Competição Estocástica para Dados Tabulares

Os dados tabulares são essenciais em diversos setores, mas ainda recebem pouca atenção no deep learning, onde modelos como GBDT (Gradient Boosted Decision Trees) dominam. Porém, novas arquiteturas vêm mudando esse cenário, superando o GBDT em várias situações. É aí que entra o modelo proposto nesse artigo: um Transformer estocástico feito para dados tabulares, com:

- **Unidades Local Winner Takes All (LWTA):** Mais generalização com estocasticidade e esparsidade.
- **Camada de Embedding com Competição Estocástica:** Escolha dinâmica entre embeddings lineares.

O modelo foi testado em bases públicas e teve resultados competitivos!

Caso tenham alguma dúvida pra executar esse script com a base de dados do projeto, podem consultar, principalmente, as duas fontes abaixo:

[Link para o artigo](https://arxiv.org/pdf/2407.13238)

[Link para o código fonte original](https://github.com/avoskou/Transformers-with-Stochastic-Competition-for-Tabular-Data-Modelling)

**Nota dos pesquisadores:** O código ainda está em versão experimental.

### 📂 Estrutura do Projeto
- **Model:** A pasta "STab" contém todos os arquivos relacionados ao modelo.
- **train_tutorial:** Notebook Jupyter com exemplo de treinamento para o dataset "Iris". A estrutura segue praticamente a mesma com o novo dataset!


### 📦 Dependências
Este projeto utiliza **torch** e **keras4torch** para treinamento e avaliação, além de componentes das seguintes implementações:
- [FtTransformer PyTorch](https://github.com/lucidrains/tab-transformer-pytorch)
- [Stochastic Transformer Networks](https://github.com/avoskou/Stochastic-Transformer-Networks-with-Linear-Competing-Units-Application-to-end-to-end-SL-Translation)
- [Keras 4 Torch](https://github.com/blueloveTH/keras4torch)
