<h2>Passo a passo com a MobilenetV2 utilizando TPUs no Google Colab</h2>

- Este repositório é destinado ao armazenamento de um passo a passo para utilizar a MobilenetV2 com a API de detecção de objetos do Tensorflow, rodando o treinamento utilizando as TPUs disponbilizadas no Google Colab.
- Está dividido em um diretório contendo o dataset utilizado no passo a passo e dois arquivos no formato do jupyter notebook(ipynb), onde um dos aquivos é destinado ao treinamento do modelo ```MobilenetV2_TPU_Train``` e o outro ao uso do modelo já treinado ```MobilenetV2_Predict```. 
- A escolha por separar em dois arquivos diferentes foi por poder utilizar os dois ao mesmo tempo em instâncias diferentes de maquinas oferecidas pelo Colab, visto que para realizar inferências utilizando o modelo treinado não é prático utilizando TPUs, assim acaba sendo necessário fazer a troca para o uso de GPU, o que acaba gerando uma perda das configurações já realizadas para o ambiente com uso de TPU.
- O dataset presente neste repositório pode ser encontrado nesse link https://www.kaggle.com/alvarole/asirra-cats-vs-dogs-object-detection-dataset e está disponível de forma pública. 
- Optei por disponibilizar o dataset aqui por ter separado de uma forma diferente os dados e por disponibilizar junto os arquivos de conversão de ```xml``` para ```csv``` e de ```csv``` para o formato ```record``` utilizado pelo Tensorflow, além disso também disponibilizei junto um txt contendo o labelmap dentro da pasta ```dataset/data```. Fora essa adições, nenhum rótulo ou imagem foi alterado, assim como não foram adicionadas imagens novas, mantendo a integridade original dos dados.

<h2>Passos iniciais</h2>

- Antes de mais nada, é necessário criar uma conta no ```Google Cloud```. Após criada a conta e estar logado na plataforma é necessário acessar o console do ```Google Cloud``` e criar um novo projeto, para isso é possível seguir este tutorial https://cloud.google.com/resource-manager/docs/creating-managing-projects. Com o projeto criado, vá no menu de navegação e selecione a opção de faturamento, fique tranquilo, para utilizar os ```buckets```, que é o que queremos, não é gerado nenhum custo, porém é necessário vincular uma conta de faturamento.
- Com a conta de faturamento já ativada, vamos criar um ```bucket```, para isso é possível seguir este tutorial disponibilizado pelo Google: https://cloud.google.com/storage/docs/creating-buckets.
> Obs: É necessário realizar a criação e uso de buckets, pois é necessário os arquivos que vamos utilizar estarem disponiveis em um bucket para podermos utilizar as TPUs disponiveis no Colab.

